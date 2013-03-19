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

For example, let's say you put a Python script on your computer. On your computer, you also start up
a web server such as Apache. This means your computer can now satisfy web requests! Whenever your computer
receives a request, it'll look at the path the request contains. If the path is a path to your Python script, it's because of CGI that your computer can execute that script and return its output as
a response to the computer that made the request.

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

###Running CGI scripts on Mac OS 10.8
1) Create an Apache configuration file

    $ sudo vim /etc/apache2/users/`whoami`.conf
    <Directory "/Library/WebServer/CGI-Executables">
        Options Indexes MultiViews
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>

2) Change the permissions of the configuration file so that
visitors can access them.

    $ sudo chown root:wheel /etc/apache2/users/*
    $ sudo chmod 644 /etc/apache2/users/*

3) Put an executable Python script in the cgi-bin.

    $ cd /Library/WebServer/CGI-Executables
    $ vim script.py
    #! /usr/bin/python
    print 'Content-Type: text/html'
    print
    print """\
    <html>
    <body>
    <h1>Hello World!</h1>
    </body>
    </html>
    """

4) The following is a Ruby CGI script:

    #! /usr/bin/ruby                                                                                             
    require 'cgi'
    cgi = CGI.new
    puts cgi.header
    puts "<html><body>This is a test</body></html>"

###Useful Links

  [Fantastic Stack Overflow answer explaining how the entire stack works](http://stackoverflow.com/questions/4113299/ruby-on-rails-server-options)
  
  [Apache Modules](http://en.wikipedia.org/wiki/Apache_modules)

  [Common Gateway Interface](http://en.wikipedia.org/wiki/Common_Gateway_Interface)

  [Another CGI explanation](http://www.tutorialspoint.com/perl/perl_cgi.htm)

  [Ruby CGI explanation](http://www.tutorialspoint.com/ruby/ruby_web_applications.htm)
  
  [FastCGI](http://en.wikipedia.org/wiki/FastCGI)

  [Phusion Passenger](http://www.modrails.com/documentation/Users%20guide%20Standalone.html)
