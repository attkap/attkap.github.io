---
layout: default
title: Your Blog Name
---

# Welcome to My Blog!

This is the home of my personal blog, where I write about various topics that interest me. Feel free to explore the posts below!

{% for post in site.posts %}
  ## [{{ post.title }}]({{ post.url }})
  *{{ post.date | date: "%B %d, %Y" }}*
  {{ post.excerpt }}
{% endfor %}
