---
layout: page
title: "Tags"
description: "Let it be, live with heart, and that is the life winner."  
header-img: "img/green.jpg"  
---

<div id='tag_cloud'>
{% for tag in site.tags %}
<a href="#{{ tag[0] }}" title="{{ tag[0] }}" rel="{{ tag[1].size }}">{{ tag[0] }}</a><br>
{% endfor %}
</div>

<hr>

<div id="tag_posts">
<ul class="listing">
{% for tag in site.tags %}
  <li class="listing-seperator" id="{{ tag[0] }}">{{ tag[0] }}</li>
<ul class="listing">
{% for post in tag[1] %}
  <li class="listing-item">
  <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
  <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endfor %}
</ul>
{% endfor %}
</ul>
</div>


