---
layout: page
title: "Blog"
description: "Experience is Writing, Writing is Experience."
header-img: "img/blue.jpg"
---

If these help you, please donate via the AliPay code below.

(*<img src="img/alipay.jpg"  height="400"  />*)
<img src="img/alipay.jpg"  height="400" width="300" />

---

<ul class="listing">
{% for post in site.posts %}
  {% capture y %}{{post.date | date:"%Y"}}{% endcapture %}
  {% if year != y %}
    {% assign year = y %}
    <li class="listing-seperator">{{ y }}</li>
  {% endif %}
  <li class="listing-item">
    <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
    <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endfor %}
</ul>



