---
layout: archive
permalink: /categories/JAVA
title: "JAVA"
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.JAVA %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
