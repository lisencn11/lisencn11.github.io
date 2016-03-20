---
layout: page
title: "我的收藏"
description: "文章本天成，妙手偶得之。"  
header-img: "img/semantic.jpg"  
---

<ul class="listing">
{% for tag in site.tags %}
  {% if tag[0] == "collection" %}
    {% for post in tag[1] %}
      <li class="listing-item">
        <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
        <a href="{{ post.url }}" title="{{ post.tile }}">{{ post.title }}</a>
    {% endfor %}
  {% endif %}
{% endfor %}