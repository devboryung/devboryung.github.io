---
layout: archive
permalink: /categories/Error
title: "Error"
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.Error | sort:"date" | reverse %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
