---
layout: page
title: Koji MASUDA
lang: ja
tagline: Kobe City College of Technology
---
{% include JB/setup %}
{% include JB/langselect_index %}

## Recent Posts

{% assign posts=site.posts | where:"lang", page.lang %}
<ul class="posts">
  {% for post in posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

