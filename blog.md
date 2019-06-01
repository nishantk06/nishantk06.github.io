---
layout: default
permalink: /blog/
title: Blog
---


{% for post in site.categories.blog %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
