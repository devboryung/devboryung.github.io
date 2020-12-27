---
title: "2020년 12월 22일"
excerpt: "Servlet - 2"
search: true
categories: 
  - Academy
tags: 
  - Servlet
  - Java
  - HTML
  - CSS
toc: true
---

## Servlet - 3(비즈니스 로직)
Servlet은 Java Class이기 때문에 비즈니스 로직(데이터 가공 처리 및 DB연결 관련 로직 등)<br>
수행이 Front 관련 기술들 보다 간편하고 효율적으로 처리할 수 있다.
<br><br>

## Servlet - 4(Servlet + JSP)
서블릿은 응답 화면에 표현될 HTML 코드를 프로그램적으로 작성함.<br> 
이로 인해 비즈니스 로직과 화면 구현 로직이 같이 작성되는 문제점이 있어 협업, 가독성이 떨어짐.<br> 
이러한 단점을 보완하기 위해서 비즈니스 로직과 화면 구현 로직을 분리함.<br>
비즈니스 로직은 Servlet이, 화면 구현 로직은 JSP로 분리.
<br><br>

### web.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">

	<!-- web.xml (배포 서술자) : 프로젝트 배포와 관련된 설정을 하는 파일  -->

	<!-- display-name : 배포 시 프로젝트 시작 이름 
		 http://localhost:8080/ServletProject
		 					   (display-name) -->
  <display-name>ServletProject</display-name>
  
  
  <!-- welcome-file-list : 배포 시작 주소로 요청이 오는 경우 연결할 파일 목록을 작성하는 부분
  	   ex) http://localhost:8080/ServletProject -->
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
  </welcome-file-list>
    
  <servlet>
	  <servlet-name>Test3</servlet-name>
	  <servlet-class>com.kh.servlet.controller.TestServlet3</servlet-class>
  </servlet>
  
  <servlet-mapping>
	  <servlet-name>Test3</servlet-name>
	  <url-pattern>/testServlet3.do</url-pattern>
  </servlet-mapping>
  
  
  <servlet>
	  <servlet-name>Test4</servlet-name>
	  <servlet-class>com.kh.servlet.controller.TestServlet4</servlet-class>
  </servlet>
  
  <servlet-mapping>
	  <servlet-name>Test4</servlet-name>
	  <url-pattern>/testServlet4.do</url-pattern>
  </servlet-mapping>
   
</web-app>

```
<br><br>

### index.html
```html
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport"
	content="width=device-width, initial-scale=1, shrink-to-fit=no">
<link rel="stylesheet"
	href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css"
	integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk"
	crossorigin="anonymous">

<title>Servlet 예제</title>
<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
<script
	src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js"
	integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo"
	crossorigin="anonymous"></script>
<script
	src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js"
	integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6"
	crossorigin="anonymous"></script>

</head>
<body>
	<nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
		<a class="navbar-brand" href="#">Navbar</a>

		<div class="collapse navbar-collapse" id="navbarsExampleDefault">
			<ul class="navbar-nav mr-auto">
				<li class="nav-item active"><a class="nav-link" href="#">Home</a>
				</li>
				<li class="nav-item"><a class="nav-link" href="#">Link</a></li>
				<li class="nav-item"><a class="nav-link disabled" href="#">Disabled</a>
				</li>
				<li class="nav-item dropdown"><a
					class="nav-link dropdown-toggle" href="#" id="dropdown01"
					data-toggle="dropdown" role="button" aria-haspopup="true"
					aria-expanded="false">Dropdown</a>
					<div class="dropdown-menu" aria-labelledby="dropdown01">
						<a class="dropdown-item" href="#">Action</a> <a
							class="dropdown-item" href="#">Another action</a> <a
							class="dropdown-item" href="#">Something else here</a>
					</div></li>
			</ul>
			<form class="form-inline my-2 my-lg-0">
				<input class="form-control mr-sm-2" type="text" placeholder="Search"
					aria-label="Search">
				<button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
			</form>
		</div>
	</nav>

	<div class="jumbotron">
		<div class="container">
			<h1 class="display-3">Hello, Servlet!</h1>
			<p>
				서블릿이란? 웹 서비스를 위한 자바 클래스를 말하며 <br> 자바를 사용해 웹을 만들기 위한 기술로
				javax.servlet.http.HttpServlet 클래스를 상속받는다.<br> 다시 말해 기존의
				java파일에 웹 페이지를 구현하기 위한 html코드가 들어간 구조<br> 클라이언트의 요청을 처리하고, 그
				결과를 HTML을 이용하여 응답할 수 있는 페이지를 만들어 클라이언트에게 다시 전달하는 구현 규칙이 있다. <br>

				단, Java 코드에 작성된 HTML 코드 변경 시 재 컴파일을 해야되는 단점이 있다.
			<p>
				<a class="btn btn-primary btn-lg"
					href="https://www.google.co.kr/search?q=Servlet&rlz=1C1QJDB_enKR792KR792&oq=Servlet&aqs=chrome..69i57j69i65j69i60j69i59j69i61l2.2395j0j7&sourceid=chrome&ie=UTF-8"
					role="button">Learn more &raquo;</a>
			</p>
		</div>
	</div>

	<div class="container">
		<!-- Example row of columns -->
		<div class="row">

		<div class="row">
			<h2>Servlet - 3(비즈니스 로직)</h2>
			<p>Servlet은 Java Class이기 때문에 비즈니스 로직(데이터 가공 처리 및 DB연결 관련 로직 등)
				수행이 Front 관련 기술들 보다 간편하고 효율적으로 처리할 수 있다.</p>
			<p>
				<a class="btn btn-secondary" href="views/testServlet3.html"
					role="button">View details &raquo;</a>
			</p>
		</div>
		<div class="row">

			<h2>Servlet - 4(Servlet + JSP)</h2>
			<p>
				서블릿은 응답 화면에 표현될 HTML 코드를 프로그램적으로 작성함.<br> 이로 인해 비즈니스 로직과 화면 구현
				로직이 같이 작성되는 문제점이 있어 협업, 가독성이 떨어짐.<br> 이러한 단점을 보완하기 위해서 비즈니스 로직과
				화면 구현 로직을 분리함.<br> 비즈니스 로직은 Servlet이, 화면 구현 로직은 JSP로 분리.
			</p>
			<p>
				<a class="btn btn-secondary" href="views/testServlet4.html"
					role="button">View details &raquo;</a>
			</p>
		</div>

		<hr>

	</div>

	<footer class="container">
		<p>&copy; Company 2017-2018</p>
	</footer>

</body>
</html>
```

### Servlet - 3(비즈니스 로직).html
```html
<!DOCTYPE html>
<html lang="ko">
    <head>
    <meta charset="UTF-8">
        <title>개인정보 입력 - 비즈니스 로직</title>
        <style>
            ul{ list-style-type: none; }

            li{  padding-bottom: 20px; }
        </style>
    </head>
    <body>
        <h2>개인 정보 입력 - 비즈니스 로직</h2>
        당신의 개인 정보를 입력하겠습니다.<br>
        데이터 입력 후 확인 버튼을 눌러주세요.<br>

        <form action="/ServletProject/testServlet3.do" method="post" name="testFrm">
                <ul>
                    <li>이름 : <input type="text" name="name" size="10" /></li>
                    <li>성별 : 
                        남자<input type="radio" name="gender" value="남자" />
                        여자<input type="radio" name="gender" value="여자" /> 
                    </li>
                    <li>나이 : 
                        10대 미만<input type="radio" name="age" value="10대 미만" />
                        10대<input type="radio" name="age" value="10대" />
                        20대<input type="radio" name="age" value="20대" />
                        30대<input type="radio" name="age" value="30대" />
                        40대<input type="radio" name="age" value="40대" />
                        50대<input type="radio" name="age" value="50대" />				 
                    </li>
                    <li>사는 도시 : 
                        <select name="city">
                            <option value="서울">서울</option>
                            <option value="경기">경기</option>
                            <option value="강원">강원</option>
                            <option value="충북">충북</option>
                            <option value="충남">충남</option>
                            <option value="경북">경북</option>
                            <option value="경남">경남</option>
                            <option value="전북">전북</option>
                            <option value="전남">전남</option>
                            <option value="제주">제주</option>
                        </select>
                    </li>
                    <li>키 :
                        <input type="range" name="height" min="140" max="200" step="1">
                        <input type="text" id="heightValue">
                    </li>
                    <li>좋아하는 음식 (모두 고르세요) : 
                        <label for="food1">한식</label><input type="checkbox" name="food" id="korea" value="한식" />
                        <label for="food2">중식</label><input type="checkbox" name="food" id="china" value="중식" />
                        <label for="food3">일식</label><input type="checkbox" name="food" id="japan" value="일식" />
                        <label for="food4">양식</label><input type="checkbox" name="food" id="western" value="양식" />
                        <label for="food5">분식</label><input type="checkbox" name="food" id="dduk" value="분식" /> 
                    </li>
                    <li>
                        <br/>
                        <input type="submit" name="btnOK" id="btnOK" value="확인"  />&nbsp;&nbsp; 
                        <input type="reset" value="취소" />
                    </li>
                 </ul>
            </form>

            <!-- jQuery  -->
            <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
            <script>
                $("input[type=range]").on("input",function(){
                    $(this).next().val($(this).val());
                });

                $("#heightValue").on("input",function(){
                    $(this).prev().val($(this).val());
                });
            </script>
    </body>
</html>
```
<br>

### Servlet - 3(비즈니스 로직).java
```java
package com.kh.servlet.controller;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class TestServlet3 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       

    public TestServlet3() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	
		// post 방식의 데이터 전달 -> 문자 인코딩이 깨짐 -> UFG-8형식으로 셋팅
		request.setCharacterEncoding("UTF-8");
		
		// 전달된 값(파라미터)를 변수에 저장
		String name = request.getParameter("name");
		String gender = request.getParameter("gender");
		String age = request.getParameter("age");
		String city = request.getParameter("city");
		String height = request.getParameter("height");
		String[] foodArr = request.getParameterValues("food");
	
		// 요청 데이터를 이용해서 새로운 응답 데이터를 생성
		String gift = null;
		// 나이대에 따른 선물 추천
		switch(age) {
		case "10대 미만" : gift = "장난감"; break;
		case "10대" : gift = "닌텐도 스위치"; break;
		case "20대" : gift = "스마트폰"; break;
		case "30대" : gift = "돈"; break;
		case "40대" : gift = "정관장"; break;
		case "50대" : gift = "바디프랜드"; break;
		}
		
		
		// 응답화면 출력 준비
		response.setContentType("text/html; charset=UTF-8");
	
		// 작성된 HTML 문자열을 출력하기 위한 스트림 준비
		PrintWriter out = response.getWriter();
		
		// 화면에 출력
		out.println("<!DOCTYPE html>\r\n" + 
				"<html lang=\"ko\">\r\n" + 
				"<head>\r\n" + 
				"    <meta charset=\"UTF-8\">\r\n" + 
				"    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\r\n" + 
				"    <title>TestServlet1 응답 페이지</title>\r\n" + 
				"    <style>\r\n" + 
				"        h1{ color : red;}\r\n" + 
				"        span.name{ color : orange; }\r\n" + 
				"        span.gender{ color : yellow; }\r\n" + 
				"        span.age{ color : green; }\r\n" + 
				"        span.city{ color :blue; }\r\n" + 
				"        span.height{ color : navy; }\r\n" + 
				"        span.food{ color : purple; }\r\n" + 
				"        span{font-weight: bold;}\r\n" + 
				"    </style>\r\n" + 
				"</head>\r\n" + 
				"<body>\r\n" + 
				"    <h1>개인 정보 입력 결과(GET)</h1>");
		
		
		out.printf("    <span class='name'>%s</span>님은\r\n" + 
				"    <span class='age'>%s</span>이며\r\n" + 
				"    <span class='city'>%s</span>에 사는\r\n" + 
				"    키 <span class='height'>%s</span>cm 인\r\n" + 
				"    <span class='gender'>%s</span>입니다.\r\n" + 
				"    <br>\r\n" + 
				"    좋아하는 음식은\r\n" + 
				"    <span class='food'>%s</span>입니다.\r\n" 
				, name, age, city, height, gender, 
				  String.join(", ", foodArr) );
		
		out.println("<h3>" + age + "에 추천하는 선물은</h3>");
		out.println("<h4>" + gift + "입니다.</h4>");
		
		
		// <body> 닫는 구문
		out.println("</body>\r\n" +	"</html>");
		
	
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}

```
<br>
<br>



### Servlet - 4(Servlet + JSP)
![캡처](https://user-images.githubusercontent.com/73421820/102872792-9409b200-4483-11eb-98c8-c93b503c49ab.PNG)
<br>

```html
<!DOCTYPE html>
<html lang="ko">
    <head>
    <meta charset="UTF-8">
        <title>개인정보 입력 - Servlet + JSP</title>
        <style>
            ul{ list-style-type: none; }

            li{  padding-bottom: 20px; }
        </style>
    </head>
    <body>
        <h2>개인 정보 입력 - Servlet + JSP</h2>
        당신의 개인 정보를 입력하겠습니다.<br>
        데이터 입력 후 확인 버튼을 눌러주세요.<br>

        <form action="/ServletProject/testServlet4.do" method="post" name="testFrm">
                <ul>
                    <li>이름 : <input type="text" name="name" size="10" /></li>
                    <li>성별 : 
                        남자<input type="radio" name="gender" value="남자" />
                        여자<input type="radio" name="gender" value="여자" /> 
                    </li>
                    <li>나이 : 
                        10대 미만<input type="radio" name="age" value="10대 미만" />
                        10대<input type="radio" name="age" value="10대" />
                        20대<input type="radio" name="age" value="20대" />
                        30대<input type="radio" name="age" value="30대" />
                        40대<input type="radio" name="age" value="40대" />
                        50대<input type="radio" name="age" value="50대" />				 
                    </li>
                    <li>사는 도시 : 
                        <select name="city">
                            <option value="서울">서울</option>
                            <option value="경기">경기</option>
                            <option value="강원">강원</option>
                            <option value="충북">충북</option>
                            <option value="충남">충남</option>
                            <option value="경북">경북</option>
                            <option value="경남">경남</option>
                            <option value="전북">전북</option>
                            <option value="전남">전남</option>
                            <option value="제주">제주</option>
                        </select>
                    </li>
                    <li>키 :
                        <input type="range" name="height" min="140" max="200" step="1">
                        <input type="text" id="heightValue">
                    </li>
                    <li>좋아하는 음식 (모두 고르세요) : 
                        <label for="food1">한식</label><input type="checkbox" name="food" id="korea" value="한식" />
                        <label for="food2">중식</label><input type="checkbox" name="food" id="china" value="중식" />
                        <label for="food3">일식</label><input type="checkbox" name="food" id="japan" value="일식" />
                        <label for="food4">양식</label><input type="checkbox" name="food" id="western" value="양식" />
                        <label for="food5">분식</label><input type="checkbox" name="food" id="dduk" value="분식" /> 
                    </li>
                    <li>
                        <br/>
                        <input type="submit" name="btnOK" id="btnOK" value="확인"  />&nbsp;&nbsp; 
                        <input type="reset" value="취소" />
                    </li>
                 </ul>
            </form>

            <!-- jQuery  -->
            <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
            <script>
                $("input[type=range]").on("input",function(){
                    $(this).next().val($(this).val());
                });

                $("#heightValue").on("input",function(){
                    $(this).prev().val($(this).val());
                });
            </script>
    </body>
</html>
```
<br>

### Servlet - 4(Servlet + JSP).java
```java
package com.kh.servlet.controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class TestServlet4 extends HttpServlet {
	private static final long serialVersionUID = 1L;

	public TestServlet4() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		request.setCharacterEncoding("UTF-8");
		
		String name = request.getParameter("name");
		String gender = request.getParameter("gender");
		String age = request.getParameter("age");
		String city = request.getParameter("city");
		String height = request.getParameter("height");
		String[] foodArr = request.getParameterValues("food");
	
		
		String gift = null;
		
		switch(age) {
		case "10대 미만" : gift = "슬라임"; break;
		case "10대" : gift = "옷"; break;
		case "20대" : gift = "스마트폰"; break;
		case "30대" : gift = "돈"; break;
		case "40대" : gift = "정관장"; break;
		case "50대" : gift = "나훈아 콘서트 티켓"; break;
		}
	
		
		// 응답 화면을 만들기 위한 servlet 코드를 JSP로 작성할 수 있도록
		// 요청 위임을 진행.
		
		// 여기서 요청 위임이란?
		// 현재 Servlet의 역할을 JSP로 넘기는 것
		
		// 요청 위임 객체> RequestDispatcher
		RequestDispatcher view = request.getRequestDispatcher("views/testServlet4End.jsp");
	
		/* Dispatcher : 필요 정보를 제공하는 역할, 발송자
		 * RequestDispatcher : 현재 HttpServletRequest 객체에 담긴 정보를 저장하고 있다가
		 * 						지정된 다음 페이지에서 해당 정보를 볼 수 있게 위임하는 기능.
		 * */
		
		// 요청 위임 객체에 가공된 데이터를 전달할 수 있도록 세팅
		// JSP로 새로운 데이터를 전달하는 방법
		// request.setAttribute("Key",value);
		request.setAttribute("gift", gift);
		request.setAttribute("foodJoin", String.join(", ", foodArr));
	
	
		// 요청 위임 진행
		view.forward(request, response);
		/*
		 * forward 방식 : 요청에 대한 응답 방법 중 하나로 
		 * 기존 파라미터 등의 요청 정보를 유지하며 페이지를 전환하는 방법.
		 * -> forward 방식으로 페이지 전환 시 이전 페이지 주소가 유지됨.
		 * 
		 * A.html -(요청)-> B.do(servlet) -(요청위임)-> C.jsp
		 * 요청위임 시 forward 방식을 사용할 경우
		 * 요청 위임 객체 RequestDispatcher가 B.do에서 C.jsp로 넘어가지만,
		 * 주소는 B.do로 유지가 됨.
		 * ->( B.do의 응답 화면 코드가 C.jsp로 대체가 됐을 뿐이다)
		 * 
		 * */
	
	
	}
	

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}

```
<br>

### Servlet - 4(Servlet + JSP).JSP
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%> <!-- 이 구문이 있어야 JSP로 인식이 된다. -->

<%
	// 위임받은 request에 있는 파라미터 값 얻어오기
	String name = request.getParameter("name");
	String age = request.getParameter("age");
	String gender = request.getParameter("gender");
	String city = request.getParameter("city");
	String height = request.getParameter("height");
	
	// 위임받은 request에 새롭게 세팅된 가공 데이터 얻어오기
	// setAttribute()로 세팅된 값의 자료형은 Object가 된다
	// -> 알맞게 형변환 필요
	String gift = (String)request.getAttribute("gift");
	String foodJoin = (String)request.getAttribute("foodJoin");
%>

<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TestServlet1 응답 페이지</title>
    <style>
        h1{ color : red; }
        span.name{ color : orange;}
        span.gender{ color : yellow;}
        span.age{ color : green;}
        span.city{ color : blue;}
        span.height{ color : navy;}
        span.food{ color : purple;}
        span{font-weight: bold;}
    </style>
</head>
<body>
    <h1>개인 정보 입력 결과(Servlet + JSP)</h1>
    <span class='name'><%=name %></span>님은
    <span class='age'><%=age %></span>세 이며,
    <span class='city'><%=city %></span>에 사는
    키 <span class='height'><%=height %></span>cm 인 
    <span class='gender'><%=gender %></span> 입니다.
    <br>
    좋아하는 음식은
    <span class='food'><%=foodJoin %></span> 입니다.
    
    <h3><%=age %>에 추천할만한 선물</h3>
    <h4> ๑◕‿‿◕๑ つ━☆*。<%=gift %>은 어떠신가요? </h4>
</body>
</html>

```
<br>
<br>

## JSP란?
- JSP(Java Server Page) : JAVA코드가 들어가 있는 HTML 코드
- Java의 웹 서버 프로그램 스펙(서블릿)으로 변환되어 서비스 됨. 
<br>

## Servelt과 JSP의 차이점
- Servlet
 > "웹 서비스 기능을 해주는 자바 클래스"를 말하는 것으로
 자바 소스코드 속에 HTML 코드가 들어가는 형태<br>
 > HTML 문서를 작성하는데 복잡하고 번거롭다는 단점이 있음.
- JSP
 > 복잡한 Serlvet을 좀 더 간단히 사용할 수 있음.<br>
 > Servlet과 반대로 HTML소스코드 속에
자바 소스코드(<% %> 또는 <%= %>)가 들어가는 형태.<br>
 > 자바 소스코드로 작성된 부분은 웹 브라우저로 보내지지 않고
 컴파일을 통해 클래스 파일로 변환되어 웹 서버(WAS)에서 실행됨.<br>
 <br><br>


### web.xml

 ```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>JSPProject</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
  </welcome-file-list>
  
  <servlet>
  	<servlet-name>pizza</servlet-name>
  	<servlet-class>com.kh.practice.controller.PizzaServlet</servlet-class>
  </servlet>
  <servlet-mapping>
  <servlet-name>pizza</servlet-name>
  <url-pattern>/confirmPizza.do</url-pattern>
  </servlet-mapping>

</web-app>
 ```
<br>

### index.html
 ```html
 <!DOCTYPE html>
<html lang="ko">
    <head>
    <meta charset="UTF-8">
	    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css" 
        integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
        
	    <title>JSP 예제</title>
       	<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
       <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
       <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
  
    </head>
    <body>
        <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
            <a class="navbar-brand" href="#">Navbar</a>
    
            <div class="collapse navbar-collapse" id="navbarsExampleDefault">
                <ul class="navbar-nav mr-auto">
                    <li class="nav-item active">
                        <a class="nav-link" href="#">Home</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#">Link</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link disabled" href="#">Disabled</a>
                    </li>
                    <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle" href="#" id="dropdown01" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown</a>
                        <div class="dropdown-menu" aria-labelledby="dropdown01">
                            <a class="dropdown-item" href="#">Action</a>
                            <a class="dropdown-item" href="#">Another action</a>
                            <a class="dropdown-item" href="#">Something else here</a>
                        </div>
                    </li>
                </ul>
                <form class="form-inline my-2 my-lg-0">
                    <input class="form-control mr-sm-2" type="text" placeholder="Search"
                        aria-label="Search">
                    <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
                </form>
            </div>
        </nav>
        
        <div class="jumbotron">
            <div class="container">
                <h1 class="display-3">Hello, JSP!</h1>
                <p>
				JSP(Java Server Page)란?
                Java 코드가 포함된 HTML 문서를 말하며 서블릿의 복잡함을 간단하게 작성하고 해결할 수 있다.<br>
                JSP의 가장 큰 장점은 비즈니스 로직(Servlet)과 프레젠테이션 로직(HTML,JSP)을 분리할 수 있다는 것이다.<br><br>

                JSP는 서버 실행 시 Servlet의 형태로 변환되는데
                프로그래머가 직접 작성한 Servlet보다 최적화된 형태로 작성된다.	
					 

                <p>
                        <a class="btn btn-primary btn-lg" href="https://www.google.co.kr/search?q=Servlet&rlz=1C1QJDB_enKR792KR792&oq=Servlet&aqs=chrome..69i57j69i65j69i60j69i59j69i61l2.2395j0j7&sourceid=chrome&ie=UTF-8" role="button">Learn more &raquo;</a>
                </p>
            </div>
        </div>
    
        <div class="container">
            <!-- Example row of columns -->
            <div class="row">
            
                    <h2>JSP - 1</h2>
                    
                    <p>
					 스트립팅 원소(Scripting Element)는
               JSP에서 자바 코드를 직접 기술할 수 있도록 하는 기능이다.
               (JSP PDF 6p참고)
               <!-- 
                   <%! %> : 선언문 (변수 선언 시 사용)
                   <% %> : 스크립틀릿(scriptlet)
                   <%= %> : 표현식(expression)
                -->
					</p>
					
                    <p>
                        <a class="btn btn-secondary" href="views/01_sum.jsp" role="button">
                        View details &raquo;</a>
                    </p>
			</div>                
            <div class="row">
                    <h2>JSP - 2</h2>
                    <p>
					 page 지시어(지시자)<br>
                JSP 페이지 제일 윗부분에 작성되는 JSP 파일의 속성을 기술하는 기능이다.
                Servlet은 Java Class이기 때문에 비즈니스 로직(데이터 가공 처리 및 DB연결 관련 로직 등)
                수행이 Front 관련 기술들 보다 간편하고 효율적으로 처리할 수 있다.
                <br><br>
                language는 사용할 스크립트 언어 유형을 지정<br>
                contentType은 웹 브라우저가 받아 볼 페이지의 형식, 인코딩 방식 지정 <br>
                pageEncoding은 JSP 파일에 기록된 자바코드의 인코딩 방식을 지정 <br>
                import는 자바의 import와 같은 의미 <br>
                errorPage / isErrorPage 는 오류페이지 관리
                </p>
                <p>
                    <a class="btn btn-secondary" href="views/02_page.jsp" role="button">View
                        details &raquo;</a>
                </p>
                </div>
                <div class="row">
                    <h2>JSP - 3</h2>
                    <p>
				정적 include로 페이지 안으로 include한 페이지의 소스가 단순 텍스트로 그대로 복사되어 포함된다.<br> 
				include한 페이지 소스 내부에 변수가 선언되어 있으면 베이스 페이지의 변수와 충돌이 날 수 있으므로
				유의해야한다.<br> 
				include 페이지 안의 import는 같이 공유된다.<br> 
				주로 페이지를 조각내어 필요한 부분에 삽입할 때 사용한다.
			</p>
                    <p>
                        <a class="btn btn-secondary" href="views/03_include.jsp" role="button">View details &raquo;</a>
                    </p>
            </div>                
            <div class="row">
                
					<h2>JSP - 4</h2>
					<p>
                        Servlet + JSP
					</p>
					<p>
						<a class="btn btn-secondary" href="views/04_pizza.jsp" role="button">View details &raquo;</a>
					</p>
            </div>
    
            <hr>
    
        </div>
    
        <footer class="container">
            <p>&copy; Company 2017-2018</p>
        </footer>
        
  </body>
</html>
 ```
<br>

### 스트립팅 원소(Scripting Element)
JSP에서 자바 코드를 직접 기술할 수 있도록 하는 기능이다.
- <%! %> : 선언문 (변수 선언 시 사용)
- <% %> : 스크립틀릿(scriptlet)
- <%= %> : 표현식(expression)

<br>


```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSP로 1부터 100까지 합 구하기</title>
</head>
<body>

	<!-- HTML 주석은 클라이언트에게 전달됨.  -->
	<%-- JSP 주석은 클라이언트에게 전달되지 않음. --%>

	<% //스크립틀릿(scriptlet) : JSP에서 자바 코드를 작성하는 부분
	int sum =0;
	for(int i=1; i<=100; i++){
		sum+=i;
	}
	System.out.println("합계 : " + sum);
	// print 구문을 이용한 출력은
	// 이클립스 콘솔창에 출력되는 구문이다.
	%>
	<!-- 표현식<%= %> -->
	<h1>1부터 100까지의 합: <%= sum %></h1>
</body>
</html>
```

<br><br>

### page 지시어(지시자)
JSP 페이지 제일 윗부분에 작성되는 JSP 파일의 속성을 기술하는 기능이다.
- language는 사용할 스크립트 언어 유형을 지정
- contentType은 웹 브라우저가 받아 볼 페이지의 형식, 인코딩 방식 지정 
- pageEncoding은 JSP 파일에 기록된 자바코드의 인코딩 방식을 지정 
- import는 자바의 import와 같은 의미 
- errorPage / isErrorPage 는 오류페이지 관리
<br>

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" errorPage="error.jsp"%>
   
   <!-- errorPage : 현재 페이지에서 오류가 발생할 경우 처리해줄 페이지를 연결하는 지시자 -->
    
    
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>
    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>page 지시자(import, errorPage)</title>
</head>
<body>

	<% 
		List<String> list = new ArrayList<>();
	
		list.add("HTML");
		list.add("Servlet");
		list.add("JSP");
		
		// 강제 에러 발생
		list.add(10, "test");
		// list의 10번 인덱스에 "test"를 추가
	%>
	
	<h3>단순 출력</h3>
	<%= list.get(0) %><br>
	<%= list.get(1) %><br>
	<%= list.get(2) %><br>
	
	
	<h3>scriptlet + expression + html</h3>
	<% for(String str : list) { %>
		 <span>출력 : <%= str %></span><br>	
	<% } %>
	
	<h3>expression + javascript</h3>
	<button type="button" onclick="test();">실행</button>
	
	<script>
		function test(){
			alert( "<%= list.get(0) %>" );
				
		}
	</script>

</body>
</html>
```
<br>

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" isErrorPage="true"%>
    <!-- isErrorPage : 현재 페이지가 에러 처리 페이지인지 확인하는 지시자 -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Error 처리 페이지</title>
</head>
<body>

	<h1>에러가 발생했습니다.</h1>
	<p>
		isErrorPage에서만 사용 가능한 객체 : exception<br> 
		예외에 대한 정보가 담겨져있음.<br>
		<%=exception%>
	</p>

</body>
</html>
```

<br><br>

### 정적 include
페이지 안으로 include한 페이지의 소스가 단순 텍스트로 그대로 복사되어 포함된다.<br> 
include한 페이지 소스 내부에 변수가 선언되어 있으면 베이스 페이지의 변수와 충돌이 날 수 있으므로 유의해야한다.

<br>

#### include.jsp
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%@ page import="java.util.Date" %>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>include</title>
</head>
<body>
	<h1>현재 시간 : 
		<%@ include file="currentTime.jsp" %>
	</h1>
</body>
</html>
```
<br>

#### currentTime.jsp
```html
<%@page import="java.text.SimpleDateFormat"%>
<%@page import="java.util.Date"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%
	Date  now = new Date();  

	SimpleDateFormat smft = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
	
	String currentTime = smft.format(now);
%>

<span style="color:blue"><%=currentTime %></span>
```
<br><br>

## 실습문제
<br>

![요청](https://user-images.githubusercontent.com/73421820/102870735-ce258480-4480-11eb-81f5-df6e5cd12ada.PNG)
<br>

![응답](https://user-images.githubusercontent.com/73421820/102870740-cf56b180-4480-11eb-892b-12821f105df6.PNG)
<br>

### pizza.jsp
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>피자 주문</title>
<style type="text/css">
	table{border: 1px solid black; border-collapse: collapse;}
	th,td {border:1px solid black; padding:10px;}
</style>
</head>
<body>
	<h1>오늘은 <span style="color: pink;">
		<%@ include file="currentTime.jsp" %></span> 입니다.
	</h1>
	<h1>피자 아카데미</h1>
	
	<table>
		<tr>
			<th>종류</th>
			<th>이름</th>
			<th>가격</th>
			<th rowspan="12"></th>
			<th>종류</th>
			<th>이름</th>
			<th>가격</th>
		</tr>
		
		<tr>
			<td rowspan="5">피자</td>
			<td>치즈피자</td>
			<td>5000</td>
			<td rowspan="12">사이드</td>
			<td>오븐구이통닭</td>
			<td>9000</td>
		</tr>
		<tr>
			<td>콤비네이션피자</td>
			<td>6000</td>
			<td>치킨스틱&윙</td>
			<td>4900</td>
		</tr>
		<tr>
			<td>포테이토피자</td>
			<td>7000</td>
			<td>치즈오븐스파게티</td>
			<td>4000</td>
		</tr>
		<tr>
			<td>고구마피자</td>
			<td>7000</td>
			<td>새우링&웨지감자</td>
			<td>3500</td>			
		</tr>
		<tr>
			<td>불고기피자</td>
			<td>8000</td>
			<td>갈릭포테이토</td>
			<td>3000</td>
		</tr>
		
		<tr>
			<td rowspan="6">토핑</td>
			<td>고구마무스</td>
			<td>1000</td>
			<td>콜라</td>
			<td>1500</td>
		</tr>
		<tr>
			<td>콘크림무스</td>
			<td>1500</td>
			<td>사이다</td>
			<td>1500</td>
			
		</tr>
		<tr>
			<td>파인애플토핑</td>
			<td>2000</td>
			<td>갈릭소스</td>
			<td>500</td>
		</tr>
		<tr>
			<td>치즈토핑</td>
			<td>2000</td>
			<td>피클</td>
			<td>300</td>
		</tr>
		<tr>
			<td>치즈크러스트</td>
			<td>2000</td>
			<td>핫소스</td>
			<td>100</td>
		</tr>
		<tr>
			<td>치즈바이트</td>
			<td>3000</td>
			<td>파마산 치즈가루</td>
			<td>100</td>
		</tr>
	</table>
	
	<br><br>
	
	<form action="/JSPProject/confirmPizza.do" name="myInfoForm" method="post">
		피자&nbsp;&nbsp;&nbsp; : 
		<select id="pizza" name="pizza" required>
			<option value="치즈피자">치즈피자</option>
			<option value="콤비네이션피자">콤비네이션피자</option>
			<option value="포테이토피자">포테이토피자</option>
			<option value="고구마피자">고구마피자</option>
			<option value="불고기피자">불고기피자</option>
		</select>
		
		<br>
		
		토핑&nbsp;&nbsp;&nbsp; :
		<input type="checkbox" name="topping" id="topping1" value="고구마무스">고구마무스
		<input type="checkbox" name="topping" id="topping2" value="콘크림무스">콘크림무스
		<input type="checkbox" name="topping" id="topping3" value="파인애플토핑">파인애플토핑
		<input type="checkbox" name="topping" id="topping4" value="치즈토핑">치즈토핑
		<input type="checkbox" name="topping" id="topping5" value="치즈크러스트">치즈크러스트
		<input type="checkbox" name="topping" id="topping6" value="치즈바이트">치즈바이트
		
		<br>
		
		사이드 :
		<input type="checkbox" name="side" id="side1" value="오븐구이통닭">오븐구이통닭
		<input type="checkbox" name="side" id="side2" value="치킨스틱&윙">치킨스틱&윙
		<input type="checkbox" name="side" id="side3" value="치즈오븐스파게티">치즈오븐스파게티
		<input type="checkbox" name="side" id="side4" value="새우링&웨지감자">새우링&웨지감자
		<input type="checkbox" name="side" id="side5" value="갈릭포테이토">갈릭포테이토
		<input type="checkbox" name="side" id="side6" value="콜라">콜라
		<input type="checkbox" name="side" id="side7" value="사이다">사이다
		<input type="checkbox" name="side" id="side8" value="갈릭소스">갈릭소스
		<input type="checkbox" name="side" id="side9" value="피클">피클
		<input type="checkbox" name="side" id="side10" value="핫소스">핫소스
		<input type="checkbox" name="side" id="side11" value="파마산 치즈가루">파마산 치즈가루
		
		<br>
		<br>
		
		<button type="submit">주문하기</button>
	</form>	
</body>
</html>
```
<br>

### pizzaservlet.java
```java
package com.kh.practice.controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class PizzaServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public PizzaServlet() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	request.setCharacterEncoding("UTF-8");
	
	// 선택된 피자와 토핑, 사이드메뉴의 총 가격을 계산하여 
	// 요청 위임 객체에 추가 후
	// 응답 화면용 JSP를 만들어 출력하기
	
	String pizza = request.getParameter("pizza");
	String[] topping = request.getParameterValues("topping");
	String[] side = request.getParameterValues("side");
	
	int price = 0;
	switch(pizza) {
	case "치즈피자" : price = 5000; break;
	case "콤비네이션피자" : price = 6000; break;
	case "포테이토피자" : price = 7000; break;
	case "고구마피자" : price = 7000; break;
	case "불고기피자" : price = 8000; break;
	}
	
	
	for(int i=0; i<topping.length;i++) {
		switch(topping[i]) {
		case "고구마무스" : price+=1000 ;break;
		case "콘크림무스" : price+=1500 ;break;
		case "파인애플토핑" : price+= 2000;break;
		case "치즈토핑" : price+= 2000;break;
		case "치즈크러스트" : price+= 2000;break;
		case "치즈바이트" : price+= 3000;break;
		}
	}
	
	
	for(int i=0; i<side.length;i++) {
		switch(side[i]) {
		case "오븐구이통닭" : price+=9000 ;break;
		case "치킨스틱&윙" : price+=4900 ;break;
		case "치즈오븐스파게티" : price+= 4000;break;
		case "새우링&웨지감자" : price+= 3500;break;
		case "갈릭포테이토" : price+= 3000;break;
		case "콜라" : price+= 1500;break;
		case "사이다" : price+= 1500;break;
		case "갈릭소스" : price+= 500;break;
		case "피클" : price+= 300;break;
		case "핫소스" : price+= 100;break;
		case "파마산 치즈가루" : price+= 100;break;
		}
	}
	
	
	// 위임
	
	RequestDispatcher view = request.getRequestDispatcher("views/result.jsp");
	
	
	request.setAttribute("topping", String.join(",", topping));
	request.setAttribute("side", String.join(",", side));
	request.setAttribute("price", price);
	
	// 요청 위임 진행
	view.forward(request, response);
	}


	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}

```
<br>

###  result.jsp
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
  
<% 
	// 위임받은 파라미터 값 얻어오기    
	String pizza = request.getParameter("pizza");

	// 가공데이터 얻어오기
	String topping = (String)request.getAttribute("topping");
	String side = (String)request.getAttribute("side");
	int price = (int)request.getAttribute("price");
	
%>


<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>피자메뉴 응답페이지</title>
<style>
	span.pizza{ color : red; }
	span.topping{ color : green; }
	span.side{ color : blue; }
</style>


</head>
<body>

결과 출력용 화면
<h1>주문 내역</h1>
<h2>피자는 <span class="pizza"><%= pizza %></span>, 
  토핑은 <span class="topping"><%= topping %></span>, <br>
  사이드는 <span class="side"><%= side %></span> 주문하셨습니다.</h2>
<br>
<br>


<h3>총합 : <span style="text-decoration: underline"><%=price+"원" %></span></h3>

<br>
<br>
<h1 style="color:lightpink">๑◕‿‿◕๑ つ━☆*。맛점 맛저~</h1>


</body>
</html>
```
<br>
<br>
<br>
<br>