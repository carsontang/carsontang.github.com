---
layout: post
title: "Deploying your Rails app to Amazon EC2 with the Rubber gem"
category:
tags: [Rails, deployment, Ruby]
---
{% include JB/setup %}

### Introduction to Rubber
<br/>
Rubber is a Capistrano plugin that makes it easy to deploy Ruby on Rails applications to
Amazon's Elastic Compute Cloud.

To get started, determine the HTTP server, the app server, and the database you'll want to
use when running your Rails app in production. You can view the list of supported stacks
[here](https://github.com/rubber/rubber/tree/master/templates).

### Rubber Stack Templates
<br />
Rubber already provides some templates for common stacks like complete_passenger_nginx_postgresql,
complete_passenger_mysql (which includes Apache as the HTTP server), and complete_unicorn_nginx_postgresql.

You can also create your own stack. To do this, look at a template you like, and use its components
to build your own stack. For example, in complete_unicorn_nginx_postgresql, you'll see
the templates.yml file. Open it and you'll see
it's based on two templates: complete_unicorn_nginx and postgresl. The complete_unicorn_nginx
template itself is based on the following templates: base, nginx, unicorn, monit, collectd, and graphite. Monit, collectd, and graphite aren't necessary for production, but they do make monitoring server
statuses and site data much easier.

The absolute minimum stack for production must include an HTTP server, an application server, and
a database (unless your Rails app doesn't use a database at all). Thus, the following templates
will suffice:
* base
* nginx
* unicorn
* postgresql

### Installing Rubber in your Rails app
<br />
To install Rubber in your Rails app, go to the Gemfile and add
{% highlight ruby %}
gem 'rubber'
{% endhighlight %}
Do a bundle install to download and install the gem. Next, go to the root of your Rails app
and install all the templates you've chosen for your app.
{% highlight ruby %}
> rubber vulcanize base
> rubber vulcanize nginx
> rubber vulcanize unicorn
> rubber vulcanize postgresql
{% endhighlight %}
As you can see, I've chosen to custom tailor the stack. If you're just starting out with Rubber,
and your primary goal is to put your Rails app in prod, you can use one of the complete stack templates like so
{% highlight ruby %}
> rubber vulcanize complete_unicorn_nginx
{% endhighlight %}
For my own Rails apps, I tend to stay away from the complete templates that Rubber provides by default
because I don't want to use collectd and graphite.

### Configuring Rubber
Now that we've installed Rubber in our Rails app, it's time to configure it. The first file to look at
is config/rubber/rubber.yml
{% highlight ruby %}
app_name: supercoolrailsapp
app_user: supercoolrailsapp
timezone: US/Pacific
domain: supercoolrailsapp.com
cloud_providers:
  aws:
    region: us-west-1
    key_name: supercoolrailsapp-keypair
    image_type: m1.small
    image_id: ami-42b39007 # Ubuntu 12.10 Quantal instance-store
{% endhighlight %}

I've left out the useful comments that rubber.yml is generated with, but it's generally a good idea
to keep them to help you navigate and understand each configuration. Most of the configuration is
pretty self-explanatory, so I won't go into much detail here.

As you can see, I've named the app supercoolrailsapp with app_name. This name doesn't
have to be the name you selected for your app when you first created your Rails app. The app_user
can also be any name you want. I got the image_id (an AMI id) from [Alestic](http://www.alestic.com).
Make sure you choose an AMI from the AWS region you've specified; otherwise, your deployment will fail.