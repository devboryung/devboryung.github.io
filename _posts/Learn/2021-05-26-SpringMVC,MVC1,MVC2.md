---
title: "MVC1과 MVC2의 차이점"
excerpt: "MVC"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---

## MVC 패턴이란?

MVC패턴은 디자인 패턴 중 하나로 하나의 애플리케이션,프로젝트를 구성할 때 그 구성 요소를 `Model`, `View`, `Controller` 세 가지의 역할로 구분한 패턴이다.<br>
<br>

**Model** :  애플리케이션의 정보,데이터를 나타낸다.<br>
**View** : 데이터 및 객체의 입력, 출력을 담당하는 사용자 인터페이스 요소를 나타낸다.<br>
**Controller** : 데이터와 사용자인터페이스 요소들을 연결하는 다리 역할을 한다.<br>

<br><br>

### MVC1 패턴

![image](https://user-images.githubusercontent.com/73421820/119675632-2e798180-be78-11eb-8c34-de6864925740.png)
<br>

JSP가 Controller와 View 기능을 모두 담당하여 웹브라우저 요청을 모두 처리한다.<br>
JSP 하나로 유저의 요청을 받고 응답을 처리하므로 구현 난이도는 쉽다.<br>
하나의 JSP페이지 내에서 Controller는 자바, view는 html, css 이벤트는 자바스크립트를 사용한다.<br>
<br>

**장점** : 페이지 흐름이 단순하고 구조가 간단하여 중소형 프로젝트에 적합하다.<br>
**단점** : JSP 하나에서 MVC가 모두 이루어져서 규모가 커질수록 복잡해진다.<br>

<br><br>

### MVC2 패턴

![image](https://user-images.githubusercontent.com/73421820/119675695-39ccad00-be78-11eb-8fae-86260c6c756c.png)
<br>

웹 브라우저의 요청을 하나의 컨트롤러(Servlet)가 먼저 받아서 처리한다.<br>
컨트롤러는 요청에 대한 로직처리를 Model로 보내고, Model은 결과를 View로 보내 사용자에게 응답한다.<br>
MVC1과는 다르게 Controller와 View가 분리되어 있다.<br><br>

**장점** : Controller와 View의 분리로 명료한 구조를 가지기 때문에 유지보수 확장에 용이하다.<br>
**단점** : 구조 설계를 위한 시간이 많이 소요된다.<br>
<br><br>



### 스프링 프레임워크의 MVC2 패턴

![image](https://user-images.githubusercontent.com/73421820/119674476-308f1080-be77-11eb-9865-817e5a64a4ee.png)

<br>

1. 사용자의 Request는 DispatcherServlet을 통해서 처리된다.
2. HandlerMapping은 Request의 처리를 담당하는 @RequestMapping 어노테이션이 적용된 컨트롤러를 찾기 위해 존재한다.<br>적절한 컨트롤러가 찾아졌다면 HandlerAdapter를 이용해서 해당 컨트롤러를 동작시킨다.<br>
3. Controller는 개발자가 작성하는 클래스로 실제 Request를 처리하는 로직을 작성한다.<br>
View에 전달해야 하는 데이터는 주로 Model이라는 객체에 담아서 전달한다.<br>Controller는 다양한 타입의 결과를 반환하는데 이에 대한 처리는 viewResolver를 이용한다.<br>
4. ViewResolver는 Controller가 반환한 결과를 어떤 View를 통해서 처리하는 것이 좋을지 해석하는 역할을 한다.<br>
5. View는 실제로 응답 보내야 하는 데이터를 Jsp 등을 이용해서 생성하는 역할을 하게 된다.<br>
만들어진 응답은 DispatcherServlet을 통해서 전송된다.<br><br>

<br>

[참고 자료](https://chanhuiseok.github.io/posts/spring-3/)