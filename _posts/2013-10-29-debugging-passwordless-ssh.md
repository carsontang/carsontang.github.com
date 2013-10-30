---
layout: post
title: "Debugging passwordless SSH"
category: SSH
tags: [SSH]
---
{% include JB/setup %}

I encountered some issues setting up passwordless SSH, which frustrated me. However, it was a blessing in disguise
because it forced me to learn more about how SSH works. By the end of my research, I had a deeper understanding
of SSH.

The goal of this guide will be to set up SSH so that I can ssh into a remote machine without typing in a
passphrase multiple times.

### Generate an SSH key.
The command generates a pair of RSA keys: public and private. The comment is useful for identifying
public keys. The default names of the keys will be id_rsa.pub and id_rsa for the public and private
keys, respectively, so there is no need to append the last option. I wrote that for clarity.

    $ ssh-keygen -t rsa -C "test@example.com" -f ~/.ssh/id_rsa

### Add a passphrase
The generation of the public and private keys will prompt you for a passphrase. Note, this shouldn't
be a passWORD, but rather, a passPHRASE, like "An apple a day keeps the doctor away" but more secure.

### Add public key to remote machine's authorized_keys
Now that you have a public/private key pair, you must add it to a list. This list is in a file called
authorized_keys, and it's located on the remote machine you want to passwordless-ssh into. For example,
let's say you've set up a user account called user on a remote server located at lindbrook.test.edu.
There are two ways you can add your public key to user@lindbrook.test.edu's authorized_keys

    $ ssh-copy-id -i ~/.ssh/id_rsa.pub user@lindbrook.test.edu

OR

    $ cat ~/.ssh/id_rsa.pub | ssh user@lindbrook.test.edu "mkdir ~/.ssh; cat >> ~/.ssh/authorized_keys"

The first way should be used if ssh-copy-id is available as a command. If not, the second will suffice.
It's important to note that the PUBLIC key must be added to authorized_keys. And it's important to note that
either of the two commands must be run on the LOCAL machine. Remember, your goal is to ssh into a REMOTE
machine from your LOCAL machine.

### Set up ssh-agent
If you've gotten past the previous steps, you can ssh into a remote machine without having to type in the user's
password on the remote machine. However, if you've set up a passphrase for your public/private key pair, you
still have to enter that every time you ssh into that remote machine. That's annoying. Wasn't the whole point of
this to avoid having to enter passwords/passphrases?

To remedy this situation, you have to set up ssh-agent. ssh-agent is a program that will remember the decrypted
private key. To decrypt the private key, you must know the passphrase. All you have to do is start up ssh-agent,
and add the private key to it after decrypting it with a passphrase. ssh-agent can then be used for proving
to the remote machine that you are indeed the owner of the private key that belongs to the public key listed
in the remote machine's authorized_keys.

If you run ssh-agent, it will display some environment variables and their corresponding values that you should
set up so that ssh knows how to communicate with it.

    $ ssh-agent
    SSH_AUTH_SOCK=/var/folders/s8/dndsygmn64x9s244c0hqhsm40000gn/T//ssh-GqaFgVE74tem/agent.1437; export SSH_AUTH_SOCK;
    SSH_AGENT_PID=1438; export SSH_AGENT_PID;
    echo Agent pid 1438;

You can set these up manually, but since these are conveniently commands we should run directly to have the
environment variables set up, you can run the following command

    $ eval `ssh-agent`

Next, add your private key to the ssh-agent.

    $ ssh-add ~/.ssh/id_rsa
    # Enter your passphrase

### Passwordless SSH
Now you can ssh into your remote machine without having to enter a password/passphrase as many times as you want.
It's important to note that the passphrase must be entered every time you start up ssh-agent again.

### Passwordless SSH not working
Here are some reasons why passwordless SSH isn't working

1) Ensure the permissions of several directories on the remote machine are correct. sshd, the daemon running on the
remote machine, won't work properly if the permissions aren't correct.

    $ chmod go-w $HOME $HOME/.ssh
    $ chmod 600 $HOME/.ssh/authorized_keys
    $ chown `whoami` $HOME/.ssh/authorized_keys

2) Ensure you're running ssh-agent on your local machine, otherwise you'll always be prompted for the private key's
passphrase.

3) Check your local machine's ~/.ssh/config file.

    Host lindbrook
        User user
        HostName lindbrook.test.edu
        IdentifyFile ~/.ssh/id_rsa
        IdentitiesOnly yes

If the identity file listed after IdentityFile does not match the private key whose public key you've added to
the remote machine's authorized_keys list, then whenever you ssh to the remote machine, you'll use the wrong private
key. IdentitiesOnly specifies that ssh will only use the identity file listed in ~/.ssh/config.

4) If you're still not having any luck, try ssh in verbose mode to output more info.

    $ ssh -vvv user@lindbrook.test.edu

5) If all else fails, generate a new private/public key, and follow the steps of this guide. I once did this, and it
fixed my problems.
