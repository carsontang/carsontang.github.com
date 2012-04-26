---
layout: post
title: "Amazon AWS Services Descriptions"
description: ""
category: AWS
tags: [AWS]
---
{% include JB/setup %}

#### Elastic Block Store (EBS)
EBS volumes are storage not associated with one instance. Thus, they can be attached to
any instance you launch.

#### Elastic IP Addresses
Elastic IP addresses are static IP addresses. What's great about them is they aren't
associated with only one Amazon instance. They can be mapped to any Amazon instance you
control. Why is this useful? Say one of your instances goes down. You can immediately remap
the elastic IP address to another instance that serves the same contents. Users will still
be able to access the service provided by the instance that went down.