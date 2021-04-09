---
title: "카테고리 정렬"
excerpt: "2021년 04월 06일"
search: true
categories: 
  - etc
tags: 
  - github
---


카테고리를 알파벳순으로 변경하려고 한다. 

<br>

- 기존 <br>
![image](https://user-images.githubusercontent.com/73421820/113731785-02713800-9734-11eb-97f6-453f6d3bab0a.png) 


<br>


`_includes > nav_list_main` 파일에서 카테고리와 관련된 내용을 변경할 수 있다.

![카테고리](https://user-images.githubusercontent.com/73421820/113733400-62b4a980-9735-11eb-9f73-f074347eebf1.png)

 <br>


**assing**태그를 이용해 Liquid 변수를 생성한다.

![image](https://user-images.githubusercontent.com/73421820/113905095-c0b4c000-980d-11eb-800e-4b4b49b8f78f.png)
<br>

sorted_categories 변수를 생성해  site.categories(전체 카테고리)를 정렬해서 담는다.


![image](https://user-images.githubusercontent.com/73421820/113905161-d32ef980-980d-11eb-81ce-a508f409decd.png)

<br>

반복될 변수를 site.categories가 아닌 정렬된 sorted_categories로 변경해주면 끝~


![image](https://user-images.githubusercontent.com/73421820/113734008-f0909480-9735-11eb-8af3-802709915893.png) 

<br>


A-Z a-z로 정렬된 것을 확인할 수 있다.

<br>

Post를 볼 때 옆에 나오는 sideList의 정렬도 바꿔주기 위해 `_includes > nav_list` 도 똑같이 바꿔준다. 
<br><br>