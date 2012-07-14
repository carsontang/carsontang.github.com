---
layout: page
---
{% include JB/setup %}

Quick guides and cheatsheets that act as a reference for me. Mainly for myself, but I hope they help you out, too.

### Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

