---
layout: archive
permalink: /categories/Academy
title: "Post about Academy"
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.Academy | sort:"date" | reverse %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
