---
layout: post
title: "Dependency Injection Explanation"
category: 
tags: [design, testing]
---
{% include JB/setup %}

### Definition
<br />
The best definition of dependency injection that I found online is by James Shore:

    "Dependency injection means giving an object its instance variables. Really. That's it."

### Example
<br />
{% highlight ruby %}
class Computer
  def initialize
    @graphics_card = Nvidia::GeForce.new
  end

  def display_graphics
    @graphics_card.generate_images
  end
end
{% endhighlight %}

In the example above, there are unnecessary dependencies.

An instance of a Computer must use an Nvidia graphics card. What if my future computers
use AMD graphics cards? This will require me to change the name of the graphics card
I use in my Computer code. Making instances of Computer depend on a certain graphics card
also reduces the code's reusability.

To fix this, we'll use dependency injection.

{% highlight ruby %}
class Computer
  def initialize(gfx_card)
    @graphics_card = gfx_card
  end

  def display_graphics
    @graphics_card.generate_images
  end
end
{% endhighlight %}

See how simple the change was? All I did was, as James Shore would put it, give an instance
of Computer its `@graphics_card` instance variable. Now I can swap out Nvidia graphics
cards with AMD graphics cards easily.

{% highlight ruby %}
# nvidia/geforce.rb
module Nvidia
  class GeForce
    def generate_images
      puts "Generating images from a GeForce"
    end
  end
end

# amd/radeon.rb
module AMD
  class Radeon
    def generate_images
      puts "Generating images from a Radeon"
    end
  end
end

# gaming_pub.rb
computer1 = Computer.new(AMD::Radeon.new)
computer2 = Computer.new(Nvidia::GeForce.new)

computer1.display_graphics
=> "Generating images from a Radeon"

computer2.display_graphics
=> "Generating images from a GeForce"
{% endhighlight %}

Because of duck typing, Computer's `display_graphics` instance method doesn't care if
the graphics card is an instance of Nvidia::GeForce or AMD::Radeon. As long as the object
responds to `display_graphics messages`, any will do.

Furthermore, dependency injection allows me to test the graphics card that my computer uses.
For example, when an instance of Computer calls `display_graphics`, I want to ensure `generate_images` was
called on `@graphics_card` and not some integrated graphics card.

{% highlight ruby %}
# computer_test.rb

def test_should_generate_images_when_displaying_images
  geforce = mock()
  geforce.expects(:generate_images)
  computer = Computer.new(geforce)
  computer.display_graphics
end
{% endhighlight %}

### References
[Dependency Injection Demystified](http://www.jamesshore.com/Blog/Dependency-Injection-Demystified.html)