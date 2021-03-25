---
layout: archive
permalink: /categories/FinalProject
title: "FinalProject"
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.FinalProject | sort:"date" | reverse %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
