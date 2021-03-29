---
layout: archive
permalink: /categories/TIL
title: "Today I Learn"
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.TIL | sort:"date" | reverse %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
