---
title: "2020년 02월 01일"
excerpt: "Spring 1"
search: true
categories: 
  - Academy
tags: 
  - FrameWork
  - Java
  - Spring
toc: true
---

## Spring에서 사용되는 폴더
- java Resources : 자바(백엔드)에서 사용할 자원, 화면에 보여지지 않는 코드들.<br>
    -> .java 파일/ .xml / .txt / .jpg  등등
- Deployed Resources :  배포와 관련된 자원들 , 프론트 쪽 웹 설정과 관련된 파일들 <br>
    -> web.xml / .class / html / jsp / jpg,png/ js/ css
- src = 위에 두개 를 모두 묶어 놓음. ( main -> 실제 운영되는 것들, test -> test가 필요한 것들)
<br>
![폴더](https://user-images.githubusercontent.com/73421820/106600980-74939800-659e-11eb-9c5d-89072a368668.png)

<br><br>

- View 역할을 할 html ,jsp 를 담아둘 폴더 생성 <br>
deployed Resources ->  resources 폴더 안에  css, images, js , uploadImages 폴더 생성<br>
- WEB-INF폴더는  주소 접근이 차단되어 있음.<br>
왜? 자원이나 설정이 해킹되는 것을 방지하기 위해.<br>
![폴더2](https://user-images.githubusercontent.com/73421820/106601221-c2100500-659e-11eb-8761-6088f9f3508b.png)

<br><br>

## web.xml

서버를 실행하면 <br>
1. web.xml를 가장 먼저 읽음 <br>
2. web.xml을 읽으면서 가장 먼저 listener 생성<br>
3. listener를 생성할 때 context-param에 적혀있는 것을 가지고
스프링 컨테이너를 만든다.<br>  
-> 스프링 컨테이너에 들어갈 설정 내용은 root-context.xml 에 적혀있다. <br>
<br><br>

- 전 시간에 웹모듈버전을  3.1로 바꿔줬기 때문에 web.xml 의  web-app 버전을 3.1로  바꿔준다.
- spring 폴더를 Java Resources / resources 폴더로 경로를 바꿔 주었기 때문에
root-context.xml의 경로를 변경해주어야됨.
- 마찬가지로 servlet-context.xml 의 경로도 바꿔줘야됨.

<br><br>

- classpath: 란?<br> 
프로젝트 Propoerties 의 Java Build Path -> source 에 등록된 폴더들이 classpath: 이다.  알아서 폴더를 탐색해 찾아 준다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.1" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee https://java.sun.com/xml/ns/javaee/web-app_3_1.xsd">

		<!-- web.xml 파일 (배포 서술자)
			- WAS(Web Application Server :  요청에 따라 응답화면을 만들어주는 서버 == tomcat) 실행 시 제일 처음으로 읽어들이는 파일이다.
			- 서버 실행 시 초기에 설정할 내용(filter, DB접속 관련, aop, scheduling...) 또는 설정이 작성된 파일을 읽고 수행하도록 web.xml 파일에 등록한다.
			- 서블릿 실행 이전에 서블릿 초기화 용도로도 사용된다.
			
			+ 프로젝트의 웹모듈 버전과 web.xml의 버전은 같아야 한다.(같지 않으면 에러는 안나지면 태그 오류가 발생할 수 있다.)
	  -->

	<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
	<!-- context-param : 설정에 사용할 파라미터 설정 -->
	<context-param>
		<!-- contextConfigLocation : 설정 파일 경로 (변수명)  -->
		<param-name>contextConfigLocation</param-name>
		<!-- 실제 root-context.xml의 경로 
				 classpath : 프로젝트의 소스 폴더로 등록된 폴더의 경로 ( Java Build Path -> source 에서 확인 가능 )
		 -->
		<param-value>classpath:spring/root-context.xml</param-value>
	</context-param>
	
	<!-- Creates the Spring Container shared by all Servlets and Filters -->
	<!-- 모든 서블릿과 필터가 공유하는 , 스프링 컨테이너를 만드는 역할.
			 listener : context-param에 작성된 설정 파일을 읽어 스프링 컨테이너를 생성하는 리스너 객체.  
			 - 서버 실행 시 가장 먼저 로딩(pre-loading) 되어야 하는 xml 설정 문서를 읽어들이는 역할. -> 이를 이용해 스프링 컨테이너 생성
	-->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<!-- Processes application requests -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
		
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>


	<!-- 한글 깨짐을 방지하기 위한 인코딩 필터 추가  -->
	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>
			org.springframework.web.filter.CharacterEncodingFilter		
		</filter-class>
		<!-- 초기값 지정  -->
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
	</filter>
	
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
		<!-- /* : 모든 요청, 응답에 대해서 필터처리 진행  -->
	</filter-mapping>

</web-app>
```
<br><br>


## servlet-context.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

	<!-- DispatcherServlet Context: defines this servlet's request-processing infrastructure -->
	<!-- 
		DispatcherServlet : HTTP를 이용해 들어오는 모든 요청을 프레젠테이션 계층 제일 앞에 두어  중앙 집중식 처리를 해주는 프론트 컨트롤러
		모든 요청이 오면 다 잡아서 제어함.
	  -->
	
	<!-- Handler Mapping : 요청 받은 주소에 맞는 클래스 or 메소드를 연결하는 역할
			 xml에 작성하여 bean 형식으로 등록 가능하지만 비효율적이므로  @ (Annotation) 방식을 사용  
			 -> @Controller, @RequestMapping 두 어노테이션이 Handler Mapping 을 대체한다.
	 -->  
	  
	<!-- Enables the Spring MVC @Controller programming model -->
	<!-- annotation-driven :  @Controller에 요청을 보내기 위한 HandlerMapping을 bean으로 등록함. 
			해당 태그가 작성되어야만 @RequestMaping, @ExceptionHandler와 같은 어노테이션을 사용할 수 있다.
	 -->
	<annotation-driven />

	<!-- 정적 자원 호출 경로 지정  -->
	<!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
	<resources mapping="/resources/**" location="/resources/" />
	<!-- webapp 아래의 resources 폴더와 연결됨.  -->


	<!-- 
		View Resolver : Controller에서 반환된 뷰의 이름을 토대로  처리할(응답화면) view를 검색하는 역할  
		
		접두사(prefix) + 반환된 View 이름 + 접미사 (suffix)  = /WEB-INF/views + common/main + .jsp  
	-->
	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
	
	
	<!-- component-scan : 지정된 패키지에 있는 @Component와 그 구체화 요소를 스캔하여 bean으로 등록하는 태그
			(bean으로 등록됐다 == 스프링 컨테이너에 의해서 생성된 객체다. == 스프링 컨테이너가 제어할 수 있는 객체다.  (IOC, 제어의 역전)
				== bean으로 등록된 객체는 개발자가 별도로 생성하지 않아도 스프링 컨테이너에 의해서 필요한 곳에 의존적으로 주입된다. (DI, 의존성주입))
	 -->
	<context:component-scan base-package="com.kh.spring" />
	
</beans:beans>
```
<br>


## HomeController.java

```java
package com.kh.spring;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;



@Controller  // 프레젠테이션 레이어, 웹 애플리케이션에서 전달 된 요청과 응답을 처리하는 클래스임을 지정함  + bean 등록
public class HomeController {
	
	// 로그를 찍을 때 사용하는 코드 (지금은 사용하지 않으니 주석처리)
//	private static final Logger logger = LoggerFactory.getLogger(HomeController.class);
	

	
	// @RequestMapping : 요청을 매핑할 주소와 전송 타입을 작성하는 어노테이션
	// value : 매핑할 요청 주소.  * 같은 패턴도 사용할 수 있음.
	// method : 요청되는 데이터의 전송 방식 지정.
	// --> 생략된 경우 : 모든 전송 방식에 매핑
	// 해당 어노테이션은 메소드 뿐만 아니라 클래스에도 작성 가능함.
	
	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home(Locale locale, Model model) {
		/* Spring의 컨트롤러 메소드에는 반환형이 존재함(String,ModelAndView 등등) 
		 * 반환값으로 응답화면에 해당되는 view의 이름(파일명)을 작성
		 * 
		 * 반환 값을 작성하는 방법이 두 가지 존재
		 * 1) forward 방식(요청 위임)을 하고 싶을 때 -> 파일명만 작성(ex:"home")
		 * 		-> 자동으로 Dispatcher Servlet으로 돌아간 후 View Resolver에 의해 경로가 완성됨.
		 * 
		 * 2) redirect 방식(다른 요청 주소를 재요청) -> "redirect:요청주소" (ex:"redirect:/")
		 * 		-> Dispatcher Servlet으로 돌아간 후 Handler Mapping 으로 전달 됨.
		 * */	
		return "common/main";
	}
}
```

<br>



## MemberController.java

- 파라미터 전달 받는 5가지 방법.
1. HttpServletRequest를 이용한 파라미터 전달 받기.
2. @RequestParam을 이용한 파라미터 전달 방법.
3. @RequestParam 어노테이션 생략
4. @ModelAttribute를 이용한 파라미터 전달
5. @ModelAttribute 어노테이션 생략
<br>

- @RequestParam 과 @ModelAttribute를 동시에 사용할 수 있지만, 데이터가 중복될 수 있으니 주의해야 한다.
<br>

```java
package com.kh.spring.member.controller;

import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Component;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;

import com.kh.spring.member.model.vo.Member;

// 1) @Component  // 객체(컴포넌트)를 나타내는 일반적인 타입으로 bean 등록 역할을 함.

@Controller  // @Component 보다 구체적인 의미를 가지고 있음. (MVC를 확실하게 구분하기 위해서)
			 // 프레젠테이션 레이어, 가장 앞에서  웹 애플리케이션에서 잔달된 요청 응답을 처리하는 클래스 + bean 등록
@RequestMapping("/member/*")
public class MemberController {
	
	/** 로그인 화면 전환용 Controller
	 * @return viewName
	 */
	@RequestMapping("login")
	public String loginView() {
		return "member/login";
	}
	
	
	// --------------------------------------------------------------------------------
	// 로그인 동작 Controller

	// 1. HttpServletRequest를 이용한 파라미터 전달 받기.
 /*	@RequestMapping("loginAction")
	public String loginAction(HttpServletRequest request) {
		// 매개변수에 HttpServletRequest를 작성한 경우 해당 객체를 스프링 컨테이너가 자동으로 주입해 줌
		String memberId = request.getParameter("memberId");
		String memberPwd = request.getParameter("memberPwd");
		
		System.out.println("memberId : " + memberId);
		System.out.println("memberPwd : " + memberPwd);
		
		return "redirect:/";
	}
 */
	
	// 2. @RequestParam을 이용한 파라미터 전달 방법.
	// - request객체를 이용한 파라미터 전달 어노테이션
	// 매개변수 앞에 해당 어노테이션을 작성하면, 매개변수에 값이 주입됨.
	
	// [속성]
	// value : 전달받은 input 태그의 name 속성값 (하나의 속성만 있을 때 속성명을 안 쓸 경우 value로 인식함, 하나 이상 사용 시 무조건 속성명을 적어줘야 된다.)
	// required : 필수적으로 필요한 파라미터, 입력된 name 속성값 파라미터 필수 여부 지정 (기본값 true)
	//  -> required = true 인 파라미터가 존재하지 않는다면 400 Bad Request 에러가 발생함.
	// defaultValue : 파라미터 중 일치하는 name 속성 값이 없을 경우 대입할 값 지정.
	// -> required = false인 경우 주로 사용
/*	@RequestMapping(value="loginAction", method=RequestMethod.POST)
	public String loginAction( @RequestParam("memberId") String memberId, @RequestParam("memberPwd") String memberPwd,
							   @RequestParam(value="cp", required=false, defaultValue="1") int cp){
		
		System.out.println("memberId : " + memberId);
		System.out.println("memberPwd : " + memberPwd);
		System.out.println("cp : " + cp);
		
		return "redirect:/";
	}
*/
	
	// 3. @RequestParam 어노테이션 생략
	// - 매개변수명을 전달되는 파라미터 name 속성값과 일치시키면 자동으로 주입됨.
	// ** 어노테이션 코드를 생략할 경우 가독성이 떨어져서, 협업 시 좋지 않아서 잘 사용하지 않음. 
/*	@RequestMapping("loginAction")
	public String loginAction(String memberId, String memberPwd) {
		
		System.out.println("memberId : " + memberId);
		System.out.println("memberPwd : " + memberPwd);
		
		return "redirect:/";
	}
*/	
	
	// 4. @ModelAttribute를 이용한 파라미터 전달
	// 요청 페이지에서 여러 파라미터가 전달 될 때
	// 해당 파라미터가 한 객체의 필드명과 같다면 일치하는 객체를 하나 생성하여 자동으로 세팅 후 반환
	
	// (주의사항) 전달 받아 값을 세팅할 VO 내부에는 반드시 기본생성자, setter가 작성되어 있어야 한다.
	//  + name 속성값과 필드명이 같아야 함.
	
	// 파라미터가 객체안에 들어와서 매핑된것을  커맨드 객체라고 한다.
/*	@RequestMapping("loginAction")
	public String loginAction(@ModelAttribute Member inputMember) {
		System.out.println("memberId : " + inputMember.getMemberId());
		System.out.println("memberPwd : " + inputMember.getMemberPwd());
		
		return "redirect:/";
	}
*/
	
	// 5. @ModelAttribute 어노테이션 생략
	@RequestMapping("loginAction")
	public String loginAction(Member inputMember) {
		System.out.println(inputMember);
		return "redirect:/";
	}
	
}
```
<br>
<br>