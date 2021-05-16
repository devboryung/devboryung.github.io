---
layout: archive
permalink: /categories/Spring
title: "Spring practice"
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.Spring | sort:"date" | reverse %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
