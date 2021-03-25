---
layout: archive
permalink: /categories/etc
title: "etc"
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.etc | sort:"date" | reverse %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
