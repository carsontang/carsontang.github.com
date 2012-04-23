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

### Interesting links
<ul>
	<li>23 April 2012 » <a href="http://www.poets.org/viewmedia.php/prmMID/15403">"anyone lived in a pretty how town" by e.e. cummings</a></li>
	<li>23 April 2012 » <a href="http://vim-adventures.com">Learn VIM while playing a game</a></li>
</ul>

