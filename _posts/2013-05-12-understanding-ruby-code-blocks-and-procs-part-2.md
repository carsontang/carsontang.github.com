---
layout: post
title: "Understanding Ruby Code Blocks and Procs (Part 2)"
category: ruby
tags: [ruby]
---
{% include JB/setup %}
### The relationship between code blocks and Procs
One thing to keep clear is a code block is not a Proc and a Proc is not a code block.
They are different. Here's how you convert a code block into a Proc
{% highlight ruby %}
def turn_into_proc(&code_block)
  code_block
end

obj = turn_into_proc { puts "I will be converted into a Proc." }
=> #<Proc:0x007fea1b9c0f88@(irb):1>

obj.class
=> Proc
{% endhighlight %}
