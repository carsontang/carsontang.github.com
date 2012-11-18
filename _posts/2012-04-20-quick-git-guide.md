---
layout: post
title: Quick Git Guide
category: git
tags: [git, tutorial, version control]
---
{% include JB/setup %}

### What is Git?
Git is a distributed version control system.
<br />
<br />
### Common Git commands
**git init** -- creates a Git repository in the current directory

**git add .** -- adds all files in the current directory to the Git repository

**git commit** -- commits current files to the Git repository

**git commit --amend -m "Whoops. Used a wrong commit message last time."** -- Revise a commit message with a new one.
<br />
<br />
### Common workflow
Let's go through an example workflow to see how the commands listed above are used:

    $ cd ~
    $ mkdir kinect_code
    $ cd kinect_code
    $ git init
    Initialized empty Git repository in /Users/ken/kinect_code/.git/

All I've done so far is 1) create a directory `~/kinect_code` and 2) initialize a Git
repository within `~/kinect_code`.

    $ echo "This is the README" > README
    $ touch .gitignore
    $ ls -al
    drwxr-xr-x   4 ken  staff   136 Apr 20 15:30 .
    drwxr-xr-x+ 62 ken  staff  2108 Apr 20 15:30 ..
    drwxr-xr-x  13 ken  staff   442 Apr 20 15:30 .git
    -rw-r--r--   1 ken  staff     0 Apr 20 15:30 .gitignore
    -rw-r--r--   1 ken  staff    19 Apr 20 15:30 README

At this point, we have created a Git repository within the `~/kinect_code`
directory, and we've created two files, `README` and `.gitignore`. However,
the **git status** command shows that the Git repository is not tracking
the two files.

    $ git status
    # On branch master
    #
    # Initial commit
    #
    # Untracked files:
    #   (use "git add <file>..." to include in what will be committed)
    #
    #	.gitignore
    #	README
    nothing added to commit but untracked files present (use "git add" to track)

To start tracking these files, use the **git add** commmand:

    $ git add README
    $ git commit -m "Initial commit; added README to the Git repository."

    [master (root-commit) 8ef6c17] Initial commit; added README to the Git repository.
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 README

The following situation involves undoing an accidental commit with **git revert HEAD**:

    $ git rm *
    rm 'README'
    $ git commit -m "Removed everything"
    [master 9f31df6] Removed everything
    0 files changed
    delete mode 100644 README
    $ git revert HEAD
    <add comment to revert>
    [master 2cca6ca] Revert "Removed everything"
    0 files changed
    create mode 100644 README

###Git concepts
* `HEAD` points to the tip of the current branch you're on.

###Avoid having to type username and password every single git-push
    $ git config credential.helper cache
    $ git push http://example.com/test.git
    Username: <type your username>
    Password: <type your password>

    [work for 5 minutes]
    git push http://example.com/test.git
    [your credentials are used automatically]

    # default timeout of credential cache is 900 seconds, or 15 minutes
    # exit credential cache early
    $ git credential-cache exit

###Useful git-log examples
    $ git log --pretty=oneline --max-count=2
    $ git log --pretty=oneline --author="Carson Tang"
    $ git log --pretty=oneline --since="5 minutes ago"
