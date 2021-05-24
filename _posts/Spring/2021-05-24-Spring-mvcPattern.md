---
title: "[Spring 실습] MVC의 기본 구조"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## Spring MVC

Spring Framework라는 메인 프로젝트 외에도 여러 종류의 서브 프로젝트가 존재하는데, 스프링 MVC 역시 이러한 프로젝트 중 일부이다.<br>

스프링은 하나의 기능을 위해서만 만들어진 프레임워크가 아니라 '코어'라고 할 수 있는 프레임워크에 여러 서브 프로젝트를 결합해서 다양한 상황에 대처할 수 있도록 개발되었다.<br>

<br><br>


### 스프링 MVC 프로젝트의 내부 구조

스프링 MVC 프로젝트를 구성해서 사용하면 내부적으로는 root-context.xml로 사용하는 일반 `JAVA 영역`과 servlet-context.xml로 설정하는 `Web관련 영역`을 같이 연동해서 구동하게 된다.<br>
스프링은 원래 목적 자체가 웹 애플리케이션을 목적으로 나온 프레임워크가 아니기 때문에 달라지는 영역에 대해서는 완전히 분리하고 연동하는 방식으로 구현된다.<br><br>

실습을 위해 'Spring Legacy Project'를 이용해서 `ex01` 프로젝트를 생성한다.<br>

> pom.xml 수정해서 스프링 버전 변경

```xml
<properties>
    <java-version>1.8</java-version>
    <org.springframework-version>5.0.7.RELEASE</org.springframework-version>
    <org.aspectj-version>1.6.10</org.aspectj-version>
    <org.slf4j-version>1.6.6</org.slf4j-version>
</properties>
```

<br>

> pom.xml에 lombok 추가

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>${org.springframework-version}</version>
</dependency>
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.0</version>
    <scope>provided</scope>
</dependency>
```
<br>

> pom.xml에 서블릿 버전 수정

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
```
<br>


> pom.xml에 Maven 버전 수정

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>2.5.1</version>
    <configuration>
        <source>1.8</source>
        <target>1.8</target>
        <compilerArgument>-Xlint:all</compilerArgument>
        <showWarnings>true</showWarnings>
        <showDeprecation>true</showDeprecation>
    </configuration>
</plugin>
```

<br>

<br><br>

### 프로젝트의 로딩 구조

프로젝트가 정상적으로 실행되었다면 서버의 구동 시 약간의 로그가 기록되는 데, 로그를 이용해서 어떤 과정을 통해 프로젝트가 실행되는지 확인할 수 있다.<br>

프로젝트 구동 시 관여하는 XMl은 web.xml, root-context.xml, servlet-context.xml파일이다.<br>
web.xml은 Tomcat 구동과 관련된 설정이고, 나머지 두 파일은 스프링과 관련된 설정이다.<br><br>

프로젝트의 구동은 web.xml에서 시작된다.<br>
web.xml의 상단에는 가장 먼저 구동되는 Context Listener가 등록되어 있다.<br>

> web.xml

```xml
<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/spring/root-context.xml</param-value>
</context-param>

<!-- Creates the Spring Container shared by all Servlets and Filters -->
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```    
<br>

context-param 태그에는 root-context.xml의 경로가 설정되어 있고,<br>
listener태그에는 스프링 MVC의 ContextLoaderListener가 등록되어 있다.<br>
ContextLoaderListener는 웹 애플리케이션 구동 시 같이 동작하며 해당 프로젝트를 실행하면 가장 먼저 로그를 출력하면서 기록한다.<br>
<br>
root-context-xml이 처리되면 파일에 있는 빈(Bean) 설정들이 동작하게 된다.<br>
root-context.xml에 정의된 객체(Bean)들은 스프링의 영역(context) 안에 생성되고, 객체들 간의 의존성이 처리된다.<br>
root-context.xml이 처리된 후에는 스프링 MVC에서 사용하는 DispatcherServlet이라는 서블릿과 관련된 설정이 동작한다.<br><br>

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
    <load-on-startup>1</load-on-startup>
</servlet>
    
<servlet-mapping>
    <servlet-name>appServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

org.springframework.web.servlet.DispatcherServlet 클래스는 스프링 MVC의 구조에서 가장 `핵심적인` 역할을 하는 클래스이다.<br>
내부적으로 웹 관련 처리의 준비작업을 진행하는데 이때 사용하는 파일이 servlet-context.xml이다.<br>
DispatcherServlet에서 XmlWebApplicationContext를 이용해서 servlet-context.xml을 로딩하고 해석하기 시작하면 등록된 객체들은 기존에 만들어진 객체들과 같이 연동되게 된다.<br>
<br><br><br>

### 스프링 MVC의 기본 사상

스프링 MVC를 이용하게 되면 개발자들은 직접적으로 HttpServletRequest/HttpServletResponse등과 같이 Servlet/JSP의 API를 사용할 필요성이 현저하게 줄어든다.<br>
스프링은 중간에 연결 역할을 하기 때문에 이런 코드를 작성하지 않고 원하는 기능을 구현할 수 있게 된다.<br><br>

개발자의 코드는 스프링 MVC에서 동작하기 때문에 과거에는 스프링 MVC의 특정한 클래스를 상속하거나 인터페이스를 구현하는 형태로 개발할 수 있었찌만, 스프링 2.5버전부터 어노테이션이나 XML등의 설정만으로 개발이 가능하게 되었다.<br>
<br><br>

### 모델2와 스프링 MVC

스프링 MVC 역시 내부적으로는 Servlet API를 활용한다.<br>
모델 2방식은 스프링 MVC가 처리되는 구조로, `로직과 화면을 분리`하는 스타일의 개발 방식이다.<br><br>

모델 2방식에서 사용자의 Request는 특별한 상황이 아닌 이상 먼저 Controller를 호출한다.<br>
이렇게 설계하는 이유는 나중에 View를 교체하더라도 사용자가 호출하는 URL 자체에 변화가 없 게 만들어 주기 때문이다.<br>

컨트롤러는 데이터를 처리하는 존재를 이용해서 데이터(Model)을 처리하고 Response 할 때 필요한 데이터(Model)를 View 쪽으로 전달한다.<br>

Servlet을 이용하는 경우 개발자들은 Servlet API의 RequestDispatcher등을 이용해서 이를 직접 처리했지만 스프링 MVC는 내부에서 이러한 처리를 하고, 개발자들은 스프링 MVC의 API를 이용해서 코드를 작성하게 된다.<br>
<br><br>

### 스프링 MVC의 기본 구조

- 사용자의 Request는 Front-Contorller인 DispatcherServlet을 통해서 처리된다.<br>생성된 프로젝트의 web.xml에서 모든 Request를 DispatcherServlet이 받도록 처리한다.<br>
>web.xml

```xml
<servlet>
    <servlet-name>appServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>appServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

<br>

- HandlerMapping은 Request의 처리를 담당하는 컨트롤러를 찾기 위해 존재한다.<br>
HandlerMapping 인터페이스를 구현한 여러 객체들 중 RequestMappingHandlerMapping같은 경우는 개발자가 @RequestMapping 어노테이션이 적용된 것을 기준으로 판단한다.<br><br>
적절한 컨트롤러가 찾아졌다면 HandlerAdapter를 이용해서 해당 컨트롤러를 동작시킨다.<br>
- Controller는 개발자가 작성하는 클래스로 실제 Request를 처리하는 로직을 작성한다.<br>
View에 전달해야 하는 데이터는 주로 Model이라는 객체에 담아서 전달한다.<br> Controller는 다양한 타입의 결과를 반환하는데 이에 대한 처리는 viewResolver를 이용한다.<br><br>
- ViewResolver는 Controller가 반환한 결과를 어떤 View를 통해서 처리하는 것이 좋을지 해석하는 역할을 한다.<br>
가장 흔하게 사용하는 설정은 servlet-context.xml에 정의된 InternalResourceViewResolver이다.<br>

>servlet-context.xml

```xml
<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <beans:property name="prefix" value="/WEB-INF/views/" />
    <beans:property name="suffix" value=".jsp" />
</beans:bean>
```

<br>

- View는 실제로 응답 보내야 하는 데이터를 Jsp 등을 이용해서 생성하는 역할을 하게 된다.<br>
만들어진 응답은 DispatcherServlet을 통해서 전송된다.<br><br>

<br><br>

모든 Request는 DispatcherServlet을 통하도록 설계되는데, 이런 방식을 Front-Controller 패턴이라고 한다.<br>
Front-Controller 패턴을 이용하면 전체 흐름을 강제로 제한할 수 있다.<br>
예를 들어 HttpServlet을 상속해서 만든 클래스를 이용하는 경우 특정 개발자는 이를 활용할 수 있지만 다른 개발자는 자신이 원래 하던 방식대로 HttpServlet을 그대로 상속해서 개발할 수 있다.<br>
Front-Controller 패턴을 이용하는 경우 모든 Request의 처리에 대한 분배가 정해진 방식대로만 동작하기 때문에 더 엄격한 구조를 만들어 낼 수 있다.<br>

<br><br>




관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>

