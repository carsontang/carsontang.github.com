---
layout: post
title: "Quick Guide to cURL"
category: command line
tags: [command line]
---
{% include JB/setup %}

####GET request
    curl http://localhost:8000/tweets

####POST to server
    curl -X POST -d tweet=hello%20world http://localhost:8000/tweets
