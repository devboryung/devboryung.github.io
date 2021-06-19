---
title: "[Spring] 화면 처리"
excerpt: "Spring Framework - 목록 페이지 작업과 includes"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 화면 처리

화면에는 JSP와 JavaScript(jQeury), CSS, HTML을 이용해서 작성한다.<br>

화면을 개발하기 전에는 반드시 화면의 전체 레이아웃이나 디자인이 반영된 상태에서 개발하는 것을 추천한다.<br>
일부 개발자들은 화면을 나중에 처리한다고 생각하고 진행하는 경우가 있는데 결과적으로는 두 배의 시간을 들이는 결과가 될 가능성이 높기 때문에 권장하지 않는다.<br>

실습은 부트스트랩의 무료 디자인인 `SB Admin2`를 다운받아 압축해서 이용한다.<br>

<br><br>

### 목록 페이지 작업과 includes

스프링 MVC의 JSP를 처리하는 설정은 servlet-context.xml에 아래와 같이 작성되어 있다.<br>
<br>

> servlet-context.xml

```xml
<resources mapping="/resources/**" location="/resources/" />

<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <beans:property name="prefix" value="/WEB-INF/views/" />
    <beans:property name="suffix" value=".jsp" />
</beans:bean>
```


스프링 MVC의 설정에서 화면 설정은 ViewResolver라는 객체를 통해서 이루어지는데, 위의 설정을 보면 '/WEB-INF/views'폴더를 이용하는 것을 볼 수 있다.<br>
'/WEB-INF'경로는 브라우저에서 직접 접근할 수 없는 경로이므로 반드시 Controller를 이용하는 모델 2방식에서는 기본적으로 사용하는 방식이다.<br>

게시물 리스트의 URL은 '/board/list'이므로 최종적인 '/WEB-INF/view/board/list.jsp'가 된다.<br>
<br>

> views/board/list.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>List Page</h1>
</body>
</html>
```
<br>

![image](https://user-images.githubusercontent.com/73421820/121705994-2aa05d00-cb10-11eb-882a-60393b60155d.png)
<br>

'/controller'경로를 가진 Tomcat의 설정을 '/'로 조정한다.<br>

![image](https://user-images.githubusercontent.com/73421820/121706187-5885a180-cb10-11eb-9e19-c55df4574837.png)

<br>

> 변경 후

![image](https://user-images.githubusercontent.com/73421820/121706301-76eb9d00-cb10-11eb-8334-ad6822352e92.png)<br>

경로에 대한 설정은 css, js 이미지 파일들의 경로에 치명적인 영향을 주기 때문에 처음부터 절대 경로 혹은 상대 경로에 대해서 명확히 결정한 후에 프로젝트를 진행해야 된다.<br>
일반적인 경우라면 절대 경로를 이용하는 것이 좋다.<br>

<br><br>

**SB Admin2 페이지 적용하기**

정상적으로 '/board/list' 페이지가 동작하면 'SB Admin2'의 pages 폴더에 있는 tables.html의 내용을 list.jsp의 내용으로 복사해서 수정한다.<br>
이 상태로 실행하면 CSS와 JS가 깨지기 때문에 브라우저의 개발자 도구를 통해서 파일들의 경로를 수정해야 한다.<br>
![image](https://user-images.githubusercontent.com/73421820/121709248-507b3100-cb13-11eb-9a41-87f0d95b59c9.png)
<br><br>

org.zerock.config.WebConfig 클래스에는 CSS나 JS 파일과 같은 정적인(static) 자원들의 경로를 'resources'라는 경로로 지정하고 있다.<br>

> servlet-context.xml

```xml
<resources mapping="/resources/**" location="/resources/" />
```
<br>

SB Admin2의 압축을 풀어둔 모든 폴더를 프로젝트 내 webapp밑의 resources 폴더로 복사해 넣는다.<br>
![image](https://user-images.githubusercontent.com/73421820/121709599-ae0f7d80-cb13-11eb-89fc-4d2964a12b3e.png)

<br><br>

파일들을 resources 경로로 넣어도 아직은 페이지에서 경로를 수정하지 않았기 때문에 문제가 생기는 것은 동일하다.<br>
list.jsp 파일에서 CSS나 JS 파일의 경로를 '/resouces'로 시작하도록 수정한다.<br>


![image](https://user-images.githubusercontent.com/73421820/121709803-e57e2a00-cb13-11eb-9c80-6ad5efd91023.png)
<br>




<br><br>


**includes 적용**

JSP를 작성할 때마다 많은 양의 HTML 코드를 이용하는 것을 피하기 위해 JSP의 include 지시자를 활용해서 페이지 제작 시에 필요한 내용만을 작성할 수 있게 사전에 작업을 해야 한다.<br>

includes 폴더를 작성하고, header.jsp와 footer.jsp를 선언한다.<br>

![image](https://user-images.githubusercontent.com/73421820/121710229-5c1b2780-cb14-11eb-8ec1-b1004f2d569a.png)

<br><br>


- header.jsp 적용

header.jsp는 페이지에서 핵심적인 부분이 아닌 영역 중에서 위쪽의 HTML 내용을 처리하기 위해 작성한다.<br>
브라우저에서 '검사'기능을 활용하면 특정한 \<div\>가 어떤 부분을 의미하는지 확인할 수 있다.<br>
SB Admin2는 \<div\>들 중에서  id 속성값이 'page-wrapper'부터가 핵심적인 페이지의 내용이므로 list.jsp 파일의 처음 부분에서 \<div id='page-wrapper'\>라인까지 잘라서 header.jsp의 내용으로 처리한다.<br>

<br><br>

**jQuery 라이브러리 변경**

JSP 페이지를 작성하다 보면 JavaScript로 브라우저 내에서의 조작이 필요한 경우가 많다.<br>
예제는 jQuery를 이용할 것이고, 문제는 위의 방식대로 처리했을 때 jQuery 라이브러리가 footer.jsp내에 포함되어 있다는 점이다.<br>
성능을 조금 손해 보더라도 jQuery를 header.jsp에 선언해 두면 작성하는 JSP에서 자유롭게 사용할 수 있으므로 수정해야 한다.<br>

footer.jsp의 상단에 있는 jquery.min.js파일의 \<scipt\> 태그를 제거한다.<br>

> footer.jsp

```java
<!-- jQuery -->
<script src="/resources/vendor/jquery/jquery.min.js"></script>
```
<br>

jQuery는 인터넷을 통해서 다운로드 받을 수 있게 jQuery의 링크를 검색해서 header.jsp .하단에 추가한다.<br>


> header.jsp

```java
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
```

<br>

<br><br>


- 반응형 웹 처리

SB Admin2는 반응형으로 설계되어 있어서 브라우저의 크기에 맞게 모바일 용으로 자동으로 변경되지만 jQuery의 최신 버전을 사용한 상태에서는 모바일 크기에서 '새로고침'시 메뉴가 펼쳐지는 문제가 발생한다.<br>

이 문제를 해결하기 위해서 includes 폴더 내 footer.jsp에 코드를 추가한다.<br>

> footer.jsp

```java
<script>
$(document).ready(function() {
    $('#dataTables-example').DataTable({
        responsive: true
    });
    
    $(".sidebar-nav")
    .attr("class","sidebar-nav navbar-collapse collapse")
    .attr("aria-expanded","false")
    .attr("style","height:1px");
});
</script>
```
<br>


<br><br>

<br><br>





<br><br>
관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
