---
layout: archive
permalink: /categories/정보처리기사
title: "정보처리기사"
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.정보처리기사 %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
