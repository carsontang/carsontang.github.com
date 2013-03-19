---
layout: post
title: "CSS cheatsheet"
category: "CSS"
tags: ["CSS"]
---
{% include JB/setup %}

### Include an internal style sheet
{% highlight html %}
<style type="text/css">
  h1 {
    background-color: blue;
  }
</style>
{% endhighlight %}
### Link a style sheet
{% highlight html %}
<link rel="stylesheet" type="text/css" href="css/main.css" />
{% endhighlight %}
### div v.s. span
A div allows you to group several block-level elements. A span lets you apply a class or ID style to part
of a tag.

### Style a group of selectors
{% highlight css %}
h1, h2, h3 { font-weight: bold; }
p, .photo, #banner { color: #eee; }
{% endhighlight %}
### Universal selector (asterisk)
{% highlight css %}
// Style EVERYTHING with the universal selector
* { font-weight: bold; }

// Style all descendants of .photo
.photo * { background-color: red; }
{% endhighlight %}
### Descendant selectors
{% highlight css %}
.container h2 { color: yellow; }

// Apply this style to every link that is a descendant of a paragraph with the explanation class
p.explanation a { color: green; }

// Apply this style to every link that is a descendant of any tag
// styled with the explanation class that is a descendant of a paragraph tag
p .explanation a { color: green; }
{% endhighlight %}
### Child selectors
{% highlight css %}
.container > p { text-decoration: underline; }
{% endhighlight %}
### Adjacent siblings selectors
{% highlight css %}
h1 + p { color: red; } // the paragraph tag after the h1 tags must be red
h1 + p + p { color: blue; } // the paragraph tag after the paragraph tag after the h1 tags must be red
{% endhighlight %}
### Attribute selectors
{% highlight css %}
input[type="text"] { background-color: black; }
{% endhighlight %}
### Pseudo-classes
{% highlight css %}
a:link { background-color: red; } // any link the user has not visited yet
a:visited { color: purple; } // a link the user has clicked before
a:hover { text-decoration: underline; } // a link the user is hovering over
{% endhighlight %}