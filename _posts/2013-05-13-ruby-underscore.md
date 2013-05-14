---
layout: post
title: "Ruby Underscore"
category: ruby
tags: [ruby]
---
{% include JB/setup %}

I was browsing through the source code for Unicorn when I noticed this piece of
code
{% highlight ruby %}
mapping.each { |hostname,_,_,_| hostnames[hostname] = true }
{% endhighlight %}

The underscores caught my attention. Based on my experience with other programming
languages, I assumed the underscore was there for unused variables.

According to Ruby syntax, a variable can be given the identifier `_`. Unlike
regular identifiers, however, the Ruby parser only allows the `_` identifier
to be duplicated. If you try an identifier like `foo`, a SyntaxError will be raised.