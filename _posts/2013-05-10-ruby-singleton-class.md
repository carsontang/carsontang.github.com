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

### Special singleton methods: class methods
As referred to earlier, a class method is a singleton method defined on an object of Class.
Just a refresher, an object of Class is just a regular Ruby class. Class methods are almost
the same as singleton methods. However, they have an additional property. Normally, the only
object that can call the singleton method is the object the singleton method was defined on.
A class method can be called by the class on which it's defined. It can _also_ be called by
other objects that fit a special criterion: those objects are subclasses of the class on
which the singleton method was defined.

{% highlight ruby %}
class Watch
  attr_reader :serial_number

  def initialize(sn)
    @serial_number = sn
  end

  class << self
    def valid?(serial_number)
      serial_number % 2 == 0
    end
  end
end

class Rolex < Watch; end

Rolex.new(2).valid?
=> true
{% endhighlight %}

This allowed because singleton classes of class objects are ancestors of singleton classes
of descending class objects.
