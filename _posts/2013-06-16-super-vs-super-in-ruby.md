---
layout: post
title: "super vs super() in Ruby"
category: Ruby
tags: [Ruby]
---
{% include JB/setup %}

Here is some Ruby code I wrote the other day (shortened for the purpose of this article)

{% highlight ruby %}

class A
  attr_reader :stack

  def initialize
    @stack = []
  end
end

class B < A
  attr_reader :name

  def initialize(args={})
    super
    @name = args.fetch(:name, nil)
  end
end

{% endhighlight %}

When I ran the following code, I got an ArgumentError

{% highlight ruby %}
b = B.new({:name => 'Ruby'})

ArgumentError: wrong number of arguments (1 for 0)
{% endhighlight %}

The error complained that I was passing in 1 argument for the initialize method
in class A. The reason the ArgumentError appears is super in the subclass will take
with it any parameters that were sent to the initialize method. To avoid sending
the parameters, call super with parentheses. Thus, the initialize method for class
B should be

{% highlight ruby %}
def initialize(args={})
  super()
  @name = args.fetch(:name, nil)
end
{% endhighlight %}