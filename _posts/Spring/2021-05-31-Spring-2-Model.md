---
title: "[Spring 실습] Model"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---

Controller의 메서드를 작성할 때 특별하게 `Model`이라는 타입을 파라미터로 지정할 수 있다.<br>

Model 객체는 JSP에 컨트롤러에서 생성된 데이터를 담아서 전달하는 역할을 하는 존재이다.<br>

JSP와 같은 뷰(View)로 전달해야 하는 데이터를 담아 보낼 수 있다.<br>

메서드의 파라미터에 Model 타입이 지정된 경우 스프링은 특별하게 Model 타입의 객체를 만들어서 메서드에 주입한다.<br>


Model은 모델2 방식에서 사용하는 request.setAttribute()와 유사한 역할을 한다.<br>

>Servlet에서 모델2 방식을 이용한 데이터 전달 방식

```java
 request.setAttribute("serverTime", new java.util.Date());
 RequestDispatcher dispatcher = request.getRequestDispatcher("/WEB-INF/jsp/home.jsp");
 dispatcher.forward(request,response);
```

<br>

>스프링 MVC에서 Model을 이용한 데이터 전달 방식

```java
public String home(Model model){
    model.addAttribute("serverTime", new java.util.Date());
    return "home";
}
```

<br>

메서드의 파라미터를 Model타입으로 선언하게 되면 자동으로 스프링 MVC에서 Model타입의 객체를 만들어 주기 때문에 개발자의 입장에서는 필요한 데이터를 담아 주는 작업만으로 모든 작업이 완료된다.<br><br>
Model을 사용해야 하는 경우는 주로 Controller에 전달된 데이터를 이용해서 추가적인 데이터를 가져와야 하는 상황이다.<br>

- 리스트 페이지 번호를 파라미터로 전달받고, 실제 데이터를 View로 전달해야 하는 경우
- 파라미터들에 대한 처리 후 결과를 전달해야 하는 경우

<br><br>

### @ModelAttribute 어노테이션

웹페이지의 구조는 Request에 전달된 데이터를 가지고 필요하다면 추가적인 데이터를 생성해서 화면으로 전달하는 방식으로 동작한다.<br>
Model의 경우는 파라미터로 전달된 데이터는 존재하지 않지만 화면에서 필요한 데이터를 전달하기 위해서 사용한다.<br>
<br>
스프링 MVC의 Controller는 기본적으로 Java Beans 규칙에 맞는 객체는 다시 화면으로 객체를 전달한다.<br>
좁은 의미에서 Java Beans의 규칙은 단순히 생성자가 없거나 빈 생성자를 가져야 하며, getter/setter를 가진 클래스의 객체들을 의미한다.<br>
<br>

SampleDTO클래스는 Java Bean의 규칙에 맞기 때문에 자동으로 다시 화면까지 전달된다.<br><br>

`@ModelAttribute`는 강제로 전달받은 파라미터를 Model에 담아서 전달하도록 할 때 필요한 어노테이션이다.<br>
@ModelAttribute가 걸린 파라미터는 타입에 관계없이 무조건 Model에 담아서 전달되므로 파라미터로 전달된 데이터를 다시 화면에서 사용해야 할 경우에 유용하게 사용된다.<br>

<br><br>

### RedirectAttributes

Model타입과 더불어서 스프링 MVC가 자동으로 전달해 주는 타입 중에는 Redirect Attributes 타입이 존재한다.<br>
RedirectAttributes는 조금 특별하게도 `일회성`으로 데이터를 전달하는 용도로 사용된다.<br>
RedirectAttributes는 기존에 Servlet에서는 response.sendRedirect()를 사용할 때와 동일한 용도로 사용된다.
<br>

> Servlet에서 redirect 방식

```java
response.sendRedirect("/home?name=aaa&age=10");
```

<br>

> 스프링 MVC를 이용하는 redirect 방식

```java
rttr.addFlashAttribute("name","AAA");
rttr.addFlashAttribute("age",10);
return "redirect:/";
```

<br>

RedirectATtributes는 Model과 같이 파라미터로 선언해서 사용하고, `addFlashAttribute(이름,값)` 메서드를 이용해서 화면에 한 번만 사용하고 다음에는 사용되지 않는 데이터를 전달하기 위해서 사용한다.<br>

<br>




<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>

