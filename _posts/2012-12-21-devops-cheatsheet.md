---
layout: post
title: "DevOps Cheatsheet"
category: 
tags: []
---
{% include JB/setup %}

###List all installed packages on Ubuntu or Debina
    dpkg --get-selections

###Install a package with RPM
    rpm -ivh foobar.rpm  # install, verbose, use hash tags to mark progress

###Install a package with Alien
    alien -i foobar.rpm
