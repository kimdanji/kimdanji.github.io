---
title: "게임"
layout: archive
permalink: /categories/game/
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.game %}
{% for post in posts %} 
  {% include archive-single2.html type=page.entries_layout %} 
{% endfor %}
