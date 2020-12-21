---
title: "2020년 12월 21일"
excerpt: "JSP 환경설정 "
search: true
categories: 
  - Academy
tags:
  - JSP
  - JAVA
  - jQuery
  - Javascript
  - HTML
  - CSS
toc: true
---


## 아파치 톰캣 다운로드
tomcat.apache.org/download-80.cgi<br>
8.5.61버전 다운로드<br>
zip으로 다운받아서 압축 풀기
<br>
![아파치 톰캣](https://user-images.githubusercontent.com/73421820/102758419-0f049700-43b6-11eb-90b2-0d7814796664.PNG)
<br>
<br>

## 이클립스 UTF-8로 인코딩 설정
Window -> Preferences (검색어 enco, spelling 입력하면 찾기 쉬움)<br>
General : Content Types, Workspace<br>
Web : CSS Files, HTML Files, JSP Files<br>
General - Editors - Text Editors : Spelling<br>
모두 Apply 후 Close

<br>
![contenttype](https://user-images.githubusercontent.com/73421820/102759552-a0c0d400-43b7-11eb-9139-4191431e745d.PNG)
![workspace](https://user-images.githubusercontent.com/73421820/102759557-a1596a80-43b7-11eb-8d29-b217134a3322.PNG)
![CSS](https://user-images.githubusercontent.com/73421820/102759561-a1f20100-43b7-11eb-8e4f-847386c599da.PNG)
![spelling](https://user-images.githubusercontent.com/73421820/102759565-a28a9780-43b7-11eb-9811-3a3988c0baf9.PNG)

<br><br>

## 아파치 등록하기
Window -> Preferences(Runtime 검색)<br>
Server Runtime Environments -> Add -> Apache -> Apache Tomcat v8.5선택 <br>
-> NEXT -> Browse.. 클릭 -> 아파치 폴더 선택 -> Finish
<br>
![아파치등록](https://user-images.githubusercontent.com/73421820/102763769-7d992300-43bd-11eb-908d-3c51639687ce.PNG)
![아파치 폴더 선택](https://user-images.githubusercontent.com/73421820/102763770-7eca5000-43bd-11eb-84e4-fead614b74f9.PNG)
<br><br>



## server 폴더 생성하기
View창의 Servers에서 create a newServer클릭(아니면 ctrl+n > server > new servers) <br>
-> Apache폴더 ->Tomcat v8.5선택 -> finish<br>
option 중 Serve modules without publishing, Modules auto reload by default 선택
<br>
![server1](https://user-images.githubusercontent.com/73421820/102764350-5bec6b80-43be-11eb-8c4a-9a5fd3b06ac8.PNG)
![server2](https://user-images.githubusercontent.com/73421820/102764354-5d1d9880-43be-11eb-9beb-7481f2cc37e4.PNG)
<br><br>

## ServletProject 폴더 생성하기
ctrl+n > Dynamic Web Project -> NEXT -> Project name 작성 -> Next -> src 클릭 -><br>
Default output folder : WebContent\WEB-INF\classes -> NEXT -><br>
Generate web.xml deployment descriptor 체크 -> Finish<br>
*폴더 안에 WEB-INF폴더 -> web.xml 이 있어야 정상이다.
<br><br>

## 오라클 포트 변경하기
JSP와 충돌하기 때문에 8080->9090 으로 변경
<br>
![3 오라클 포트 변경1](https://user-images.githubusercontent.com/73421820/102764954-517ea180-43bf-11eb-86b5-c693e0ad8b42.PNG)
![3 오라클 포트 변경2](https://user-images.githubusercontent.com/73421820/102764960-52173800-43bf-11eb-9d58-c5c443324ec1.PNG)
<br>
<br>












