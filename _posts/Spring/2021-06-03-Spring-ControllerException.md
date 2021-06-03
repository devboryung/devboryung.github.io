---
title: "[Spring 실습] Controller의 Exception 처리"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## Controller의 Exception 처리

Controll를 작성할 때 예외 상황을 고려하면 처리해야 하는 작업이 증가한다.<br>
스프링 MVC에서는 다음 방식으로 처리할 수 있다.<br>
- @ExceptionHandler와 @ControllerAdvice를 이용한 처리
- @ResponseEntity를 이용하는 예외 메시지 구성

<br><br>


### @ControllerAdvice

@ControllerAdvice는 AOP(aspect-Oriented-Programming)를 이용하는 방식이다.<br>

AOP는 핵심적인 로직은 아니지만 프로그램에서 필요한 '공통적인 관심사'는 분리하는 개념이다.<br>
Controller를 작성할 때 메서드의 모든 예외사항을 전부 핸들링해야 한다면 중복적이고 많은 양의 코드를 작성해야 하지만 AOP 방식을 이용하면 공통적인 예외사항에 대해서는 별도로 @ControllerAdvice를 잉요해서 분리하는 방식이다.<br>


<br>


> CommonExceptionAdvice 클래스 생성

예외 처리를 목적으로 생성하는 클래스도 별도의 로직처리는 하지 않는다.<br>


```java
package org.zerock.exception;

import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

import lombok.extern.log4j.Log4j;

@ControllerAdvice
@Log4j
public class CommonExceptionAdvice {
	
	@ExceptionHandler(Exception.class)
	public String except(Exception ex, Model model) {
		
		log.error("Exception ..." + ex.getMessage());
		model.addAttribute("exception", ex);
		log.error(model);
		return "error_page";
	}
}
```
<br>

CommonExceptionAdvice 클래스에 @ControllerAdvice라는 어노테이션과 @ExceptionHandler라는 어노테이션을 사용하고 있다.<br>

@ControllerAdvice는 해당 객체가 스프링의 컨트롤러에서 발생하는 예외를 처리하는 존재임을 명시하는 용도로 사용한다.<br>
@ExceptionHandler는 해당 메서드가 들어가는 예외 타입을 처리한다.<br>
@ExceptionHandler 어노테이션의 속성으로는 Exception 클래스 타입을 지정할 수 있으며, 해당 클래스는 Exception.class를 지정하였으므로 모든 예외에 대한 처리가 except()만을 이용해서 처리할 수 있다.<br>

<br>

만일 특정한 타입의 예외를 다루고 싶다면 Exception.class 대신에 구체적인 예외의 클래스를 지정해야 한다.<br>
JSP 화면에서도 구체적인 메시지를 보고 싶다면 Model을 이용해서 전달하는 것이 좋다.<br>

<br><br>


>servlet-context.xml

해당 클래스는 servlet-context.xml에서 인식하지 않기 때문에 <component-scan>을 이용해서 해당 패키지의 내용을 조사하도록 한다.<br>


```xml
<context:component-scan base-package="org.zerock.exception"/>
```

CommonExceptionAdvice의 except()의 리턴값은 문자열이므로 JSP 파일의 경로가 된다.<br>
JSP는 error_page.jsp이므로 /WEB-INF/views 폴더 내에 작성해야 한다.<br>

> error_page.jsp

```html
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page session="false" import="java.util.*" %>
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
    
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<h4><c:out value="${exception.getMessage()}"></c:out></h4>
	
	<ul>
		<c:forEach items="${esception.getStackTrace() }" var="stack">
			<li><c:out value="${stack}"></c:out></li>
		</c:forEach>
	</ul>
</body>
</html>
```


<br>

일부러 에러를 발생시킨 경우 화면에 예외 메세지가 출력된다.<br>


<br>
<br>

### 404 에러 페이지

WAS의 구동 중 가장 흔한 에러와 관련된 HTTP 상태 코드는 '404'와 '500' 에러 코드이다.<br>
500 메시지는 'Internal Server Error' 이므로 @ExceptionHandler를 이용해서 처리한다.<br>
404 메시지는 잘못된 URL을 호출할 때 보여진다.<br>


스프링 MVC의 모든 요청은 DispatcherServlet을 이용해서 처리되므로 404 에러도 같이 처리할 수 있도록 web.xml을 수정해야 한다.<br>

> web.xml

```xml
<!-- Processes application requests -->
<servlet>
	<servlet-name>appServlet</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
	</init-param>
	<init-param>
		<param-name>throwExceptionIfNoHandlerFound</param-name>
		<param-value>true</param-value>
	</init-param>
	<load-on-startup>1</load-on-startup>
</servlet>
```

<br>

> CommonExceptionAdvice클래스

```java
	@ExceptionHandler(NoHandlerFoundException.class)
	@ResponseStatus(HttpStatus.NOT_FOUND)
	public String handler404(NoHandlerFoundException ex) {
		return "custom404";
	}
```

> custom404.jsp

```java
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>

 <h1>해당 URL은 존재하지 않습니다.</h1>

</body>
</html>
```
<br>

브라우저에서 존재하지 않는 URL을 호출하면 custom404.jsp 페이지가 보인다.<br>

![image](https://user-images.githubusercontent.com/73421820/120643075-a9fab480-c4b0-11eb-9d37-4d67e194e0cc.png)




<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
