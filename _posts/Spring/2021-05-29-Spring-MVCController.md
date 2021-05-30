---
title: "[Spring 실습] MVC의 Controller"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---

## 스프링 MVC Controller의 특징

- HttpServletRequest, HttpServletResponse를 거의 사용할 필요 없이 필요한 기능을 구현할 수 있다.
- 다양한 타입의 파라미터 처리, 다양한 타입의 리턴 타입이 사용 가능하다.
- GET 방식, POST 방식 등 전송 방식에 대한 처리를 어노테이션으로 처리 가능하다.
- 상속/인터페이스 방식 대신에 어노테이션만으로도 필요한 설정이 가능하다.

<br><br>


### @Controller, @RequestMapping

> SampleController 클래스

```java
package org.zerock.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/sample/*")
public class SampleController {

}
```

클래스 선언부에 `@Controller`라는 스프링 MVC에서 사용하는 어노테이션을 적용하면, servlet-context.xml에 의해 자동으로 스프링의 객체로 등록된다.<br>
<br>

> servlet-context.xml

```xml
<context:component-scan base-package="org.zerock.controller" />
```


위 태그를 이용해서 지정된 패키지를 `스캔`하도록 설정되어 있으며, 해당 패키지에 선언된 클래스들을 조사하면서 스프링에서 객체 설정에 사용되는 어노테이션들을 가진 클래스들을 파악하고 필요하다면 이를 객체로 생성해서 관리한다.<br>

<br>

클래스 선언부에 사용된 `@RequestMapping` 어노테이션은 현재 클래스의 모든 메서드들의 기본적인 URL 경로가 된다. <br>
RequestMapping 어노테이션은 메서드 선언에서도 사용할 수 있다.<br>

```java
package org.zerock.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import lombok.extern.log4j.Log4j;


@Controller
@RequestMapping("/sample/*")
@Log4j
public class SampleController {
	
	@RequestMapping("")
	public void basic() {
		log.info("basic....");
	}
}
```
<br>

해당 클래스는 Lombok의 @Log4j를 사용하며, Spring Legacy Project로 생성한 프로젝트는 기본적으로 Log4j가 추가되어 있어 별도의 설정이 필요하지 않다.<br><br>

src/resources 폴더 내에 log4j.xml의 모든 info를 `debug`로 변경하면 '/'와 '/sample'는 호출이 가능한 경로라는 것을 확인할 수 있다.<br>
```xml
INFO : org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping - Mapped "{[/],methods=[GET]}" onto public java.lang.String org.zerock.controller.HomeController.home(java.util.Locale,org.springframework.ui.Model)
INFO : org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping - Mapped "{[/sample/*]}" onto public void org.zerock.controller.SampleController.basic()
```

<br>
<br>

### RequestMapping의 변화

@Controller 어노테이션은 추가적인 속성을 지정할 수 없지만, @RequestMapping 어노테이션은 속성을 추가할 수 있다.<br>
가장 많이 사용하는 속성은 `Method` 속성이다.<br>
Method 속성은 흔히 GET방식, POST방식을 구분해서 사용할 때 이용한다.<br>
<br>

스프링 4.3버전부터는 @RequestMapping을 줄여서 @GetMapping, @PostMapping으로 사용할 수 있다.<br>

```java
@RequestMapping(value="/basic", method = {RequestMethod.GET, RequestMethod.POST})
public void basicGet() {
    log.info("basic get....");
}


@GetMapping("/basicOnlyGet")
public void basicGet2() {
    log.info("basic Get only get");
}
```

@RequestMapping은 GET, POST 방식 모두를 지원해야 하는 경우에 배열로 처리해서 지정할 수 있다.<br>
@GetMapping의 경우 오직 GET방식에만 사용할 수 있으므로 간편하긴 해도 기능에 대한 제한이 많다.<br>
<br>

<br><br>




