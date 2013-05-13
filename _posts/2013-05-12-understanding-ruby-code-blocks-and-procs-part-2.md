---
layout: post
title: "Understanding Ruby Code Blocks and Procs (Part 2)"
category: ruby
tags: [ruby]
---
{% include JB/setup %}

### Code block and Proc recap
A method invocation can include up to five parts: the receiver, the dot operator,
the method name, the arguments, and the code block. 
{% highlight ruby %}
[1,2,3].each { |x| x**2 }
1
4
9
=> [1,2,3]

{% endhighlight %}

In the method invocation above, the code block was the code delimited by the curly braces.

Code blocks are not objects, but they can be stored in objects. These special objects
are instances of the Proc class. The following is one way to store a code block
in a Proc instance.
{% highlight ruby %}
code_block_object = Proc.new { puts "I will turn into a Proc object." }
{% endhighlight %}


### The relationship between code blocks and Procs
One thing to keep clear is a code block is not a Proc and a Proc is not a code block.
They are different. Here's how you convert a code block into a Proc
{% highlight ruby %}
def turn_into_proc(&code_block)
  code_block
end

obj = turn_into_proc { puts "code block" }
=> #<Proc:0x007fea1b9c0f88@(irb):1>

obj.class
=> Proc
{% endhighlight %}

In the code above, you can imagine the following steps taking place:

`turn_into_proc` is invoked with a code block:
{% highlight ruby %}
turn_into_proc { puts "code block" }
{% endhighlight %}

The code block becomes the code block of `Proc.new`
{% highlight ruby %}
Proc.new { puts "code block" }
{% endhighlight %}

The new Proc instance is stored in `code_block`.
{% highlight ruby %}
code_block = Proc.new { puts "code block" }
{% endhighlight %}

How does `turn_into_proc` know that `code_block` should be a Proc object
and not a regular argument? The & operator. The ampersand tells the parser to
look at the method invocation's code block and convert it into a Proc object.

### How to convert a Proc object into a code block.
Just as the & operator is used to convert a code block into a Proc object, it can
also be used to convert a Proc object into a code block.
{% highlight ruby %}
# print the square of each number in the array with a code block
[1,2,3].each { |x| puts x**2 }

# print the square of each number in the array
# with a Proc acting as a code block
each_code_block = Proc.new { |x| puts x**2 }
[1,2,3].each(&each_code_block)
{% endhighlight %}

### What is to_proc?
When using a Proc object to stand in for a code block, the & operator does
two things:
1. Calls the Proc object's `to_proc` method.
2. Informs the parser that the resulting Proc object (after `to_proc` has been
called) is acting as a code block for the method invocation.

Let's imagine what this looks like in code form:
{% highlight ruby %}
each_code_block = Proc.new { |x| puts x**2 }

# 1) & operator used
[1,2,3].each(&each_code_block)

# 2)
each_code_block.to_proc

# 3) & informs parser that each_code_block is a Proc object standing in for a code block
{% endhighlight %}

### Understanding our initial piece of code
Finally, we've arrived at the point where we can understand how the code above works.

In the previous section, we learned that the & operator calls `to_proc` on the
object that is standing in for a code block. We've been working with Proc objects
standing in for code blocks, but what if the object were a Symbol? If it's a Symbol,
Symbol#to_proc is called. Symbol#to_proc can be implemented like this:
{% highlight ruby %}
class Symbol
  def to_proc
    Proc.new { |obj| obj.send(self) }
  end
end
{% endhighlight %}

So, with that piece of code, we can now see how to understand our initial
piece of code.
{% highlight ruby %}
["ruby", "rails"].map(&:upcase)

# & operator seen, call to_proc on the object
:upcase.to_proc

# Proc object is returned
Proc.new { |obj| obj.send(:upcase) }

# Proc object acts as code block for map
["ruby", "rails"].map { |obj| obj.send(:upcase) }
{% endhighlight %}

The code above makes perfect sense! It took a long explanation to arrive at this
point, but it was worth the wait.