---
layout: page
title: "学习"
description: "知之真切笃实处即是行，行之明觉精察处即是知。"
header-img: "img/orange.jpg"
---


<ul class="listing">
{% for tag in site.tags %}
  {% if tag[0] == "study" %}
    {% for post in tag[1] %}
      <li class="listing-item">
        <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
        <a href="{{ post.url }}" title="{{ post.tile }}">{{ post.title }}</a>
      </li>
    {% endfor %}
  {% endif %}
{% endfor %}
</ul>