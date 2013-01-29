---
layout: post
title: "Excon gem SSL issue"
category: Ruby
tags: [Ruby, Rails]
---
{% include JB/setup %}

I got this issue while trying to get CarrierWave to upload pictures to S3 from
an EC2 instance:

    ERROR: Excon::Errors::SocketError: Unable to verify certificate, 
    please set Excon.defaults[:ssl_ca_path] = path_to_certs,
    Excon.defaults[:ssl_ca_file] = path_to_file, or
    Excon.defaults[:ssl_verify_peer] = false

This problem came about when an action in a controller wasn't working properly.
The most frustrating part of solving this issue was a 500 Internal Server Error
was recorded in the logs with no explanation! To fix this, I went into
`app/config/environments/production.rb` and made the following change:

    config.consider_all_requests_local       = true

By setting `config.consider_all_requests_local = true`, the logs finally reported
the previously mentioned Excon error.

I booted my EC2 instance with a custom AMI that I had created. It turns out my custom
AMI didn't have any CA certificates, so it was necessary to run the following commands:

    $ sudo update-ca-certificates
    $ sudo apt-get remove -y --force-yes --purge ec2-ami-tools
    $ sudo apt-get install -y --force-yes ec2-ami-tools

The first command is a Debian/Ubuntu command to update the CA certificates. The second
two commands remove and reinstall the ec2-ami-tools to retrieve the CA certificates for
Amazon.

Finally, I went into my Rails console, and used the following command to determine the
location of the Excon gem it was using.

    > $:.grep(/excon/)
    ["/path/to/excon/gem"]

I changed into `/path/to/excon/gem` and modified `excon/ssl_socket.rb`. Specifically,
I modified the params hash so that the Excon gem could find the CA certificates.

    # create ssl context
    ssl_context = OpenSSL::SSL::SSLContext.new

    params[:ssl_ca_path] = '/etc/ssl/certs' # <--- Line I added
    if params[:ssl_verify_peer]
    ...

This change allowed CarrierWave to upload pictures to S3 successfully because now my EC2
instance could verify it was communicating with S3. I also could have modified params
in a different way:

    params[:ssl_verify_peer] = false

This tells Excon to not verify its peer, which is not secure, so I stuck with my first solution.