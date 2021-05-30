---
title: "[Spring 실습] Controller의 파라미터 수집"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---

## Controller의 파라미터 수집

Controller를 작성할 때 가장 편리한 기능은 파라미터가 자동으로 수직되는 기능이다.<br>
이 기능을 이용하면 매번 request.getParameter()를 이용하는 불편함을 없앨 수 있다.<br>

> SampleDTO 클래스

```java
package org.zerock.domain;

import lombok.Data;

@Data
public class SampleDTO {
	
	private String name;
	private int age;

}
```

해당 클래스는 Lombok의 @Data 어노테이션을 이용했기 때문에 getter/setter, equals(), toSting()등의 메서드를 자동 생성할 수 있다.<br>
<br>
SampleController의 메서드가 SampleDTO를 파라미터로 사용하게 되면 자동으로 setter 메서드가 동작하면서 파라미터를 수집한다.<br>

> SampleController 클래스

```java
@Controller
@RequestMapping("/sample/*")
@Log4j
public class SampleController {
	
	
	@GetMapping("/ex01")
	public String ex01(SampleDTO dto) {
		log.info(""+dto);
		return "ex01";
				
	}

}
```

SampleDontroller의 경로가 '/sample/*' 이므로 ex01()메서드를 호출하는 경로는 'sample/ex01'이 된다.<br>
@GetMapping이 사용되어 필요한 파라미터를 URL 뒤에 '?name=AAA&age=10'과 같은 형태로 추가해서 호출할 수 있다.<br><br>

<br>

### 파라미터의 수집과 변환

Controller가 파라미터를 수집하는 방식은 파라미터 타입에 따라 `자동`으로 변환하는 방식을 이용한다.<br>
SampleDTO에 int 타입으로 선언된 age는 자동으로 숫자로 변환된다.<br>
<br>
만일 기본 자료형이나 문자열 등을 이용한다면 파라미터의 타입만을 맞게 선언해주는 방식을 사용할 수 있다.<br>

>SampleController

```java
@Controller
@RequestMapping("/sample/*")
@Log4j
public class SampleController {
	
	@GetMapping("/ex02")
	public String ex02(@RequestParam("name") String name, @RequestParam("age") int age) {
		
		log.info("name: " + name);
		log.info("age: " + age);
		
		return "ex02";
	}

}
```

@RequestParam은 파라미터로 사용된 변수의 이름과 전달되는 파라미터의 이름이 다른 경우에 유용하게 사용된다.<br>
(지금은 변수명과 파라미터의 이름이 동일하기 때문에 사용하지 않아도 됨)<br>

<br><br>

### 리스트, 배열 처리

동일한 이름의 파라미터가 여러 개 전달되는 경우에는 ArrayList<> 등을 이용한다.<br>

> SampleController 클래스

```java
@Controller
@RequestMapping("/sample/*")
@Log4j
public class SampleController {
	

	@GetMapping("/ex02List")
	public String ex02List(@RequestParam("ids") ArrayList<String> ids) {
		log.info("ids :" + ids);
		return "ex02List";
	}

}
```

스프링은 파라미터의 타입을 보고 객체를 생성하므로 파라미터의 타입은 List<>와 인터페이스 타입이 아닌 실제적인 클래스 타입으로 지정한다.<br>
ids라는 이름의 파라미터가 여러 개 전달되어도 ArrayList\<String\> 이 생성되어 자동으로 수집된다.<br>

> 배열의 경우

```java
@Controller
@RequestMapping("/sample/*")
@Log4j
public class SampleController {
	
	
	@GetMapping("ex02Array")
	public String ex02Array(@RequestParam("ids") String[] ids) {
		log.info("array ids: " + Arrays.deepToString(ids));
		return "ex02Array";
	}

}
```

<br><br><br>

### 객체 리스트

만일 전달하는 데이터가 SampleDTO와 같이 객체 타입이고 여러 개를 처리해야 한다면 한 번에 처리할 수 있다.<br>
SampleDTO를 여러 개 받아서 처리하고 싶다면 SampleDTO의 리스트를 포함하는 클래스를 설계한다.<br>

> SampleDTOList 클래스

```java
package org.zerock.domain;

import java.util.ArrayList;
import java.util.List;

import lombok.Data;

@Data
public class SampleDTOList {
	
	private List<SampleDTO> list;
	
	public SampleDTOList() {
		list = new ArrayList<>();
	}
}
```
<br>

> SampleController 클래스

```java
@Controller
@RequestMapping("/sample/*")
@Log4j
public class SampleController {

	
	@GetMapping("/ex02Bean")
	public String ex02Bean(SampleDTOList list) {
		log.info("list dtos: " + list);
		return "ex02Bean";
	}
}
```

파라미터는 `[인덱스]` 같은 형식으로 전달해서 처리할 수 있다.<br>
하지만 Tomcat 버전에 따라서 문자열에서 '[]'문자를 특수문자로 허용하지 않을 수 있기 때문에, ']'는 '%5B'로 ']'는 '%5D'로 변경한다.<br>

<br><br>

<br><br>




관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>

