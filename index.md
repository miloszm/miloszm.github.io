---
layout: default
title: Home
---

# Rust/Blockchain Blog

Welcome to my blog about Rust and blockchain development.

## Recent Posts

{% for post in site.posts %}
- [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %d, %Y" }}
  {% endfor %}