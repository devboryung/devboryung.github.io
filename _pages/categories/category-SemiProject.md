---
layout: archive
permalink: /categories/SemiProject
title: "SemiProject"
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.SemiProject | sort:"date" | reverse %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}