---
layout: page
title: 亦玩亦工作，团结、紧张、严肃、活泼
tagline: 团结、紧张、严肃、活泼
---
<p>最新文章</p>
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


