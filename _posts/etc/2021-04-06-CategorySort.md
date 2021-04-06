---
title: "2021년 04월 06일"
excerpt: "카테고리 정렬"
search: true
categories: 
  - etc
tags: 
  - github
toc: true
---

카테고리를 알파벳순으로 변경하려고 한다. <br>

- 기존
![image](https://user-images.githubusercontent.com/73421820/113731785-02713800-9734-11eb-97f6-453f6d3bab0a.png) <br>


_includes > nav_list_main 파일에서 카테고리와 관련된 내용을 변경할 수 있다.

![카테고리](https://user-images.githubusercontent.com/73421820/113733400-62b4a980-9735-11eb-9f73-f074347eebf1.png) <br>


**assing**태그를 이용해 Liquid 변수를 생성한다.

```java
{% assign sorted_categories = site.categories | sort%}
```
sorted_categories 변수를 생성해  site.categories(전체 카테고리)를 정렬해서 담는다.


```java
{% for category in sorted_categories %}
```

반복될 변수를 site.categories가 아닌 정렬된 sorted_categories로 변경해주면 끝~


![image](https://user-images.githubusercontent.com/73421820/113734008-f0909480-9735-11eb-8af3-802709915893.png) <br>

A-Z a-z로 정렬된 것을 확인할 수 있다.

<br><br>

