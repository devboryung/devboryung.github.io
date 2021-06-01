---
title: "[Spring 실습] Controller의 리턴 타입"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---

## Controller의 리턴 타입

스프링 MVC의 구조가 기존의 상속과 인터페이스에서 어노테이션을 사용하는 방식으로 변한 이후에 가장 큰 변화 중 하나는 리턴 타입이 자유로워 졌다는 점이다.<br>

- Controller의 메서드가 사용할 수 있는 리턴 타입
1. String : jsp를 이용하는 경우에는 jsp파일의 경로와 파일이름을 나타내기 위해서 사용한다.
2. void : 호출하는 URL과 동일한 이름의 jsp를 의미한다.
3. VO,DTO 타입 : 주로 JSON 타입의 데이터를 만들어서 반환하는 용도로 사용한다.
4. ResponseEntity 타입 : response할 때 Http 헤더 정보와 내용을 가공하는 용도로 사용한다.
5. Model, ModelAndView : Model로 데이터를 반환하거나 화면까지 같이 지정하는 경우에 사용한다.
6. HttpHeaders : 응답에 내용 없이 Http 헤더 메시지만 전달하는 용도로 사용한다.

<br><br>

### void 타입

메서드의 리턴 타입을 void로 지정하는 경우 일반적인 경우에는 해당 URL의 경로를 그대로 jsp 파일의 이름으로 사용한다.

```java
@GetMapping("/ex05")
public void ex05() {
    log.info("/ex05..");
}
```
/WEB-INF/views/sample/ex05.jsp 경로 -> JSP 파일의 이름.

<br><br>

### String 타입

void 타입과 더불어서 가장 많이 사용하는 타입이다.<br>
String 타입은 상황에 따라 다른 화면을 보여줄 필요가 있을 경우에 유용하게 사용한다.<br>
EX) if~else와 같은 처리가 필요한 상황<br>
일반적으로 String 타입은 현재 프로젝트의 경우 JSP 파일의 이름을 의미한다.<br>

> 프로젝트 생성 시 기본으로 만들어지는 HomeController

```java
@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home(Locale locale, Model model) {
		logger.info("Welcome home! The client locale is {}.", locale);
		
		Date date = new Date();
		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);
		
		String formattedDate = dateFormat.format(date);
		
		model.addAttribute("serverTime", formattedDate );
		
		return "home";
```

home() 메서드는 'home'이라는 문자열을 리턴했기 때문에 경로는 '/WEB-INF/views/home.jsp'경로가 된다.<br>
<br>
String 타입에는 특별한 키워드를 붙여서 사용할 수 있다.

- redirct : 리다이렉트 방식으로 처리하는 경우
- forward : 포워드 방식으로 처리하는 경우

<br><br>

### 객체 타입

Controller의 메서드 리턴 타입을 VO(Value Object)나 DTO(Data Transfer Object)타입 등 복합적인 데이터가 들어간 객체 타입으로 지정할 수 있다.<br>
주로 JSON 데이터를 만들어 내는 용도로 사용한다.<br>

<br>

- jackson-databind 라이브러리를 pom.xml에 추가

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.4</version>
</dependency>	
```

<br>

- SampleController에 메서드 생성

```java
@GetMapping("/ex06")
public @ResponseBody SampleDTO ex06() {
    log.info("/ex06...");
    SampleDTO dto = new SampleDTO();
    dto.setAge(10);
    dto.setName("홍길동");
    
    return dto;
}
```

<br>

스프링 MVC는 자동으로 브라우저에 JSON타입으로 객체를 변환해서 전달한다.<br>





<br><br>

### ResponseEntity 타입

Web을 다루다 보면 HTTP 프로토콜의 헤더를 다루는 경우도 종종 있다.<br>
스프링 MVC의 사상은 HttpServletRequest나 HttpServletResponse를 직접 핸들링하지 않아도 작업이 가능하도록 작성되었기 때문에 이러한 처리를 위해 ResponseEntity를 통해서 원하는 헤더 정보나 데이터를 전달할 수 있다.

> SampleController

```java
@GetMapping("/ex07")
public ResponseEntity<String> ex07(){
    log.info("/ex07...");
    
    //{"name" : "홍길동"}
    String msg = "{\"name\": \"홍길동\"}";
    
    HttpHeaders header = new HttpHeaders();
    header.add("Content-Type", "application/json;charset=UTF-8");
    
    return new ResponseEntity<>(msg, header, HttpStatus.OK);
}
```
<br>

ResponseEntity는 HttpHeaders 객체를 같이 전달할 수 있고, 이를 통해서 원하는 HTTP 헤더 메시지를 가공하는 것이 가능하다.<br>
JSON타입이라는 헤더 메시지와 200 OK라는 상태 코드를 전송한다.<br>




<br><br>

### 파일 업로드 처리

파일 업로드를 하기 위해서는 전달되는 파일 데이터를 분석해야 한다.<br> 
Servlet 3.0 까지는 commons의 파일 업로드를 이용하거나 cos.jar를 이용해서 처리한다.<br>
Servlet 3.0 이후에는 기본적으로 업로드되는 파일을 처리할 수 있는 기능이 추가되어 추가적인 라이브러리가 필요하지 않다.<br>
<br>
Spring Legacy Project로 생성되는 프로젝트는 Servlet 2.5를 기준으로 생성되기 때문에 commons-fileupload 라이브러리를 추가해야 한다.<br>

> pom.xml

```xml
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.3</version>
</dependency>	
```

<br>

라이브러리 추가 후 파일이 임시로 업로드될 폴더를 C 드라이브 아래에 추가한다.<br>
<br>

> servlet-context.xml

다른 객체를 설정하는 것과 달리 파일 업로드의 경우에는 반드시 id 속성의 값을 `multipartResolver`로 정확하게 지정해야 한다.<br>

```xml
<beans:bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <beans:property name="defaultEncoding" value="utf-8"></beans:property>
    <beans:property name="maxUploadSize" value="104857560"></beans:property>
    <beans:property name="maxUploadSizePerFile" value="2097152"></beans:property>
    <beans:property name="uploadTempDir" value="file:/C:/upload/tmp"></beans:property>
    <beans:property name="maxInMemorySize" value="10485756"></beans:property>
</beans:bean>
```

<br>

maxUploadSize : 한 번의 Request로 전달될 수 있는 최대의 크기. (1024 * 1024 * 10 bytes  10MB) <br>
maxUploadSizePerFile : 하나의 파일 최대 크기. (1024 * 1024 * 2 bytes 2MB)<br>
maxInMemorySize : 메모리상에서 유지하는 최대의 크기.
uploadTempDir : 메모리상에서 유지하는 최대의 크기 이상의 데이터는 임시 파일의 형태로 보관된다. 절대 경로를 이용하려면 URI형태로 제공해야되기 때문에 'file:/'로 시작하는게 좋다.<br>
defaultEncoding : 업로드 하는 파일의 이름이 한글일 경우 깨지는 문제를 처리.

<br>

> SampleController

```java
@GetMapping("exUpload")
public void exUpload() {
    log.info("/exUpload...");
}
```
<br>

> exUpload.jsp

```html
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>

<form action="/sample/exUploadPost" method="post" enctype="multipart/form-data">
	<div>
		<input type='file' name='files'>
	</div>
	<div>
		<input type='file' name='files'>
	</div>
	<div>
		<input type='file' name='files'>
	</div>
	<div>
		<input type='file' name='files'>
	</div>
	<div>
		<input type='file' name='files'>
	</div>
	
	<div>
		<input type='submit'>
	</div>
</form>
</body>
</html>
```
<br>

exUpload.jsp는 여러 개의 파일을 한꺼번에 업로드하는 예제이다.<br>

![image](https://user-images.githubusercontent.com/73421820/120347526-53b63600-c337-11eb-8ce2-3694e3269ffa.png)

<br>

> SampleController

```java
@PostMapping("/exUploadPost")
public void exUploadPost(ArrayList<MultipartFile> files) {
    
    files.forEach(file -> {
        log.info("--------------------");
        log.info("name:" + file.getOriginalFilename());
        log.info("size:" + file.getSize());
    });
}
```

스프링 MVC는 전달되는 파라미터가 동일한 이름으로 여러 개 존재하면 배열로 처리가 가능하므로 파라미터를 MultipartFile의 배열 타입으로 작성한다.<br>
<br>

실제로 파일을 로드 후 log.<br>

```java
INFO : org.zerock.controller.SampleController - /exUpload...
INFO : org.zerock.controller.SampleController - --------------------
INFO : org.zerock.controller.SampleController - name:20210331211944_1.jpg
INFO : org.zerock.controller.SampleController - size:690593
INFO : org.zerock.controller.SampleController - --------------------
INFO : org.zerock.controller.SampleController - name:20210328201329_1.jpg
INFO : org.zerock.controller.SampleController - size:508441
INFO : org.zerock.controller.SampleController - --------------------
INFO : org.zerock.controller.SampleController - name:20210129222138_2.jpg
INFO : org.zerock.controller.SampleController - size:324209
INFO : org.zerock.controller.SampleController - --------------------
INFO : org.zerock.controller.SampleController - name:20210129222139_3.jpg
INFO : org.zerock.controller.SampleController - size:261700
INFO : org.zerock.controller.SampleController - --------------------
INFO : org.zerock.controller.SampleController - name:20210129222139_2.jpg
INFO : org.zerock.controller.SampleController - size:9650
```

<br>

최종 업로드를 하려면 byte[]를 처리해야 하는데 아직 처리하지 않은 상태이다.<br>

<br>




<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
