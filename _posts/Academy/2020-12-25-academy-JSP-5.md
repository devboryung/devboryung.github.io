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

## 회원가입
<br>
### header.jsp
```html
<a class="btn btn-lg btn-secondary btn-block" href="${contextPath}/member/signUpForm.do">회원가입</a>
```
<br>

### SignUpFormServlet.java
```java
package com.kh.wsp.member.controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/member/signUpForm.do")
public class SignUpFormServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		// 회원가입 화면을 보여주는 서블릿(응답화면)
		// jsp의 경로가 길어서 변수에 저장해서 사용
		String path = "/WEB-INF/views/member/signUpForm.jsp";
		RequestDispatcher view = request.getRequestDispatcher(path);
		
		view.forward(request, response);
		
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}
```
<br>

### sighUpForm.jsp
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>회원가입</title>
<style>
	/* number 태그 화살표 제거 */
	input[type="number"]::-webkit-outer-spin-button, input[type="number"]::-webkit-inner-spin-button
		{
		-webkit-appearance: none;
		margin: 0;
	}
</style>
</head>
<body>
	<div class="container">
		<%-- 정적 Include : <%@ include file="../common/header.jsp"%> --%>
		<jsp:include page="../common/header.jsp"></jsp:include>

		<div class="py-5 text-center">
			<h2>회원 가입</h2>
		</div>

		<div class="row">
			<div class="col-md-6 offset-md-3">

				<form method="POST" action="signUp.do" class="needs-validation" name="signUpForm" onsubmit="return validate();">
					<%-- action="${contextPath}/member/signUp.do" --%>

					<!-- 아이디 -->
					<div class="row mb-5 form-row">
						<div class="col-md-3">
							<label for="id">* 아이디</label>
						</div>
						<div class="col-md-6">
							<input type="text" class="form-control" name="id" id="id" maxlength="12" placeholder="아이디를 입력하세요" autocomplete="off" required>
							<!-- required : 필수 입력 항목으로 지정 -->
							<!-- autocomplete="off" : input 태그 자동완성 기능을 끔 -->

							<!-- 팝업창 중복체크 여부 판단을 위한 hidden 타입 요소 -->
							<input type="hidden" name="idDup" id="idDup" value="false">
						</div>

						<!-- ajax 중복검사 시 필요없음 -->
						<div class="col-md-3">
                <button type="button" class="btn btn-primary" id="idDupCheck">중복검사</button>
            </div>

						<div class="col-md-6 offset-md-3">
							<span id="checkId">&nbsp;</span>
						</div>
					</div>


					<!-- 비밀번호 -->
					<div class="row mb-3 form-row">
						<div class="col-md-3">
							<label for="pwd1">* 비밀번호</label>
						</div>
						<div class="col-md-6">
							<input type="password" class="form-control" id="pwd1" name="pwd1" maxlength="12" placeholder="비밀번호를 입력하세요" required>
						</div>

						<div class="col-md-6 offset-md-3">
							<span id="checkPwd1">&nbsp;</span>
						</div>
					</div>

					<!-- 비밀번호 확인 -->
					<div class="row mb-3 form-row">
						<div class="col-md-3">
							<label for="pwd2">* 비밀번호 확인</label>
						</div>
						<div class="col-md-6">
							<input type="password" class="form-control" id="pwd2" maxlength="12" placeholder="비밀번호 확인" required>
						</div>

						<div class="col-md-6 offset-md-3">
							<span id="checkPwd2">&nbsp;</span>
						</div>
					</div>
					<br>

					<!-- 이름 -->
					<div class="row mb-3 form-row">
						<div class="col-md-3">
							<label for="name">* 이름</label>
						</div>
						<div class="col-md-6">
							<input type="text" class="form-control" id="name" name="name" required>
						</div>

						<div class="col-md-6 offset-md-3">
							<span id="checkName">&nbsp;</span>
						</div>
					</div>

					<!-- 전화번호 -->
					<div class="row mb-3 form-row">
						<div class="col-md-3">
							<label for="phone1">* 전화번호</label>
						</div>
						<!-- 전화번호1 -->
						<div class="col-md-3">
							<select class="custom-select" id="phone1" name="phone1" required>
								<option>010</option>
								<option>011</option>
								<option>016</option>
								<option>017</option>
								<option>019</option>
							</select>
						</div>

						<!-- number타입의 input태그에는 maxlength를 설정할 수 없음 -->
						<!-- 전화번호2 -->
						<div class="col-md-3">
							<input type="number" class="form-control phone" id="phone2" name="phone2" required>
						</div>

						<!-- 전화번호3 -->
						<div class="col-md-3">
							<input type="number" class="form-control phone" id="phone3" name="phone3" required>
						</div>

						<div class="col-md-6 offset-md-3">
							<span id="checkPhone">&nbsp;</span>
						</div>
					</div>

					<!-- 이메일 -->
					<div class="row mb-3 form-row">
						<div class="col-md-3">
							<label for="email">* Email</label>
						</div>
						<div class="col-md-6">
							<input type="email" class="form-control" id="email" name="email" autocomplete="off" required>
						</div>

						<div class="col-md-6 offset-md-3">
							<span id="checkEmail">&nbsp;</span>
						</div>
					</div>
					<br>

					<!-- 주소 -->
					<!-- 오픈소스 도로명 주소 API -->
					<!-- https://www.poesis.org/postcodify/ -->
					<div class="row mb-3 form-row">
						<div class="col-md-3">
							<label for="postcodify_search_button">우편번호</label>
						</div>
						<div class="col-md-3">
							<input type="text" name="post" class="form-control postcodify_postcode5">
						</div>
						<div class="col-md-3">
							<button type="button" class="btn btn-primary" id="postcodify_search_button">검색</button>
						</div>
					</div>

					<div class="row mb-3 form-row">
						<div class="col-md-3">
							<label for="address1">도로명 주소</label>
						</div>
						<div class="col-md-9">
							<input type="text" class="form-control postcodify_address" name="address1" id="address1">
						</div>
					</div>

					<div class="row mb-3 form-row">
						<div class="col-md-3">
							<label for="address2">상세주소</label>
						</div>
						<div class="col-md-9">
							<input type="text" class="form-control postcodify_details" name="address2" id="address2">
						</div>
					</div>

					<!-- 관심분야 -->
					<hr class="mb-4">
					<div class="row">
						<div class="col-md-3">
							<label>관심 분야</label>
						</div>
						<div class="col-md-9 custom-control custom-checkbox">
							<div class="form-check form-check-inline">
								<input type="checkbox" name="memberInterest" id="sports" value="운동" class="form-check-input custom-control-input">
								<label class="form-check-label custom-control-label" for="sports">운동</label>
							</div>
							<div class="form-check form-check-inline">
								<input type="checkbox" name="memberInterest" id="movie" value="영화" class="form-check-input custom-control-input">
								<label class="form-check-label custom-control-label" for="movie">영화</label>
							</div>
							<div class="form-check form-check-inline">
								<input type="checkbox" name="memberInterest" id="music" value="음악" class="form-check-input custom-control-input">
								<label class="form-check-label custom-control-label" for="music">음악</label>
							</div>
							<br>
							<div class="form-check form-check-inline">
								<input type="checkbox" name="memberInterest" id="cooking" value="요리" class="form-check-input custom-control-input">
								<label class="form-check-label custom-control-label" for="cooking">요리</label>
							</div>
							<div class="form-check form-check-inline">
								<input type="checkbox" name="memberInterest" id="game" value="게임" class="form-check-input custom-control-input">
								<label class="form-check-label custom-control-label" for="game">게임</label>
							</div>
							<div class="form-check form-check-inline">
								<input type="checkbox" name="memberInterest" id="etc" value="기타" class="form-check-input custom-control-input">
								<label class="form-check-label custom-control-label" for="etc">기타</label>
							</div>
						</div>
					</div>

					<hr class="mb-4">
					<button class="btn btn-primary btn-lg btn-block" type="submit">가입하기</button>
				</form>
			</div>
		</div>
		<br>
		<br>

		<!-- jQuery와 postcodify(가장 간단한 도로명 주소를 얻어오는 API) 를 로딩한다. -->
		<script src="//d1p7wdleee1q2z.cloudfront.net/post/search.min.js"></script>
		
		<!-- 회원 관련 Javascript 코드를 모아둘 wsp_member.js 파일을 작성 -->
		<script src="${contextPath}/resources/js/wsp_member.js"></script>

	</div>
	<jsp:include page="../common/footer.jsp"></jsp:include>
</body>
</html>
```
<br>

### wsp_member.js








## Servlet 전체

## header 전체