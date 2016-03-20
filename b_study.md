---
layout: page
title: "学习"
description: "你看到的，是我所有的文章"
header-img: "img/orange.jpg"
---


<ul class="listing">
{% for tag in site.tags %}
  {% if tag[0] == "study" %}
    {% for post in tag[1] %}
      <li class="listing-item">
        <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
        <a href="{{ post.url }}" title="{{ post.tile }}">{{ post.title }}</a>
    {% endfor %}
  {% endif %}
{% endfor %}


</ul>