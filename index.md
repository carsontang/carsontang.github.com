---
layout: page
---
{% include JB/setup %}

Guides I've written over the past year. Mainly for myself, but I hope they help you out, too.

### Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

