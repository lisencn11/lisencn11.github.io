---
layout: page
title: "生活"
description: "知之真切笃实处即是行，行之明觉精察处即是知 "
header-img: "img/zhihu.jpg"
---


<ul class="listing">
{% for tag in site.tags %}
  {% if tag[0] == "life" %}
    {% for post in tag[1] %}
      <li class="listing-item">
        <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
        <a href="{{ post.url }}" title="{{ post.tile }}">{{ post.title }}</a>
    {% endfor %}
  {% endif %}
{% endfor %}