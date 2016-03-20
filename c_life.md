---
layout: page
title: "生活"
description: "对自己好点，因为一辈子不长；对身边的人好点，因为下辈子不一定能够遇见！"
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