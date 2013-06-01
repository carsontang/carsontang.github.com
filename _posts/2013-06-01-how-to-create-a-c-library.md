---
layout: post
title: "How to create a C library"
category: unix
tags: [unix, c]
---
{% include JB/setup %}

### What's an archive?
An archive is a collection of relocatable object files. Archive filenames end with the .a suffix.

### Step-by-step guide on how to create an archive (C library)
First, write the header and implementation files for the C library.

{% highlight c %}
/* math.h */
#ifndef MATH_H
#define MATH_H
int add(int a, int b);
#endif

/* math.c */
#include "math.h"
int add(int a, int b)
{
  return a + b;
}
{% endhighlight %}


Next, create a relocatable object file from the implementation files.

    unix > gcc -c math.c -o math.o
The `-c` option for gcc ensures math.c is preprocessed, compiled into assembly, assembled into machine code,
but not linked with other object files. Simply put, an object file is produced.

After, create the archive file.

    unix > ar rcs libmath.a math.o

If you want to install the library so that all programs can used it on your system,
put the header file in /usr/local/include and the archive file in /usr/local/lib.

    unix > chmod 0644 math.h libmath.a
    unix > chmod 0755 /usr/local/include
    unix > chmod 0755 /usr/local/lib
    unix > mv math.h /usr/local/include
    unix > mv libmath.a /usr/local/lib