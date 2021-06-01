---
title: "[Spring 실습] @InitBinder"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


파라미터의 수집을 다른 용어로 'binding(바인딩)'이라고 한다.<br>
변환이 가능한 데이터는 자동으로 변환되지만 경우에 따라서 파라미터를 변환해서 처리해야 하는 경우가 있다.<br>
EX) 화면에서는 '2018-01-01'과 같이 문자열로 전달된 데이터를 java.util.Date 타입으로 변환하는 작업.<br>
<br>

스프링 Controller에서는 파라미터를 바인딩할 때 자동으로 호출되는 `@InitBinder`를 이용해서 변환을 처리한다.<br>

> TodoDTO 클래스

```java
package org.zerock.domain;

import java.util.Date;

import lombok.Data;

@Data
public class TodoDTO {
	private String title;
	private Date dueDate;
}
```

TodoDTO에는 dueDate변수의 타입이 java.util.Date타입이다.<br>
만약 사용자가 '2018-01-01'과 같이 들어오는 데이터를 변환하고자 할 때 문제가 발생하게 된다.<br>
@InitBinder를 이용해서 문제를 해결할 수 있다.<br>
<br>

>SampleController 클래스


```java
@Controller
@RequestMapping("/sample/*")
@Log4j
public class SampleController {
	
	public void initBinder(WebDataBinder binder) {
		SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
		binder.registerCustomEditor(java.util.Date.class, new CustomDateEditor(dateFormat, false));
	}
	
	@GetMapping("/ex03")
	public String ex03(TodoDTO todo) {
		log.info("todo: " + todo);
		return "ex03";
	}
}
```

SimpleDateFormat : 날짜를 원하는 형식으로 표현할 수 있다.<br>

binder.registerCustomEditor : binder에 커스텀 에디터를 등록한다.<br>




<br><br>




관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>

