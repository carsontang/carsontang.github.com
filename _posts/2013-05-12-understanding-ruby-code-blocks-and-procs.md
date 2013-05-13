---
layout: post
title: "Understanding Ruby Code Blocks and Procs (Part 1)"
category: ruby
tags: [ruby]
---
{% include JB/setup %}

### Goal
The goal of this series of articles is to understand how the following code
works.
{% highlight ruby %}
["ruby", "rails"].map(&:upcase)
=> ["RUBY", "RAILS"]
{% endhighlight %}

To understand this code, we have to look at code blocks and Procs.

### What is a code block
A code block is an optional part of a method invocation.
{% highlight ruby %}
1.upto(10) { |x| puts x }
{% endhighlight %}
In the code above, the code block is the code following `1.upto(10)` that is
in between the curly braces. It is  important to note that a code block is
not standalone. It _must_ follow a method invocation. However, a method 
invocation does not necessarily need to have a code block.

As seen above, a code block is delimited by curly braces. It can also be
delimited by the keywords `do` and `end` if it's a multiline code block.
{% highlight ruby %}
1.upto(10) do |x|
  puts x 
end
{% endhighlight %}

### How a method calls a code block
Even if you tack on a code block to a method invocation, how does the method
actually call the code within the code block? Look at the following example.
{% highlight ruby %}
def call_code_block
  yield
end

call_code_block { puts "Calling code in the code block." }
=> Calling code in the code block.
{% endhighlight %}

As you can see, the keyword `yield` calls the code block. What if a code block
isn't given? In that case, the Kernel module provides a block_given? method
that returns true if a code block is given. So we can modify our previous code:
{% highlight ruby %}
def call_code_block
  yield if block_given?
end
{% endhighlight %}

As you noticed earlier on, code blocks can take arguments. Simply pass the arguments
to the `yield` keyword, and they'll be passed to the code block. Armed with
this knowledge, it's not too hard to write our own version of the `each`
method that classes that include the Enumerable module can call.

{% highlight ruby %}
class Array
  def my_each
    i = 0
    while i < self.size
      yield(self[i])
      i += 1
    end
    self
  end
end

[1,2,3].my_each { |x| puts x }
1
2
3
 => [1, 2, 3] # It works!
{% endhighlight %}

### What is a Proc?
A lot of things in Ruby are objects, but code blocks are not. What if you want
to store a code block somewhere and pass it around from method to method? Well,
fortunately with Ruby, you _can_ with a Proc object. An instance of Proc is simply
an object that represents a code block. You're familiar with code blocks. They
look like the following:
{% highlight ruby %}
{ "I am a code block." }
{% endhighlight %}

Remember, though, a code block _cannot_ stand alone. It must follow a method
invocation. The code above is just to remind you what a code block looks like.
To store a code block in an object, it must be the code block of a Proc instance.

{% highlight ruby %}
code_block_object = Proc.new { "I am a code block. I am also the code block of a Proc instance, and I'm stored within that instance." }
{% endhighlight %}

In methods, we have the ability to call a code block with the `yield` keyword.
How do we call the code block stored within a Proc object then? Simple:
use the `call` method on that object.

{% highlight ruby %}
code_block_object.call
=> "I am a code block. I am also the code block of a Proc instance, and I'm stored within that instance."
{% endhighlight %}

The code block within `code_block_object` was called, and it returned a string because
the code block was simply a string. If that is confusing, you can think of code blocks
supplied to Proc.new in a different way. The code block supplied to Proc.new becomes the
body of a method, which just so happens to be placed in a Proc instance. Let's look
at an example to clear this up.
{% highlight ruby %}
def print_important_message
  puts "Hello world!" # <--- body of method
end

print_important_message
Hello world!
=> nil

print_important_message_proc = Proc.new { puts "Hello world!" }
# the code block following Proc.new is the same as the body of the previous method

print_important_message_proc.call
Hello world!
=> nil
{% endhighlight %}

All right, that might have been a lot to take in, so let's stop for a bit and continue this with [part 2 of this series]({% post_url 2013-05-12-understanding-ruby-code-blocks-and-procs-part-2 %}).