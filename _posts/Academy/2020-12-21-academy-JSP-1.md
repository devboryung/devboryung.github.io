---
title: "2020년 12월 21일"
excerpt: "JSP - 1"
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

##   Servlet

### Servlet이란??
- 웹 서비스를 위한 자바 클래스
- 웹 프로그래밍에서 클라이언트의 요청(Request)을 처리하고, 그 결과를 다시 클라이언트에게 전송(응답, Response)하는 Servlet클래스의 구현 규칙을 지킨 자바 프로그래밍 기술이다.


### Servlet특징
- 클라이언트의 요청에 대해 동적으로 작동하는 웹 애플리케이션 컴포넌트.
- HTML을 사용하여 요청에 응답
- java thread를 이용하여 동작
- mvc패턴에서 controller로 이용
- http프로토콜 서비스를 지원하는javax.servlet.http.HttpServlet 클래스를 상속 받음


<br><br>

```html
<!DOCTYPE html>
<html lang="ko">
    <head>
    <meta charset="UTF-8">
	    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css" 
        integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
        
	    <title>Servlet 예제</title>
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
                <h1 class="display-3">Hello, Servlet!</h1>
                <p>
                서블릿이란? 웹 서비스를 위한 자바 클래스를 말하며 <br>
                
                자바를 사용해 웹을 만들기 위한 기술로 
                javax.servlet.http.HttpServlet 클래스를 상속받는다.<br>
                
                다시 말해 기존의 java파일에 웹 페이지를 구현하기 위한 html코드가 들어간 구조<br>
                
                클라이언트의 요청을 처리하고, 그 결과를 HTML을 이용하여 응답할 수 있는 페이지를 만들어
                클라이언트에게 다시 전달하는 구현 규칙이 있다. <br>
                
                단, Java 코드에 작성된 HTML 코드 변경 시 재 컴파일을 해야되는 단점이 있다.
                    

            <p>
                    <a class="btn btn-primary btn-lg" href="https://www.google.co.kr/search?q=Servlet&rlz=1C1QJDB_enKR792KR792&oq=Servlet&aqs=chrome..69i57j69i65j69i60j69i59j69i61l2.2395j0j7&sourceid=chrome&ie=UTF-8" role="button">Learn more &raquo;</a>
            </p>
            </div>
        </div>
    
        <div class="container">
            <!-- Example row of columns -->
            <div class="row">
            
                    <h2>Servlet - 1(GET)</h2>
                    
                <p>
                    GET 방식은 URL에 변수(데이터)를 포함시켜 요청하는 방법 중 하나로<br>
                    요청에 사용되는 데이터가 주소에 노출되어 보안 유지가 불가능 하다.<br>
                    ex) 로그인 시 아이디, 비밀번호가 주소창에 노출됨.<br><br>
                    
                    GET방식에서 데이터를 Header에 포함하여 전송하는데 Body는 보통 빈 상태로 전송 되며 
                    Header내용 중 Body의 데이터를 설명하는 Content-Type헤더필드도 들어가지 않는다.
                    Header에 데이터가 들어가기 때문에 전송하는 길이에 제한이 있으며 초과 데이터는 절단된다. 
                    캐싱이 가능하다.
                </p>
                
                <p>
                    <a class="btn btn-secondary" href="views/testServlet1.html" role="button">
                    View details &raquo;</a>
                </p>
			</div>                
            <div class="row">
                    <h2>Servlet - 2(POST)</h2>
                <p>
                    POST방식은 URL에 변수(데이터)를 노출하지 않고 요청하는 것으로 보안 유지가 가능하다.
                    데이터를 Body에 포함하여 전송하므로 
                    Body의 데이터를 설명하는 Content-Type헤더필드가 들어가고 어떤 타입인지 명시해줘야 한다.<br>
                    Body에 데이터가 들어가 전송 길이는 제한이 없지만 
                    최대 요청 받는 시간(Time Out)이 존재해 페이지 요청, 기다리는 시간이 있다.
                    URL에 데이터가 노출되지 않아 즐겨찾기나 캐싱이 불가능하다.
                </p>
                <p>
                    <a class="btn btn-secondary" href="views/testServlet2.html" role="button">View
                        details &raquo;</a>
                </p>
        </div>                
        <div class="row">
                <h2>Servlet - 3(비즈니스 로직)</h2>
                <p>			

                </p>
                <p>
                    <a class="btn btn-secondary" href="#l" role="button">View details &raquo;</a>
                </p>
        </div>                
        <div class="row">
            
                <h2>Servlet - 4(Servlet + JSP)</h2>
                <p>

                </p>
                <p>
                    <a class="btn btn-secondary" href="" role="button">View details &raquo;</a>
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
<br><br>


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
  
  <!-- Servlet 생성 및 url 연결 -->
  <servlet>
  	   <servlet-name>Test1</servlet-name>
	   <servlet-class>
	   		com.kh.servlet.controller.TestServlet1
	   </servlet-class>
  </servlet>
  <servlet-mapping>
  		<servlet-name>Test1</servlet-name>
  		<url-pattern>/testServlet1.do</url-pattern>
  </servlet-mapping>
  
  <servlet>
  		<servlet-name>Test2</servlet-name>
  		<servlet-class>com.kh.servlet.controller.TestServlet2</servlet-class>
  </servlet>
  <servlet-mapping>
  		<servlet-name>Test2</servlet-name>
  		<url-pattern>/testServlet2.do</url-pattern>
  </servlet-mapping>
  

</web-app>
```
<br/>

### GET방식
- URL에 변수(데이터)를 포함시켜 요청
 보안 유지를 안 하기 때문에 로그인 같은 경우는 get방식으로 하면 부적합
- 데이터를 HTTP Header에 포함하여 전송
 GET방식에서 바디는 보통 빈 상태로 전송 되며
 헤더의 내용 중 Body의 데이터를 설명하는 Content-type헤더필드도 들어가지 않음
- 전송하는 길이 제한(보내는 길이가 너무 길면 초과데이터는 절단됨)
- 캐싱 가능 (ex. 즐겨찾기, 북마크)
 (한번 접근 후, 또 요청할 시 빠르게 접근하기 위해 데이터를 저장시켜 놓는 것)

<br>

```html
<!-- html -->
<!DOCTYPE html>
<html lang="ko">
    <head>
    <meta charset="UTF-8">
        <title>개인정보 입력 - GET</title>
        <style>
            ul{ list-style-type: none; }

            li{  padding-bottom: 20px; }
        </style>
    </head>
    <body>
        <h2>개인 정보 입력 - GET</h2>
        당신의 개인 정보를 입력하겠습니다.<br>
        데이터 입력 후 확인 버튼을 눌러주세요.<br>

        <form action="/ServletProject/testServlet1.do" method="get" name="testFrm">
            <!-- /ServletProject을 앞에 넣는 이유는 현재 Context root 값을 수정하지 않고
                 프로젝트 이름으로 그대로 들어가있음 -->
            <!-- 이렇게 해주면 뒤에 Servlet에서 매핑할 URL을 등록할 때 
                /testServlet1.do 라고만 해도 앞에 Context root가 붙은 것으로 인식하여
                 우리는 직접적으로 이렇게 붙여준다. -->
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


```java
// java 
package com.kh.servlet.controller;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class TestServlet1 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public TestServlet1() {
        super();
    }

	public void init(ServletConfig config) throws ServletException {
		System.out.println("init() 메소드 호출(servlet 객체 생성됨)");
		// init() 메소드 다음에는 service()가 호출됨 
		// init() 메소드가 없을 경우 자동으로 service() 메소드가 호출됨
	}

	public void destroy() {
		System.out.println("destroy() 호출됨.");
	}

//	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
//		System.out.println("");
//	}
//  service() 메소드가 없을 경우 자동으로 요청 방식이 get/post 임을 확인하여 
//	get일 경우 doGet(), post일경우 doPost()를 호출함.
	
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		// MVC란?
		
		// Model : 비즈니스 로직 처리 역할(Service, DAO, VO, ...)
		
		// View : 입력(요청)을 받아서 결과(응답)을 보여주는 역할
		//        (HTML , Servlet , jsp)
		
		// Controller : 요청에 따라 알맞은 Model을 연결하고 Model 처리 결과를 보여줄 View를 선택하는 역할
		//				(Servlet)
		
		// HttpServletRequest : 웹 브라우저에서 사용자가 요청한 내용과 관련 정보를 받아주는 객체
		
		// HttpServletResponse : 요청 처리 결과를 요청한 클라이언트에게 전달하는 역할을 하는 객체
		
		// form 태그에서 제출된 값(Parameter) 얻어오기
		// tip. 파라미터의 자료형은 모두 String 이다.
		
		// request.getParameter("name")
		// -> 해당 name속성으로 전달된 input태그의 value를 얻어옴
		String name = request.getParameter("name");
		String gender = request.getParameter("gender");
		String age = request.getParameter("age");
		String city = request.getParameter("city");
		String height = request.getParameter("height");
		
		// 체크박스 또는 name속성이 같은 여러 input 태그 값을 얻어 올 경우
		// String[] 형태로 얻어와야 함.
		String[] foodArr = request.getParameterValues("food");
		
		// 파라미터 전달 확인
		System.out.println("name : " + name);
		System.out.println("gender : " + gender);
		System.out.println("age : " + age);
		System.out.println("city : " + city);
		System.out.println("height : " + height);
		
//		for(String food : foodArr) {
//			System.out.println("food : " + food);
//		}
		
		for(int i=0; i<foodArr.length; i++) {
			System.out.println("foodArr["+i+"]");
		}
		
		// 응답(response) 화면 준비
		response.setContentType("text/html; charset=UTF-8");
		
		// 서버에서 작성된 문자열을 출력할 스트림
		// HttpServletResponse 객체를 이용해서 얻어와 
		// 클라이언트 응답 화면과 연결
		PrintWriter out = response.getWriter();
		
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
				"    <span class='food'>%s</span>입니다.\r\n" + 
				"</body>\r\n" + 
				"</html>", name, age, city, height, gender,
						   String.join(", ", foodArr) );
		
	
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
		
		// post로 요청이 와도 doGet() 메소드로 처리하겠다.
		// == get방식이든 post방식이든 같은 방법으로 처리하겠다.
	}

}

```
<br><br>

### POST방식
- 데이터를 서버로 제출하여 추가 또는 수정하기 위해 데이터를 전송하는 방식

- URL에 변수(데이터)를 노출하지 않고 요청 데이터를 HTTP Body에 포함하여 전송
- 헤더필드 중 Body의 데이터를 설명하는 Content-Type이라는 헤더필드가 들어가고
 어떤 데이터 타입인지 명시해주어야 함

- 전송하는 길이 제한이 없음.
Body에 데이터가 들어가기 때문에 길이에 제한이 없지만
 최대 요청을 받는 시간(Time Out)이 존재해서
 페이지 요청, 기다리는 시간 존재
 - 캐싱할 수 없음.
 URL에 데이터가 노출 되지 않으므로 즐겨찾기나 캐싱 불가능
 하지만 쿼리스트링(문자열)데이터, 라디오 버튼, 텍스트 박스와 같은
 객체들의 값도 전송 가능
 
 <br>

```html
<!-- html -->
<!DOCTYPE html>
<html lang="ko">
    <head>
    <meta charset="UTF-8">
        <title>개인정보 입력 - POST</title>
        <style>
            ul{ list-style-type: none; }

            li{  padding-bottom: 20px; }
        </style>
    </head>
    <body>
        <h2>개인 정보 입력 - POST</h2>
        당신의 개인 정보를 입력하겠습니다.<br>
        데이터 입력 후 확인 버튼을 눌러주세요.<br>

        <form action="/ServletProject/testServlet2.do" method="post" name="testFrm">
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



```java
// java
package com.kh.servlet.controller;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


public class TestServlet2 extends HttpServlet {
	private static final long serialVersionUID = 1L;
    
    public TestServlet2() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("UTF-8");
		// POST방식으로 데이터 전송 시 문자 인코딩이 정해져 있지 않아
		// 아스키 코드 범위(영어, 숫자, 특수문자 몇개)를 제외한 문자는 모두 깨진다.
		
		// POST 방식으로 데이터 전송 시 문자 인코딩이 지정되지 않아
		// 전달되는 데이터의 문자 인코딩이 브라우저의 기본값(ISO-8859-1)을 따름.
		// UTF-8(이클립스 세팅)과 일치하지 않으므로
		// 파라미터를 얻어오기 전에 request의 문자 인코딩을 UTF-8로 변경해줌.
		
		// 요청 데이터(파라미터)를 모두 얻어와 각 변수에 저장
		String name = request.getParameter("name");
		String gender = request.getParameter("gender");
		String age = request.getParameter("age");
		String city = request.getParameter("city");
		String height = request.getParameter("height");
		
		String[] foodArr = request.getParameterValues("food");
		
    // 파라미터 전달 확인
            System.out.println("name : " + name);
            System.out.println("gender : " + gender);
            System.out.println("age : " + age);
            System.out.println("city : " + city);
            System.out.println("height : " + height);
            
            /*for(String food : foodArr) {
                System.out.println("food : " + food);
            }*/
            
            for(int i=0 ; i<foodArr.length ; i++) {
                System.out.println("foodArr [" + i + "] : " + foodArr[i]);
            }
            
            // 응답 화면 준비
            response.setContentType("text/html; charset=UTF-8");
            
            // 응답 화면을 내보낼 스트림 연결
            PrintWriter out = response.getWriter();
            
            
            out.println("<!DOCTYPE html>\r\n" + 
                    "<html lang=\"ko\">\r\n" + 
                    "<head>\r\n" + 
                    "    <meta charset=\"UTF-8\">\r\n" + 
                    "    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\r\n" + 
                    "    <title>TestServlet2 응답 페이지</title>\r\n" + 
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
                    "    <h1>개인 정보 입력 결과(POST)</h1>");
            
            out.printf("    <span class='name'>%s</span>님은\r\n" + 
                    "    <span class='age'>%s</span>이며\r\n" + 
                    "    <span class='city'>%s</span>에 사는\r\n" + 
                    "    키 <span class='height'>%s</span>cm 인\r\n" + 
                    "    <span class='gender'>%s</span>입니다.\r\n" + 
                    "    <br>\r\n" + 
                    "    좋아하는 음식은\r\n" + 
                    "    <span class='food'>%s</span>입니다.\r\n" + 
                    "</body>\r\n" + 
                    "</html>", name, age, city, height, gender,
                                String.join(", ", foodArr) );
                    

}

protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    doGet(request, response);
}

}

```
<br>




