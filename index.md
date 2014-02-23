---
layout: page
title: 呵呵
tagline: 终于倒腾出来了，泪流满面%>_<%
---
{% include JB/setup %}
    
## 文章列表

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


