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

> 클래스 생성

```java
package org.zerock.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/sample/*")
public class SampleController {

}
```

<br>

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

