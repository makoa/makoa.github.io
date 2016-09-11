---
layout: default
title: Blog
permalink: /blog/
---

<div class="posts">
{% for post in site.posts %}
  {{ forloop.index }}. [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}
</div>