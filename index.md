---
layout: page
---
{% include JB/setup %}

Follow me on Twitter [@carsontang](http://www.twitter.com/carsontang)
    
### Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


