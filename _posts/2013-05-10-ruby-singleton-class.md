---
layout: post
title: "Ruby Singleton Class"
category: ruby
tags: [ruby]
---
{% include JB/setup %}

### What's a singleton method?
A singleton method defined on a given object can only be called by that
object.

{% highlight ruby %}
name = "Matz"

def name.ruby_creator?
  true
end
{% endhighlight %}

In the code above, an instance of String is created, name, and a singleton
method, ruby_creator?, is defined. The only instance of String that can call
that method is name.

### Location of singleton methods
While instance methods live inside a class or module, singleton methods live inside
an object's singleton class.

### How to access a singleton class
To access an object's singleton class, assuming the object's identifier is name,
use the following syntax
{% highlight ruby %}
name = "Matz"

class << name
  def ruby_creator?
    true
  end
end
{% endhighlight %}

The code above is an alternate way to define singleton methods. So, you can either
define singleton methods with the dot operator following an object, or you can
open up its singleton class and define singleton methods within it.

### Real world usage
The most common usage of singleton classes is defining class methods. The following four ways
are equivalent:
{% highlight ruby %}
# using a singleton class to define class methods
class BasketballPlayer
  class << self
    def tallest(*players)
      players.max_by(&:height)
    end
  end
end

# defining a class method on self
class BasketballPlayer
  def self.tallest(*players)
    players.max_by(&:height)
  end
end

# defining a singleton method on the BasketballPlayer object, an instance of Class
class BasketballPlayer; end

def BasketballPlayer.tallest(*players)
  players.max_by(&:height)
end


# opening up the singleton class after the Class instance has been created
class BasketballPlayer; end

class << BasketballPlayer
  def tallest(*players)
    players.max_by(&:height)
  end
end
{% endhighlight %}