---
layout: post
title: "JavaScript Prototypes Short Explanation"
category: JavaScript 
tags: [JavaScript]
---
{% include JB/setup %}

I'm finding it difficult to understand fundamentally how prototypes work in JavaScript,
so after reading [Yehuda Katz's 'Understanding "Prototypes" in JavaScript'](http://yehudakatz.com/2011/08/12/understanding-prototypes-in-javascript/),
I'm going to attempt to describe prototypes with as few words as possible in my own words
to solidify my understanding of the concept.

All objects in JavaScript have a property called "prototype". That property is a pointer to
another object. Whenever a property is being searched for in an object, if that property is not
found in that object, the object's prototype will be searched. The object's prototype's prototype
will be searched as well if the property is still not found. Looking for a property continues
until no more prototypes are left.