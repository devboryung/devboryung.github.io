---
title: "2020년 12월 25일"
excerpt: "WSP-2"
search: true
categories: 
  - Academy
tags: 
  - Servlet
  - Java
  - HTML
  - CSS
toc: true
---
## 이클립스 Tab 키 간격 조정
<br>
![탭키 간격조정](https://user-images.githubusercontent.com/73421820/103184354-26cea480-48fb-11eb-88da-8e31862226dd.png)<br>
<br>
Window -> Preferences -> General -> Editors -> <br> 
Text Editors -> Displayed tab wiedth (스페이스 몇 번 입력한 것과 똑같을지 설정)
<br><br>

## 아이디 저장 체크
<br>

### LoginServlet.java
```java
// 파라미터를 얻어와 변수에 저장
String save = request.getParameter("save");

// cookie -> 사용자 컴퓨터에 저장
// session -> 서버에 저장

// 서버가 접속한 브라우저 마다 session을 구분하는 방법
// -> 해당 브라우저의 쿠키파일에 session을 구분할 수 있는 
//   session id를 저장시켜 두었다가 접속 될 때 마다 쿠키에서 자동으로 session id를 얻어감.


// 6_4. 아이디를 Cookie에 저장하기

// 2) 쿠키 객체 생성
// checkbox를 체크 안 했을 때 아이디가 저장되지 않게 하기 위해 if문 밖에서 선언해주어야한다.
Cookie cookie = new Cookie("saveId",memberId);

// 1) 아이디 저장 checkbox가 체크 되었는지 확인
// 서블릿 맨 위에 name 값이 save인 파라미터를 받아왔음.
if(save !=null ) {
  // 3) 로그인에 성공했을 때 아이디 저장이 체크가 되었을 때, 아이디를 저장하지만
  //    언제까지 유효한지 쿠키의 생명 주기를 설정 해주어야 됨.
  //    1주일 동안 쿠키가 유효하도록 설정 (초 단위)
  cookie.setMaxAge(60*60*24*7); // 7일
}else {
  // 4) 로그인에 성공했는데 아이디 저장 checkbox가 체크가 안 되었다면
  //    다음 로그인 때 아이디 저장이 되지 않게 기존에 있던 쿠키 파일 삭제
  cookie.setMaxAge(0); // 생성되자마자 삭제
}

// 5) 쿠키 유효 디렉토리 지정
cookie.setPath(request.getContextPath());
// ContextPath : 배포되는 프로젝트의 시작 주소 == /wsp

// 6) 생성된 쿠키를 클라이언트로 전달(응답)
response.addCookie(cookie);

```
<br><br>

### header.jsp
```html
<!-- 쿠키에 저장되어 있는 값을 얻어오기,저장이 안되어있으면 null.value 
EL은 NullpointException이 없다.빈문자열 반환-->	
<input type="text" class="form-control" id="memberId" name="memberId" placeholder="아이디" value="${cookie.saveId.value }">
<br>
<input type="password" class="form-control" id="memberPwd" name="memberPwd" placeholder="비밀번호">
<br>

<div class="checkbox mb-3">
  <!-- 쿠키에 저장된 아이디가 있다면 input 태그에 checked라는 속성을 추가해라 
    한번 아이디 저장을 체크한다면 다음에도 생명기간이 다 될 때 까지 항상 체크되어있어야 한다.-->
  <label> 
  <input type="checkbox" name="save" id="save" 
    <c:if test="${!empty cookie.saveId.value }">
      checked										
    </c:if>
  > 아이디 저장
  </label>
</div>
```
<br><br>

