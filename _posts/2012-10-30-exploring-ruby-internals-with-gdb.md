---
layout: post
title: "Exploring Ruby Internals with GDB"
category: Ruby
tags: [Ruby, GDB]
---
{% include JB/setup %}

##Setup
###Install autoconf 2.68
Thanks to Jean-Sebastien Delfino for posting on [his blog](http://jsdelfino.blogspot.com/2012/08/autoconf-and-automake-on-mac-os-x.html) how to set this up on Mac OS X Mountain Lion

    $ mkdir -p ~/devtools
    $ cd ~/devtools
    $ curl -OL http://ftpmirror.gnu.org/autoconf/autoconf-2.68.tar.gz
    $ tar xzf autoconf-2.68.tar.gz
    $ cd autoconf-2.68
    $ ./configure --prefix=/Users/ken/devtools/autotools-bin
    $ make
    $ make install
    $ export PATH=$PATH:/Users/ken/devtools/autotools-bin/bin

###Grab latest Ruby source code
    $ git clone git://github.com/ruby/ruby.git

###Build and install Ruby
    $ cd ruby
    $ autoconf # no configure file available
    $ ./configure --prefix=/Users/ken/devtools/ruby-bin
    $ make
    $ make install

###Explore Ruby internals
Thanks to Howard Yeh for posting on [his blog](http://metacircus.com/hacking/2010/04/11/exploring-ruby-internals.html) tips that got me started.
 
Terminal 1

    $ /Users/ken/devtools/ruby-bin/irb
    1.9.3-p286 :001 > Process.pid
     => 69983

Terminal 2

    $ cgdb /Users/ken/devtools/ruby-bin/ruby 69983 # using cgdb, not gdb; same pid as one obtained in irb
    $ (gdb) set args -e 'puts "hello world"'
    $ (gdb) break main
    $ (gdb) run

In this terminal, I attach cgdb to the Ruby process I want to debug, and then
I set the arguments I want to feed into the ruby executable to see what happens
when 'puts "hello world"' is interpreted. If you can't set breakpoints, use "file" to
associate C source code with the ruby executable. Use "step" in gdb to step into
functions and "next" in gdb to skip over functions.

###Useful articles
* [CGDB guide](http://cgdb.sourceforge.net/docs/cgdb-no-split.html#Understanding-CGDB)

* [GDB Cheatsheet](http://darkdust.net/files/GDB%20Cheat%20Sheet.pdf)

* [GDB Quickstart](http://web.eecs.umich.edu/~sugih/pointers/gdbQS.html)

* [Introduction to GNU build system](http://airs.com/ian/configure/configure_1.html)

* [No symbol table loaded](http://stackoverflow.com/questions/9245685/gdb-no-symbol-table-is-loaded)

* [Understanding software installation: configure, make, make install](http://www.codecoffee.com/tipsforlinux/articles/27.html)
