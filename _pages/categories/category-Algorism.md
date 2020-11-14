---
layout: archive
permalink: /categories/Algorism
title: "Post about Algorism"
author_profile: true
sidebar_main: true
---
<!-- 사이드바 카테고리 클릭 후 나오는 포스트 목록 정렬 변경 원래는 sort:"date"였음 --->
{% assign posts = site.categories.Algorism | sort | reverse %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
