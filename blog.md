---
layout: default
title: Blog
permalink: /blog/
---

{% for post in site.posts %}
  {{ forloop.index }}. [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}