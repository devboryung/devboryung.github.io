---
layout: archive
permalink: /categories/ToyProject
title: "Post about ToyProject"
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.ToyProject | sort:"date" | reverse %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
