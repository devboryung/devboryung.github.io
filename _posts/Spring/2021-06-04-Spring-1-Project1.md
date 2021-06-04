---
title: "[Spring] 스프링 MVC프로젝트의 기본 구성"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---

## 스프링 MVC를 이용하는 프로젝트의 구성

브러우저에서 전송한 데이터를 스프링 MVC의 어떤 단계를 거쳐서 실행되는지를 이애한다면 문제가 발생했을 때 빠른 대처와 대안을 찾을 수 있다.<br>

> 3-tier(티어) 방식

일반적으로 웹 프로젝트는 3-tier 방식으로 구성한다.<br>

![image](https://user-images.githubusercontent.com/73421820/120744762-add11a00-c536-11eb-9baf-9e8103090552.png)

<br>

`Presentation Tier`(화면 계층)는 화면에 보여주는 기술을 사용하는 영역으로 Servlet/JSP나 스프링 MVC가 담당하는 영역이다.<br> 
프로젝트의 성격에 맞춰 앱으로 제작하거나, CS(Client-Server)로 구성되는 경우도 있다.<br>
<br>

`Business Tier`(비즈니스 계층)는 순수한 비즈니스 로직을 담고 있는 영역이다.<br>
이 영역이 중요한 이유는 고객이 원하는 요구 사항을 반영하는 계층이기 때문이다.<br>
이 영역의 설계는 고객의 요구 사항과 정확히 일치해야 하며, 주로 'xxxService'와 같은 이름으로 구성하고, 메서드의 이름 역시 고객들이 사용하는 용어를 그대로 사용하는 것이 좋다.<br>
<br>

`Persistence Tier`(영속 계층 혹은 데이터 계층)는 데이터를 어떤 방식으로 보관하고, 사용하는가에 대한 설계가 들어가는 계층이다.<br>
일반적인 경우에는 데이터베이스를 많이 사용하지만, 경우에 따라서 네트워크 호출이나 원격 호출 등의 기술이 접목될 수 있다.<br>
<br>

### 스프링 MVC 구조

![image](https://user-images.githubusercontent.com/73421820/120745427-15d43000-c538-11eb-9c8c-688c8d401da2.png)

<br>

스프링 MVC 영역은 Presentation Tier를 구성하게 되는데 각 영역은 사실 별도의 설정을 가지는 단위로 볼 수 있다.<br>
root-context.xml, servlet-context.xml 등의 설정 파일이 해당 영역의 설정을 담당하였다.<br>
스프링 Core영역은 흔히 POJO(Plain-Old-Java-Object)의 영역이다.<br>
스프링의 의존성 주입을 이용해서 객체 간의 연관구조를 완성해서 사용한다.<br>
MyBatis 영역은 현실적으로는 mybatis-spring을 이용해서 구성하는 영역으로, SQL에 대한 처리를 담당하는 구조이다.<br>
<br>

<br><br>




<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>