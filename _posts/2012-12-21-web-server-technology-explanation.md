---
layout: post
title: "Web server technology explanation"
description: ""
category: 
tags: []
---
{% include JB/setup %}

##Common Gateway Interface
The Common Gateway Interface is a standard method for web server software to delegate the generation of web content to executable files. These files are known as CGI scripts usually written in
a scripting language.

###CGI Drawback
The problem with CGI is that calling an executable generally means the invocation of a newly created process
on the server. Thus, more time might be spent starting the process than generating the output. Alternatives
came about to address this speed and memory drawback.

###Alternative 1: Embed the langue interpreter inside the web server
One alternative involves embedding language interpreters inside the web server. Apache web servers allow
third-party software to run inside of them via Apache modules. One such module is mod_ruby, which, as its name suggests, embeds the Ruby interpreter inside the Apache web server to interpret, surprise surprise, Ruby.

###Alternative 2: Use a persistent process to evaluate requests
CGI spawns a new process per request. FastCGI addresses this speed problem by using a persistent process to
handle multiple requests. These processes are owned by the FastCGI server. Separating the application processes
from the web server process can be very helpful. The application and the web server can be restarted
separately, which is helpful for busy websites.

###Useful Links
  [Fantastic Stack Overflow answer explaining how the entire stack works](http://stackoverflow.com/questions/4113299/ruby-on-rails-server-options)
  [Apache Modules](http://en.wikipedia.org/wiki/Apache_modules)

  [Common Gateway Interface](http://en.wikipedia.org/wiki/Common_Gateway_Interface)
  
  [FastCGI](http://en.wikipedia.org/wiki/FastCGI)

  [Phusion Passenger](http://www.modrails.com/documentation/Users%20guide%20Standalone.html)