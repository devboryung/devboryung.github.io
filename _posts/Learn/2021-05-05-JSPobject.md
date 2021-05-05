---
title: "[JSP]내장 객체"
excerpt: "JSP"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---

## JSP 내장 객체

JSP에서 기본적으로 제공되는 객체이다.<br>
선언하지 않고 사용할 수 있는 객체이다.<br>
  exception 객체는 isErrorPage="true" 인 경우만 추가된다.<br>
하나의 Java 클래스 객체이기 때문에 생성을 위해 사용되어지는 interface, class가 있으며, 그 interface, class를 포함하고 있는 패키지가 있다.<br>


<br>


## 객체의 종류

|내장 객체|소속 패키지|클래스 이름|사용 용도|
|:---:|:---:|:---:|:-----:|
|**request**|javax.servlet.http|HttpServletRequest|클라이언트 요청에 담겨진 사용자 입력 정보를 읽고자 할 때 사용|
|**response**|javax.servlet.http|HttpServletResponse|사용자 요청에 대한 응답을 처리할 때 사용|
|**session**|javax.servlet.http|HttpSession|클라이언트의 세션 정보를 처리할 때 사용|
|**application**|javax.servlet|ServletContext|웹 서버의 웹 어플리케이션 정보를 처리할 때 사용|
|**config**|javax.servlet|ServletConfig|현재 JSP 페이지의 초기화 환경을 처리할 때 사용|
|**pageContext**|javax.servlet.jsp|PageContext|현재 JSP 페이지에 대한 context 정보를 참조할 때 사용|
|**out**|javax.servlet.jsp|JspWriter|사용자에게 전달하기 위한 출력 스트림 처리 객체|
|**exception**|java.lang|Throwable|예외처리할 때 사용|
|**page**|java.lang|Object|현재 JSP 페이지에 대한 클래스 정보를 볼 때 사용|

<br>

- session과 application

session은 하나의 접속에 국한되어 지속적으로 어떤 정보를 유지하고자 할 때 사용한다.<br>  
application은 현재 웹 서버 전반에 걸쳐서 정보를 전역적으로 유지할 때 사용한다.<br><br>


- out

웹 서버에서 웹 클라이언트(사용자)에게 응답으로 보내지는 페이지에 출력할 때 사용되는 출력 스트림 객체이다.<br>


<br>



### Request

- 웹 클라이언트로부터 전달 받은 사용자 요청과 관련된 기능을 제공
    - 클라이언트에서 서버로 전달된 정보를 처리할 때 사용
    - HTML 폼 태그를 통해 입력된 값을 JSP에서 가져올 때 사용 <br>
    ex) 로그인 시, id나 비밀번호를 입력해서 로그인 버튼을 클릭하면 현재 사용자 계정 정보를 서버에게 보내고, 정확한 정보인지 웹 서버가 확인하는 작업을 한다.<br> 이 때 입력된 정보를 얻어오는 작업을 request 객체가 사용된다.<br>
    
### Response

- 웹 서버에서 웹 클라이언트로 보내지는 사용자 요청에 대한 응답과 관련된 기능을 제공
    - 요청에 대한 응답을 사용자에게 전달
    - 응답을 다른 페이지로 전달

<br><br>


[참고 영상 - 유튜브](https://www.youtube.com/watch?v=0gEo84hMx6U&t=966s)

<br><br>
