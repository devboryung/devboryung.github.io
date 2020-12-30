---
title: "2020년 12월 30일"
excerpt: "Ajax"
search: true
categories: 
  - Academy
tags: 
  - Ajax
  - JavaScript
  - JQuery
toc: true
---


## Ajax란?
- Asynchronous(비동기) JavaScript and XML의 약자 
- JavaScript를 이용하여 비동기식으로 클라이언트와 서버가 데이터(XML)를 주고받는(통신) 방식
- 데이터 형식은 XML 뿐만 아닌 Text, HTML, JSON, CSV 등 다양한 형식 사용 가능 
<br>
<br>

### 비동기식 데이터 통신
- 클라이언트가 서버로 데이터 요청 후 응답을 기다리지 않고 다른 작업 수행 가능. <br>
- 추후 요청에 대한 응답이 오면 응답에 관련된 작업을 진행
<br>
<br>

### Ajax의 특징
- 전체 페이지를 갱신하지 않고 일부분만 업데이트 가능
- 사용자에게 즉각적인 반응과 풍부한 UI 경험을 제공 가능
- ActiveX나 플러그인 프로그램 설치 없이 이용 가능
- JavaScript 방식, jQuery 방식으로 구현 가능
<br><br>

### Ajax의 단점
- Ajax는 JavaScript이므로 브라우저에 따른 크로스 브라우저 처리가 필요함
- 오픈 소스로 차별화가 어려움
- 연속적인 데이터 요청 시 서버 부하 증가하여 페이지가 느려짐
- 페이지 내 복잡도가 증가하여 에러 발생 시 디버깅이 어려움
<br><br>

## DB접속 세팅하기.
- ajax Server-config -> context.xml 에 DBCP 세팅<br>
context.xml
```xml
<!-- DBCP 세팅
  DBCP : DataBase Connection Pool
  -->
<Resource 
  name="jdbc/oracle" 
  auth="Container"
  type="javax.sql.DataSource" 
  driverClassName="oracle.jdbc.OracleDriver"
  url="jdbc:oracle:thin:@127.0.0.1:1521:xe"
  username="server" 
  password="server" 
  maxTotal="20"      
  maxIdle="10"
  maxWaitMillis="-1"
/>
<!-- 
  name : JNDI 이름 Context의 lookup()을 사용하여 자원을 찾을때 사용한다. java:comp/env 디렉터리에서 찾을 수 있다.
  auth : 자원 관리 주체 지정(Application 또는 Container)
  type : REsourtece의 타입 지정
  driverClassName : JDBC 드라이버 클래스 이름.
  maxTotal : DataSource 에서 유지할 수 있는 최대 커넥션 수 	
  maxIdle : 사용되지 않고 풀에 저장될 수 있는 최대 커넥션의 개수. 음수일 경우 제한이 없음
  maxWaitMillis : 커넥션 반납을 기다리는 시간(-1은 반환될 때 까지 기다림)
  -->
```
<br>

- 데이터 베이스를 사용하는  resource  ajax web.xml에 작성하기<br>
web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>ajaxProject</display-name>
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>

    <resource-ref>
    <description>Oracle Datasource</description>
    <res-ref-name>jdbc/oracle</res-ref-name>
    <res-type>javax.sql.DataSource</res-type>
    <res-auth>Container</res-auth>
  </resource-ref>
</web-app>
```
<br>

- JDBC템플릿 추가<br>

```java
package com.kh.ajax.common;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.naming.NamingException;
import javax.sql.DataSource;

public class JDBCTemplate {
	// 1. 반복되는 Connection 객체의 생성을 간소화
	// 2. 트랜잭션 처리, close()처리의 간소화

	
	// 모든 필드, 메소드를 static으로 선언하여
	// 프로그램 구동 시 static 메모리 영역에
	// 모든 클래스 내용을 로드하여 하나의 객체 모양을 띄게함.
	
	// 하나의 공용 커넥션 참조 변수 선언
	private static Connection conn = null;
	// private 선언 이유 : 직접 접근 시 null 값이나
	// 닫혀진 커넥션 객체를 가져갈 수 있는 확률이 있어서 이를 미연에 차단.
	
	// 해당 클래스의 내용은 모두 static에서 객체의 모양을 이루기 때문에
	// 추가적인 객체 생성을 막기위해 private 사용
	private JDBCTemplate() { }
	
	
	public static Connection getConnection() throws NamingException, SQLException {
		
		/* 이전 JDBC 수업때는 요청 시 마다 Connection을 새로 만들어 반환하는 과정을 거쳤다.
		 * --> 요청 시간이 지연됨. (비효율적)
		 * 
		 * WAS(Web Application Server, Tomcat(== servlet Container)) 가
		 * 미리 DB에 접속할 수 있는 객체(Connection)를 일정 개수를 만들어 두고
		 * 요청이 올 때 마다 만들어 둔 객체를 전달하고 사용 완료 후 반환받는
		 * Connection Pool을 사용함.
		 * */
		
	      // JNDI(Java Naming and Directory Interface API)
	      /*디렉터리 서비스에 접근하는데 사용하는 API
	      어플리케이션은 JNDI를 사용하여 서버의 resource를 찾는다.
	      특히 JDBC resource를 data source라고 부른다.
	      
	      Resource를 서버에 등록할 때 고유한 JNDI 이름을 붙이는데, JNDI 이름은 디렉터리 경로 형태를 가진다.
	      예를 들어 data source의 JNDI 이름은 'jdbc/mydb' 형식으로 짓는다.
	      
	       서버에서 'jdbc/oracle'라는 DataSource를 찾으려면 
	      'java:comp/env/jdbc/oracle'라는 JNDI 이름으로 찾아야 한다. 
	      즉 lookup() 메소드에 'java:comp/env/jdbc/oracle'를 인자값으로 넘긴다.
	      
	      */
	      
	      // Servers에 존재하는 context.xml 파일을 찾는 작업
	      Context initContext = new InitialContext();
	      Context envContext  = (Context)initContext.lookup("java:/comp/env");  
	      // java:comp/env   응용 프로그램 환경 항목
	      
	      // context.xml 파일에서 name이 "jdbc/oracle"인 DataSource를 얻어옴
	      // DataSource : DriverManager를 대체하는 객체로 
	      // Connection 생성, Connectoin pool을 구현하는 객체
	      DataSource ds = (DataSource)envContext.lookup("jdbc/oracle");

	      conn = ds.getConnection(); // DataSource에 의해 미리 만들어진 Connection 중 하나를 얻어옴.
	      
	      conn.setAutoCommit(false);
		
		
		return conn;
	}
	
	// 트랜잭션 처리(commit, rollback)도 공통적으로 사용함
	// static으로 미리 선언 코드길이 감소 효과 + 재사용성
	public static void commit(Connection conn) {
		try {
			if(conn != null && !conn.isClosed()) {
				// 참조하고 있는 커넥션이 닫혀있지 않은 경우
				conn.commit();
			}
		}catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void rollback(Connection conn) {
		try {
			if(conn != null && !conn.isClosed()) {
				// 참조하고 있는 커넥션이 닫혀있지 않은 경우
				conn.rollback();
			}
		}catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	// DB 연결 자원 반환 구문도 static으로 작성
	public static void close(Connection conn) {
		try {
			if(conn != null && !conn.isClosed()) {
				// 참조하고 있는 커넥션이 닫혀있지 않은 경우
				conn.close();
			}
		}catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void close(ResultSet rset) {
		try {
			if(rset != null && !rset.isClosed()) {
				rset.close();
			}
		}catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	// Statement, PreparedStatement 두 객체를 반환하는 메소드
	// 어떻게? PreparedStatement는 Statement의 자식이므로
	// 매개변수 stmt에 다형성이 적용되서 자식 객체인 PreparedStatement를 참조 가능
	public static void close(Statement stmt) {
		try {
			if(stmt != null && !stmt.isClosed()) {
				stmt.close();
			}
		}catch (Exception e) {
			e.printStackTrace();
		}
	}

}
```
<br>

- ojdbc6 라이브러리 lib폴더에  추가<br>

![ojdbc6](https://user-images.githubusercontent.com/73421820/103346446-5b975300-4ad7-11eb-8db6-096deadeceee.png)
<br><br>


## Ajax 연습
연습에 필요한 내용. 서버를 켜면 바로 보이는 화면
<br>

#### 출력화면
<br>

![index jsp 화면](https://user-images.githubusercontent.com/73421820/103346698-263f3500-4ad8-11eb-85b4-58b5f8694e7c.png)
<br><br>

index.jsp

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
<title>Ajax 연습하기</title>
<style>
	#result-area{
		width: 100%;
		height : 400px;
		border : 1px solid black;
	}
	
	#side{
		position : fixed;
		right : 0;
	}
</style>
</head>
<body>
<div class="container-fluid">
	<div class="row mt-5">
		<div class="col-md-8">
			<div class="list-group">
			
      <h3 class="list-group-item list-group-item-action active">
        Javascript 방식의 Ajax
      </h3>
  
  <!-- 1. GET 방식으로 서버에 데이터 전송 및 응답 -->
  <div class="list-group-item">
    <h5 class="list-group-item-heading">
      <span class="text-primary">
        1. 버튼 클릭 시 GET 방식으로 서버에 데이터 전송 및 응답
      </span>
      <button id="jsBtn1" class="btn btn-primary float-right">
          GET 방식 전송
      </button>
    </h5>
  </div>
  
  <!-- 2. POST 방식으로 서버에 데이터 전송 및 응답 -->
  <div class="list-group-item">
    <h5 class="list-group-item-heading">
      <span class="text-primary">
          2. 버튼 클릭 시 POST 방식으로 서버에 데이터 전송 및 응답
      </span>
      <button id="jsBtn2" class="btn btn-primary float-right">
          POST 방식 전송
      </button>
    </h5>
  </div>
  
  <!-------------------------------------------------------------------------------------------->
  
  <h3 class="list-group-item list-group-item-action active bg-warning border-warning mt-3">
    jQuery 방식의 Ajax
  </h3>
  
  <!-- 1. 버튼 클릭 시  GET GET으로 서버에 데이터 전송 및 응답 -->
  <div class="list-group-item">
    <h5 class="list-group-item-heading">
      <span class="text-warning">
          1. 버튼 클릭 시  GET 방식으로 서버에 데이터 전송 및 응답
      </span><br><br>
          입력 : <input type="text" id="input1">
      <button id="jqBtn1" class="btn btn-warning float-right">
          GET 방식 전송
      </button>
    </h5>
  </div>
  
  <!-- 2. 버튼 클릭 시 POST 방식으로 서버에 데이터 전송 및 응답 -->
  <div class="list-group-item">
    <h5 class="list-group-item-heading">
      <span class="text-warning">
          2. 버튼 클릭 시 POST 방식으로 서버에 데이터 전송 및 응답
      </span><br><br>
      입력 : <input type="text" id="input2">
      <button id="jqBtn2" class="btn btn-warning float-right">
          POST 방식 전송
      </button>
    </h5>
  </div>
  
  <!-- 실시간 아이디 중복 검사 -->
  <div class="list-group-item">
    <h5 class="list-group-item-heading">
      <span class="text-warning">
          실시간 아이디 중복 검사
      </span><br><br>
      입력 : <input type="text" id="inputId">
      <span id="idDupResult"></span>
    </h5>
  </div>
  
  <!-- 3. [JSON]서버로 기본형 데이터 전송 후, 응답을 객체(Obejct)로 받기 -->
  <div class="list-group-item">
    <h5 class="list-group-item-heading">
      <span class="text-warning">
          3. [JSON]서버로 기본형 데이터 전송 후, 응답을 객체(Obejct)로 받기
      </span><br>
      1 ~ 5 사이 입력 : <input type="text" id="input3">
      <button id="jqBtn3" class="btn btn-warning float-right">
        전송
      </button>
    </h5>
  </div>
  
  
  <!-- [JSON] 실시간으로 입력받은 아이디와 일치하는 회원이 존재하면 회원 정보 조회하기 -->
  <div class="list-group-item">
    <h5 class="list-group-item-heading">
      <span class="text-warning">
          입력받은 아이디와 일치하는 회원이 존재하면 회원 정보 조회하기
      </span><br>
      ID 입력 : <input type="text" id="inputId2">
      <button id="selectMemberBtn" class="btn btn-warning float-right">
          조회
      </button>
    </h5>
  </div>
  
</div>
</div>
  
<div class="col-md-4" id="side">
  <div class="list-group" >
    <h3 class="list-group-item list-group-item-action active bg-secondary border-secondary">
      Ajax 수행 결과
    </h3>
    
    <div id="result-area">
      
    </div>
      
			</div>
		</div>
	</div>
</div>


<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script src="resources/js/jsScript.js"></script>
<script src="resources/js/jQueryScript.js"></script>
</body>
</html>
```
<br>


## JavaScript방식 Ajax
※ Ajax는 브라우저 내장 객체인 XMLHttpRequest를 이용하여 비동기식으로 데이터를 송수신함<br>
( IE 브라우저는 ActiveXObject 객체 사용 )
1. 객체 생성 -> script문에 요청을 위한 XMLHttpRequest객체 생성
2. 서버 응답 처리 함수 생성 및 지정 -> onreadystatechange에 함수 지정
- 지정되는 함수에 포함될 내용<br>
    1) 응답 상태 확인 -> readyState(서버 응답 상태 확인) , status(Http 응답 상태 코드 확인)<br>
    2) 응답 완료(서버 응답 데이터 접근) -> responseText / responseXML<br>
3. 요청 방식, 대상(서버), 동기/비동기 지정 -> open() 메소드 호출
4. 대상(서버)에 전송 -> send() 메소드 호출


## JavaScript용 스크립트
자바스크립트용 스크립트를 모아 둠

<br>

jsScript.js

```js
// 자바스크립트 방식의 ajax
// XMLHttpRequest 객체 생성
// 변수 선언
var xhr;
// 크로스 브라우저 대처 작업 진행
function getXMLHttpRequest(){
    // IE7 이상 이거나 또는 그 외 브라우저(크롬,엣지,웨일...)
    if(window.XMLHttpRequest){
        return new XMLHttpRequest();
    }
    // IE6 이하 버전
    else if(window.ActiveXObject){
        try{
            return new ActiveXObject("Microsoft.XMLHTTP");
        }catch(e){
            return null;
        }
    }
    // ajax를 지원하지 않는 브라우저
    else{
        return null;
    }
}
// contextPath를 구하는 함수
function getContextPath() {
    var hostIndex = location.href.indexOf( location.host ) + location.host.length;
    return location.href.substring( hostIndex, location.href.indexOf('/', hostIndex + 1) );
};

// 버튼 요소를 얻어와 저장
var jsBtn1 = document.getElementById("jsBtn1");
var jsBtn2 = document.getElementById("jsBtn2");
```
<br>

### GET방식

#### 출력 화면
<br>

![get방식](https://user-images.githubusercontent.com/73421820/103346887-ba110100-4ad8-11eb-9851-b7203ce62c48.png)
<br><br>

```java
// 1. GET방식으로 서버에 데이터 전송 및 응답 받기
jsBtn1.addEventListener("click",function(){
  
  // 1)XMLHttpRequest 객체 생성
  xhr = getXMLHttpRequest();

  // 2) on ready state change : Ajax 통신이 성공한 경우에 대한 동작 지정
  xhr.onreadystatechange = function(){

      // Ajax 통신이 성공했는지 검사
      // 2-1) readyState : 서버 응답 상태 확인(Ajax 통신 상황 확인)
      if(xhr.readyState == 4){ //  4 : 요청처리가 완료 됐고, 응답 준비 되어있음
          
          // 2-2) status : HTTP 응답 상태 코드(응답이 정상적으로 이루어졌는가)
          if(xhr.status==200){ // 200 : 응답 성공
              console.log("ajax 통신 성공!");

              // responseText : 응답 데이터 문자열 반환
              document.getElementById("result-area").innerHTML 
                  = xhr.responseText;
              // result-area : 화면의 Ajax수행 결과 칸
          }
      }
  }
  // 3) open() : 서버와 데이터 교환 시 필요한 정보를 입력
  xhr.open("GET", "jsAjax1.do?name=홍길동&age=20", true);

  // 4) send() : 서버로 데이터 교환 요청
  xhr.send();
});
```

#### readyState

<br>
![readyState](https://user-images.githubusercontent.com/73421820/103346737-3fe07c80-4ad8-11eb-8e46-3ccc74db7ff7.png)
<br><br>


### jsAjax1.do 주소를 받는 Servlet

JsAjaxServlet1.java

```java
package com.kh.ajax.js.controller;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/jsAjax1.do")

public class JsAjaxServlet1 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		
		// 전달된 파라미터 변수에 저장
		String name = request.getParameter("name");
		String age = request.getParameter("age");
		
		// 파라미터 확인용 출력 구문
		System.out.println(name + "/" + age);
		
		// 응답 문자 인코딩 지정
		response.setCharacterEncoding("UTF-8");
		
		// 응답 데이터 전송 스트림 연결
		PrintWriter out = response.getWriter();
		
		out.append("서버에서 비동기로 전송한 값<br>");
		out.append("이름은 " + name + "이고, 나이는 " + age + "세 입니다.");
		
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		doGet(request, response);
	}

}
```
<br><br>



### POST방식

#### 출력 화면
<br>

![post방식](https://user-images.githubusercontent.com/73421820/103346893-baa99780-4ad8-11eb-857f-551bd377bc3e.png)
<br><br>

```java
// 2. POST 방식으로 서버에 데이터 전송 및 응답
jsBtn2.addEventListener("click",function(){

    xhr = getXMLHttpRequest();

    xhr.onreadystatechange = function(){
        if(xhr.readyState == 4){
            if(xhr.status == 200){
                document.getElementById("result-area").innerHTML
                 = xhr.responseText;
            }
        }
    }

    // 3. open() : 서버와 데이터 교환 시 필요한 정보 입력
    xhr.open("POST", "jsAjax2.do", true);

    // * POST 방식 데이터 전송 시 send() 호출 전 Mime Type을 설정해줘야함.
    xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded;');

    // 4. send() : 서버로 데이터 교환 요청
    // POST 방식 전송 시 send의 매개변수로 전달할 데이터를 쿼리스트링 형태로 작성
    xhr.send("pname=노트북&price=1,000,000");

});
```
<br>

### jsAjax2.do 주소를 받는 servlet

JsAjaxservlet2.java

```java
package com.kh.ajax.js.controller;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/jsAjax2.do")
public class JsAjaxServlet2 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		// POST 방식으로 데이터 전달 -> 문자 인코딩 깨짐
		request.setCharacterEncoding("UTF-8");
		
		String pname = request.getParameter("pname");
		String price = request.getParameter("price");
		
		System.out.println(pname + "/" + price);
		
		// 응답 데이터 문자 인코딩 처리
		response.setCharacterEncoding("UTF-8");
		
		// 응답 데이터 전송 스트림 연결
		PrintWriter out = response.getWriter();
		
		out.append("서버에서 비동기 통신으로 전송한 값<br>");
		out.append(pname + "의 가격은" + price+"원 입니다.");	
	}
	protected void doPost(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		doGet(request, response);
	}
}
```
<br><br>




## jQuery방식 Ajax
- JavaScript 방식보다 구현 방법이 간단함  코드 길이 감소
- 직관적이며 다양한 방법의 코딩 가능
- 크로스 브라우저 처리를 jQuery가 자동으로 해결
   ->JavaScript방식 XMLHttpRequest 객체 생성 시 브라우저 검사 생략 가능

<br><br>

## jQuery방식 Ajax 주요 속성
- url : 요청(request) 데이터를 전송할 URL
- data : 서버로 전송할 요청 Parameter 설정
- type : Http 요청 방식 지정 (GET / POST)
- datatype :  서버의 응답(response)데이터의 형식(xml,text,json,html 등)을 지정
(미작성 시 자동으로 판단하여 지정)
- success(data) :ajax 통신 성공 시 호출되는 함수를 지정
(매개변수로 응답 데이터를 받음(data))
- error : ajax 통신 실패 시 호출되는 함수를 지정
- complete : ajax통신 성공여부와 관계없이 통신 완료 후 실행되는 함수 지정
- async : 비동기(true) / 동기(false) 지정


## jQuery용 스크립트
제이쿼리용 스크립트를 모아 둠

<br>

### GET 방식

#### 출력 화면
<br>

![get방식](https://user-images.githubusercontent.com/73421820/103347430-5b4c8700-4ada-11eb-96a3-8bd119cb61a8.png)
<br><br>

jQueryScript.js

```js
// 1. 버튼 클릭 시 GET 방식으로 서버에 입력값 전달 및 응답
$("#jqBtn1").on("click",function(){
    
  $.ajax({
      // url : 요청을 보낼 주소(필수!!)
      url : "jqTest1.do", 
      type : "GET", // type : 데이터 전송 방식
      data : { "input" : $("#input1").val() }, // data : 전달할 데이터
      success : function(result){
          // success : ajax 통신 성공 시 동작
          // 매개변수 result : 서버 응답 데이터

          $("#result-area").html(result);
      },   
      error : function(){
          // error : ajax 통신 실패 시 동작
          console.log("ajax 통신 실패");

      },
      complete : function(){
          // complete : ajax 통신 성공 여부와 관계없이 수행
          console.log("통신 성공 여부 관계 없이 수행됨.");
      }

  });

});
```
<br><br>

### jqTest1.do 주소를 받는 servlet
JqueryAjaxServlet1.java

```java
package com.kh.ajax.jquery.controller;
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/jqTest1.do")
public class JqueryAjaxServlet1 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		
		// 파라미터 저장
		String input = request.getParameter("input"); // 데이터 키 값: input
		
		System.out.println("입력값 : " + input);
		
		response.setCharacterEncoding("UTF-8");
		
		PrintWriter out = response.getWriter();
		
		out.append("제이쿼리 방식 Ajax 결과 <br>");
		out.append("입력된 값 :" +input + "<br>");
		out.append("입력된 값의 길이 : " + input.length());	
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		doGet(request, response);
	}

}
```

<br>
<br>





### GET방식 실시간 아이디 중복검사
#### 출력 화면
<br>

![중복검사](https://user-images.githubusercontent.com/73421820/103348124-73250a80-4adc-11eb-9d08-14489d1d8228.gif)
<br><br>

```js
// 실시간 아이디 중복 검사
$("#inputId").on("input",function(){
    $.ajax({
        url : "member/idDupCheck.do",
        data : {"inputId":$("#inputId").val()},
        type : "GET",
        success : function(result){
            if(result ==0){
                $("#idDupResult").text("사용 가능한 아이디 입니다.");
            }else{
                $("#idDupResult").text("이미 사용 중인 아이디 입니다.");
            }
        },

        error : function(request,status,error){
            console.log("ajax 통신 오류");
			console.log("code:"+request.status+"\n"+"message:"+request.responseText+"\n"+"error:"+error);
        }
    });
});
```
<br><br>

### member/idDupCheck.do 주소를 받는 servlet
IdDupCheckServlet.java

```java
package com.kh.ajax.jquery.controller;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.kh.ajax.jquery.model.service.MembertService;

@WebServlet("/member/idDupCheck.do")
public class IdDupCheckServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		// 전달받은 파라미터 저장
		String inputId = request.getParameter("inputId");
		
		try {
			int result = new MemberService().idDupCheck(inputId);
			
			PrintWriter out = response.getWriter();
			
			out.append(result+"");
			// jQueryScript.js success:function 으로 넘어감.
			
		}catch(Exception e) {
			e.printStackTrace();
		}
	}
	protected void doPost(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		doGet(request, response);
	}

}

```
<br>

### MemberService
```java
package com.kh.ajax.jquery.model.service;

import static com.kh.ajax.common.JDBCTemplate.*;

import java.sql.Connection;

import com.kh.ajax.jquery.model.dao.MemberDAO;

public class MemberService {
	
	private MemberDAO dao = new MemberDAO();

	/** 아이디 중복검사 Service
	 * @param inputId
	 * @return result
	 * @throws Exception
	 */
	public int idDupCheck(String inputId) throws Exception {
		Connection conn = getConnection();
		
		int result = dao.inDupCheck(conn, inputId);
		
		close(conn);
		
		return result;
	}

}
```
<br>

### MemberDAO
```java
package com.kh.ajax.jquery.model.dao;

import static com.kh.ajax.common.JDBCTemplate.*;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class MemberDAO {	
	/**아이디 중복검사 DAO
	 * @param conn
	 * @param inputId
	 * @return result
	 * @throws Exception
	 */
	public int inDupCheck(Connection conn, String inputId) throws Exception {
		PreparedStatement pstmt = null;
		ResultSet rset = null;
		int result = 0;
		
		String query 
		   = "SELECT COUNT(*) FROM MEMBER WHERE MEMBER_ID=? AND MEMBER_STATUS='Y'";
		
		try {
			pstmt = conn.prepareStatement(query);
			pstmt.setString(1, inputId);
			
			rset = pstmt.executeQuery();
			
			if(rset.next()) {
				result = rset.getInt(1);
			}
		}finally {
			close(rset);
			close(pstmt);
		}
		return result;
	}
}
```
<br><br>
<br><br><br>
