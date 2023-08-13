---
title: "카테고리"
layout: archive
permalink: /categories/main-category/
---

{% assign posts = site.categories.csharp %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}

{% assign posts = site.categories.designpattern %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}
