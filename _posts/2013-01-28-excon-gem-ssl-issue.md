---
layout: post
title: "Excon gem SSL issue
category: Ruby
tags: [Ruby, Rails]
---
{% include JB/setup %}

I got this issue while trying to get CarrierWave to upload pictures to S3 from
an EC2 instance:
    ERROR: Excon::Errors::SocketError: Unable to verify certificate, 
    please set Excon.defaults[:ssl_ca_path] = path_to_certs,
    Excon.defaults[:ssl_ca_file] = path_to_file, or Excon.defaults[:ssl_verify_peer] = false
