---
layout: post
title: "Quick Guide to RVM"
category: Ruby
tags: [tutorial, Ruby]
---
{% include JB/setup %}

####What is RVM?
RVM stands for Ruby Version Manager.

####Why use RVM?
When you start making a ton of websites with Ruby and Rails, you'll have a lot
of versions of Ruby and Rails. For example, your honor society's website might have been
built with Ruby 1.9.2/Rails 3.2.0. Your club sports team's website might have been built
with Ruby 1.9.1/Rails 3.1.0. If you don't have RVM on your computer, you can
only have ONE version of Ruby and ONE version of Rails on your computer.
With RVM, you can have MULTIPLE versions of Ruby/Rails.

####How to install Ruby 1.9.2 on your computer
    rvm install 1.9.2

####How to create and use a new gemset named "blog" for Ruby 1.9.2
    rvm --create use 1.9.2@blog

####How to list all the gemsets managed by RVM
    rvm gemset list

####How to list all the versions of Ruby managed by RVM
    rvm list

####How to delete a gemset test@jruby-1.6.5
    rvm gemset delete test@jruby-1.6.5

####How to use JRuby 1.6.5
    rvm use jruby-1.6.5