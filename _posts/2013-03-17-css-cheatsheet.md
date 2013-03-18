---
layout: post
title: "CSS cheatsheet"
category: "CSS"
tags: ["CSS"]
---
{% include JB/setup %}

### Link a style sheet
    <link rel="stylesheet" type="text/css" href="css/main.css" />

### div v.s. span
A div allows you to group several block-level elements. A span lets you apply a class or ID style to part
of a tag.

### Style a group of selectors
    h1, h2, h3 { font-weight: bold; }
    p, .photo, #banner { color: #eee; }

### Universal selector (asterisk)
    // Style EVERYTHING with the universal selector
    * { font-weight: bold; }

    // Style all descendants of .photo
    .photo * { background-color: red; }

### Descendant selectors
    .container h2 { color: yellow; }

    // Apply this style to every link that is a descendant of a paragraph with the explanation class
    p.explanation a { color: green; }

    // Apply this style to every link that is a descendant of any tag
    // styled with the explanation class that is a descendant of a paragraph tag
    p .explanation a { color: green; }

### Child selectors
    .container > p { text-decoration: underline; }

### Adjacent siblings selectors
    h1 + p { color: red; } // the paragraph tag after the h1 tags must be red
    h1 + p + p { color: blue; } // the paragraph tag after the paragraph tag after the h1 tags must be red

### Attribute selectors
    input[type="text"] { background-color: black; }
### Pseudo-classes
    a:link { background-color: red; } // any link the user has not visited yet
    a:visited { color: purple; } // a link the user has clicked before
    a:hover { text-decoration: underline; } // a link the user is hovering over
