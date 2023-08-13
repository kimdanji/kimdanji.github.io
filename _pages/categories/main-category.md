---
title: "카테고리"
layout: archive
permalink: /categories/main-category/
---

{% assign posts = site.categories %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
