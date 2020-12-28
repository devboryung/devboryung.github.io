---
title: "2020년 12월 28일"
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

### 아이디 signUpForm.jsp
```html

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
```
<br>

### 아이디 wsp_member.js
```js
// 아이디 유효성 검사
// 첫 글자는 영어 소문자, 나머지 글자는 영어 대,소문자 + 숫자, 총 6~12글자
$("#id").on("input",function(){
    var regExp = /^[a-z][a-zA-Z\d]{5,11}$/;

    var value = $("#id").val();
    if(!regExp.test(value) ){
        $("#checkId").text("유효하지 않은 아이디 형식입니다.").css("color","red");
        validateCheck.id = false;
    }else{
        $("#checkId").text("유효한 아이디 형식입니다.").css("color","green");
        validateCheck.id = true;
    }
});
```
<br>
<br>

### 비밀번호 signUpForm.jsp
```html
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
```
<br>


### 비밀번호 wsp_member.js
```js
// 비밀번호 유효성 검사
// 영어 대,소문자 + 숫자, 총 6~12글자
// + 비밀번호, 비밀번호 확인의 일치 여부
// + 비밀번호를 입력하지 않거나 유효하지 않은 상태로
//   비밀번호 확인을 작성하는 경우 비밀번호를 먼저 입력해야함.

$("#pwd1, #pwd2").on("input",function(){

    // 비밀번호 유효성 검사
    var regExp = /^[a-zA-Z\d]{6,12}$/;

    var v1 = $("#pwd1").val();
    var v2 = $("#pwd2").val();

    if(!regExp.test(v1)){
        $("#checkPwd1").text("유효하지 않은 비밀번호 형식입니다.").css("color","red");
        validateCheck.pwd1 = false;
    }else{
        $("#checkPwd1").text("유효한 비밀번호 형식입니다.").css("color","green");
        validateCheck.pwd1 = true;
    }
    
    // 비밀번호가 유효하지 않은 상태에서 비밀번호 확인 작성 시
    if(!validateCheck.pwd1 && v2.length>0){
        swal("유효한 비밀번호를 먼저 작성해주세요.");
        $("#pwd2").val(""); // 비밀번호 확인에 입력한 값 삭제
        $("#pwd1").focus(); // 커서를 비밀번호로 이동
    }else{
        // 비밀번호, 비밀번호 확인의 일치 여부
        if(v1.length ==0 || v2.length == 0){
            $("#checkPwd2").html("&nbsp;");
        }else if(v1 == v2){
            $("#checkPwd2").text("비밀번호 일치").css("color","green");
            validateCheck.pwd2 = true;
        }else{
            $("#checkPwd2").text("비밀번호 불일치").css("color","red");
            validateCheck.pwd2 = false;
        }
    }
});
```
<br><br>

### 이름 signUpForm.jsp
```html
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
```
<br>


### 이름 wsp_member.js
```js
/*
이름 유효성 검사
/^[가-힣]{2,}$/; // 한글 두 글자 이상
*/
$("#name").on("input",function(){
    var regExp = /^[가-힣]{2,}$/;

    var value = $("#name").val();

    if(!regExp.test(value)){
        $("#checkName").text("유효하지 않은 이름 형식입니다.").css("color","red");
        validateCheck.name = false;
    }else{
        $("#checkName").text("유효한 이름 형식입니다.").css("color","green");
        validateCheck.name = true;
    }
});
```
<br><br>

### 전화번호 signUpForm.jsp
```html
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
```
<br>


### 전화번호 wsp_member.js
```js
// 전화번호 유효성 검사
$(".phone").on("input",function(){
  // 전화번호 input 태그에 4글자 초과 입력하지 못하게 하는 이벤트
  if ($(this).val().length > 4) {
    $(this).val( $(this).val().slice(0, 4));
 }  

 var regExp1 = /^\d{3,4}$/;
 var regExp2 = /^\d{4}$/;
 
 var v1 = $("#phone2").val();
 var v2 = $("#phone3").val();

 if(!regExp1.test(v1) || !regExp2.test(v2)){
    $("#checkPhone").text("유효하지 않은 전화번호 형식입니다.").css("color","red");
    validateCheck.phone2 = false;
 }else{
    $("#checkPhone").text("유효한 전화번호 형식입니다.").css("color","green");
    validateCheck.phone2 = true;
}
});
```
<br><br>

### 이메일 signUpForm.jsp
```html
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
```
<br>


### 이메일 wsp_member.js
```js
/* 
이메일 유효성 검사
/^[\w]{4,}@[\w]+(\.[\w]+){1,3}$/; // 4글자 아무단어 @ 아무단어 . * 3
*/

$("#email").on("input",function(){
        // \w == [^A-Za-z0-9_] 와 동일
    var regExp = /^[\w]{4,}@[\w]+(\.[\w]+){1,3}$/;

    var value = $("#email").val();

    if(!regExp.test(value)){
        $("#checkEmail").text("유효하지 않은 이메일 형식입니다.").css("color","red");
        validateCheck.email = false;
    }else{
        $("#checkEmail").text("유효한 이메일 형식입니다.").css("color","green");
        validateCheck.email = true;
    }
});
```
<br><br>

### 주소 signUpForm.jsp
```html
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
```
<br>

### 주소 wsp_member.js
```js
// 주소 입력
$(function(){
    $("#postcodify_search_button").postcodifyPopUp();
});
```
<br><br>

### validate 검증 signUpForm.jsp
```html
<form method="POST" action="signUp.do" class="needs-validation" name="signUpForm" onsubmit="return validate();"> 
```

### 유효성 검사를 확인하기 위한 객체 생성 wsp_member.js
js의 제일 상단에 작성하기.
<br>
```js
// 입력 값들이 유효성 검사가 진행되었는지 확인하기 위한 객체 생성
var validateCheck = {
    "id" : false,
    "pwd1" : false,
    "pwd2" : false,
    "name" : false,
    "phone2" : false,
    "email" : false
}
```

<br>

### 유효성 검사 여부 확인 wsp_member.js
```js
function validate(){

    // 유효성 검사 여부 확인
    // 객체에 순서대로 접근
    for(var key in validateCheck){
        if(!validateCheck[key]){

            var msg;
            switch(key){
                case "id" : msg="아이디가"; break;
                case "pwd1" :  
                case "pwd2" : msg="비밀번호가"; break;
                case "name" : msg="이름이"; break;
                case "phone2" : msg="전화번호가"; break;
                case "email" : msg="이메일이"; break;
            }

            swal(msg + " 유효하지 않습니다.");
            $("#"+key).focus();
            // 기본 이벤트 제거
            return false;
        }
    }
}
```
<br><br>

## 회원가입을 위한 SignUpServlet
```java
package com.kh.wsp.member.controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import com.kh.wsp.member.model.service.MemberService;
import com.kh.wsp.member.model.vo.Member;

@WebServlet("/member/signUp.do")
public class SignUpServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		// POST 방식 전송 시 한글이 깨지는 문제를 해결하기 위해 인코딩 변경
		request.setCharacterEncoding("UTF-8");
		
		// 전달 받은 파라미터를 모두 변수에 저장.
		String memberId = request.getParameter("id");
		String memberPwd = request.getParameter("pwd1");
		String memberName = request.getParameter("name");
		String memberEmail = request.getParameter("email");
		
		String phone1 = request.getParameter("phone1");
		String phone2 = request.getParameter("phone2");
		String phone3 = request.getParameter("phone3");
		// 전화번호를 '-'를 구분자로 하여 하나로 합치기
		String memberPhone = phone1+"-"+phone2+"-"+phone3;
		
		String post = request.getParameter("post"); //우편번호
		String address1 = request.getParameter("address1"); // 도로명 주소
		String address2 = request.getParameter("address2"); // 상세주소
		// 주소를 ","를 구분자로 하여 하나로 합치기
		String memberAddress = post+","+address1+","+address2;
		
		String[] interest = request.getParameterValues("memberInterest");
		
		String memberInterest = null;
		
		if(interest != null ) memberInterest = String.join(", ", interest);
		
		
		// Member 객체를 생성하여 파라미터를 모두 저장
		Member member = new Member(memberId, memberPwd, memberName,memberPhone, 
									memberEmail, memberAddress, memberInterest);
		
		// 비즈니스 로직 수행
		try {
			// 비즈니스 로직 수행 후 결과를 반환 받아 저장
			
			int result = new MemberService().signUp(member);
			// MemberService를 일회성으로 사용하기 위해  new를 사용해서 작성함.
			
			String swalIcon = null;
			String swalTitle = null;
			String swalText = null;
			
			if(result>0) { //성공
				swalIcon = "success";
				swalTitle = "회원가입 성공!";
				swalText = memberName + "님 환영합니다.";
			}else { //실패
				swalIcon ="error";
				swalTitle = "회원가입 실패";
				swalText = "문제가 지속될 경우 고객센터로 문의 바랍니다.";
			}
			
			HttpSession session = request.getSession();
			
			session.setAttribute("swalIcon", swalIcon);
			session.setAttribute("swalTitle", swalTitle);
			session.setAttribute("swalText", swalText);
			
			// 회원가입 진행 후 메인 페이지로 이동(첫화면 재요청)
			response.sendRedirect(request.getContextPath());
			
			
		}catch(Exception e) {
			e.printStackTrace();
			request.setAttribute("errorMsg", "회원가입 과정에서 오류가 발생했습니다.");
			String path = "/WEB-INF/views/common/errorPage.jsp";
			RequestDispatcher view = request.getRequestDispatcher(path);
			view.forward(request,response);
		}
		
	}
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}

```

<br> 

### Member.vo에 매개변수 7개인 생성자 생성. 
```java
public Member(String memberId, String memberPwd, String memberName, String memberPhone, String memberEmail,
    String memberAddress, String memberInterest) {
  super();
  this.memberId = memberId;
  this.memberPwd = memberPwd;
  this.memberName = memberName;
  this.memberPhone = memberPhone;
  this.memberEmail = memberEmail;
  this.memberAddress = memberAddress;
  this.memberInterest = memberInterest;
}
```
<br>

### 회원가입 Service
```java
/** 회원가입 Service
	 * @param member
	 * @return result 
	 * @throws Exception
	 */
	public int signUp(Member member)throws Exception {
		
		// 1) Connection 얻어오기
		Connection conn = getConnection();
		
		// 2) DAO 메소드 호출 후 결과 반환
		int result = dao.signUp(conn, member);
		
		// 3) 트랜잭션 처리
		if(result>0) commit(conn);
		else         rollback(conn);
		
		// 4) Connection 반환
		close(conn);
		
		// 5) 결과값 반환
		return result;
	}
```
<br>

### 회원가입 SQL
```sql
<entry key="signUp">
INSERT INTO MEMBER
VALUES(SEQ_MNO.NEXTVAL, ?, ?, ?, ?, ?, ?, ?, DEFAULT, DEFAULT, DEFAULT)
</entry>
```
<br>

### 회원가입 DAO
```java
/** 회원가입 DAO
	 * @param conn
	 * @param member
	 * @return result
	 * @throws Exception
	 */
	public int signUp(Connection conn, Member member) throws Exception {
		
		// 1) 결과를 저장할 변수 선언
		int result =0;
		
		// 2) SQL 구문 준비
		String query = prop.getProperty("signUp");
		try {
			// 3) PreparedStatement 객체를 얻어와 SQL구문을 세팅
			pstmt = conn.prepareStatement(query);
			
			// 4) 위치 홀더(?) 에 알맞은 값 세팅
			pstmt.setString(1, member.getMemberId());
			pstmt.setString(2, member.getMemberPwd());
			pstmt.setString(3, member.getMemberName());
			pstmt.setString(4, member.getMemberPhone());
			pstmt.setString(5, member.getMemberEmail());
			pstmt.setString(6, member.getMemberAddress());
			pstmt.setString(7, member.getMemberInterest());
			// 5) SQL 구문 수행 후 반환값 저장
			result = pstmt.executeUpdate();
		}finally {
			// 6) 사용한 JDBC 자원 반환
			close(pstmt);
		}
		// 7) 결과 반환
		return result;
	}
```
<br><br>

## 아이디 중복 검사

### signUpForm.jsp
```html
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
```
<br>

### 아이디 중복 체크창 오픈 wsp_member.js
```js
// 아이디 중복 체크창 오픈
$("#idDupCheck").on("click",function () {
    window.open("idDupForm.do", "idDupForm","width=450, height=250");
     //               팝업 주소         팝업창 name            설정
 });
 ```
 <br>

### idDupCheck.jsp
 ```html
 <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>아이디 중복 검사</title>
</head>
<style>
	body>*{
		margin-left: 75px;
	}
</style>

<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
<body>
	<h4 class="mt-3">아이디 중복 검사</h4>
	<br>
	
	<!-- 실질적인 중복 검사 수행  (회원가입에 작성한 id를 가지고 db로 가서 결과를 가지고 옴) -->
	<form action="${contextPath }/member/idDupCheck.do" id="idChekcForm" method="post">
		<input type="text" id="id" name="id">
		<input type="submit" value="중복확인">
	</form>
	<br>
	
	<!-- 사용 가능 여부 메세지 출력 -->
	<span>
	<c:if test="${!empty result }">
		<c:choose>
			<c:when test="${result == 0 }">사용 가능한 아이디입니다.</c:when>
			<c:otherwise>이미 사용중인 아이디입니다.</c:otherwise>
		</c:choose>
	</c:if>
	</span>
	<br>
	<br>
	
	<div>
		<input type="button" id="cancel" value="취소" onclick="window.close();">
		<input type="button" id="confirmId" value="확인" onclick="confirmId();">
	</div>
	
	<script type="text/javascript">
		
		var id;
		var result;
		
		// 팝업창이 오픈 완료 된 후 자동으로 실행
		$(function(){
			result = "${result}";
			/*${result} == 중복검사 실행했을 때 중복이 되면 1, 아니면 0 을 반환 받아옴, 처음에는 비어있음  */
			
			console.log(result);
			
			// 팝업창 최초 오픈 시 if문이 동작되고 중복 체크 버튼으로 인한 팝업창 재 요청 시 else문이 실행됨. 
			if(id == null && result == ""){
				id = opener.document.signUpForm.id.value; // 부모창의 아이디 저장
			}else{
				// 중복 체크 후 아이디 저장
				id = "${param.id}"; 
			}
			
			console.log(id);
			// 부모창에서 입력한 아이디 또는 중복검사를 진행한 아이디를 화면에 표시
			//document.getElementById("id").value = id;
			
			$("#id").val(id);
		});
		
		
		
		// 확인버튼을 눌렀을 경우 부모창에 전달할 값 제어
		function confirmId(){ 
			
			// 중복체크 결과 중복되는 아이디가 없을 경우(result가 0인 경우)
			if(result == 0){
				// 부모창 문서 내에서 signUpForm 이라는 이름의 태그 내에 id라는 이름을 가진 태그의 value값을
				// 현재 태그 중 id가 "id" 요소의 값을 대입.
				opener.document.signUpForm.id.value = $("#id").val();
				
				// 부모창에 type이 hidden인 요소의 값을 true로 변경
				opener.document.signUpForm.idDup.value = true;
			}else{
				
				// 중복인 상태로 확인을 누른 경우 부모창에 type이 hidden인 요소의 값을 false로 변경
				opener.document.singUpCheck.signUpForm.idDupCheck = false;
			}
		
			if(opener != null){ // 아이디 중복창 닫기
				opener.checkForm = null;
				self.close();
			}
		}
		
		$("#idChekcForm").submit(function(){
			var regExp = /^[a-z][a-zA-z\d]{5,11}$/;
			if(!regExp.test($("#id").val())){
				alert("유효하지 않은 아이디 형식 입니다.");
				return false;
      }
		});
		
		
	</script>
</body>
</html>
```
<br>

### IdDupFormServlet
```java
package com.kh.wsp.member.controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/member/idDupForm.do")
public class IdDupFormServlet extends HttpServlet {
   private static final long serialVersionUID = 1L;
       
   protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
      String path = "/WEB-INF/views/member/idDupCheck.jsp";
      RequestDispatcher view = request.getRequestDispatcher(path);
      view.forward(request, response);
   }

   protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
      doGet(request, response);
   }
}
```
<br><br>

## 전체 signUpForm.jsp
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
	<%-- 상대경로 : 현재 주소창에 있는 주소의 가장 마지막이 변경됨 --%>
	<form method="POST" action="signUp.do" class="needs-validation" name="signUpForm" onsubmit="return validate();"> 
																																											<!-- 기본 이벤트 제거 -->
<%--절대경로 :  action="${contextPath}/member/signUp.do" --%>

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
<br><br>

## 전체 wsp_member.js
```js
// 입력 값들이 유효성 검사가 진행되었는지 확인하기 위한 객체 생성
var validateCheck = {
    "id" : false,
    "pwd1" : false,
    "pwd2" : false,
    "name" : false,
    "phone2" : false,
    "email" : false
}

// 아이디 유효성 검사
// 첫 글자는 영어 소문자, 나머지 글자는 영어 대,소문자 + 숫자, 총 6~12글자
$("#id").on("input",function(){
    var regExp = /^[a-z][a-zA-Z\d]{5,11}$/;

    var value = $("#id").val();
    if(!regExp.test(value) ){
        $("#checkId").text("유효하지 않은 아이디 형식입니다.").css("color","red");
        validateCheck.id = false;
    }else{
        $("#checkId").text("유효한 아이디 형식입니다.").css("color","green");
        validateCheck.id = true;
    }
});

// 비밀번호 유효성 검사
// 영어 대,소문자 + 숫자, 총 6~12글자
// + 비밀번호, 비밀번호 확인의 일치 여부
// + 비밀번호를 입력하지 않거나 유효하지 않은 상태로
//   비밀번호 확인을 작성하는 경우 비밀번호를 먼저 입력해야함.

$("#pwd1, #pwd2").on("input",function(){

    // 비밀번호 유효성 검사
    var regExp = /^[a-zA-Z\d]{6,12}$/;

    var v1 = $("#pwd1").val();
    var v2 = $("#pwd2").val();

    if(!regExp.test(v1)){
        $("#checkPwd1").text("유효하지 않은 비밀번호 형식입니다.").css("color","red");
        validateCheck.pwd1 = false;
    }else{
        $("#checkPwd1").text("유효한 비밀번호 형식입니다.").css("color","green");
        validateCheck.pwd1 = true;
    }
    
    // 비밀번호가 유효하지 않은 상태에서 비밀번호 확인 작성 시
    if(!validateCheck.pwd1 && v2.length>0){
        swal("유효한 비밀번호를 먼저 작성해주세요.");
        $("#pwd2").val(""); // 비밀번호 확인에 입력한 값 삭제
        $("#pwd1").focus(); // 커서를 비밀번호로 이동
    }else{
        // 비밀번호, 비밀번호 확인의 일치 여부
        if(v1.length ==0 || v2.length == 0){
            $("#checkPwd2").html("&nbsp;");
        }else if(v1 == v2){
            $("#checkPwd2").text("비밀번호 일치").css("color","green");
            validateCheck.pwd2 = true;
        }else{
            $("#checkPwd2").text("비밀번호 불일치").css("color","red");
            validateCheck.pwd2 = false;
        }
    }
});



/*
이름 유효성 검사
/^[가-힣]{2,}$/; // 한글 두 글자 이상
*/
$("#name").on("input",function(){
    var regExp = /^[가-힣]{2,}$/;

    var value = $("#name").val();

    if(!regExp.test(value)){
        $("#checkName").text("유효하지 않은 이름 형식입니다.").css("color","red");
        validateCheck.name = false;
    }else{
        $("#checkName").text("유효한 이름 형식입니다.").css("color","green");
        validateCheck.name = true;
    }
});



// 전화번호 유효성 검사
$(".phone").on("input",function(){
  // 전화번호 input 태그에 4글자 초과 입력하지 못하게 하는 이벤트
  if ($(this).val().length > 4) {
    $(this).val( $(this).val().slice(0, 4));
 }  

 var regExp1 = /^\d{3,4}$/;
 var regExp2 = /^\d{4}$/;
 
 var v1 = $("#phone2").val();
 var v2 = $("#phone3").val();

 if(!regExp1.test(v1) || !regExp2.test(v2)){
    $("#checkPhone").text("유효하지 않은 전화번호 형식입니다.").css("color","red");
    validateCheck.phone2 = false;
 }else{
    $("#checkPhone").text("유효한 전화번호 형식입니다.").css("color","green");
    validateCheck.phone2 = true;
}
});






/* 
이메일 유효성 검사
/^[\w]{4,}@[\w]+(\.[\w]+){1,3}$/; // 4글자 아무단어 @ 아무단어 . * 3
*/

$("#email").on("input",function(){
        // \w == [^A-Za-z0-9_] 와 동일
    var regExp = /^[\w]{4,}@[\w]+(\.[\w]+){1,3}$/;

    var value = $("#email").val();

    if(!regExp.test(value)){
        $("#checkEmail").text("유효하지 않은 이메일 형식입니다.").css("color","red");
        validateCheck.email = false;
    }else{
        $("#checkEmail").text("유효한 이메일 형식입니다.").css("color","green");
        validateCheck.email = true;
    }
});

function validate(){

    // 유효성 검사 여부 확인
    // 객체에 순서대로 접근
    for(var key in validateCheck){
        if(!validateCheck[key]){

            var msg;
            switch(key){
                case "id" : msg="아이디가"; break;
                case "pwd1" :  
                case "pwd2" : msg="비밀번호가"; break;
                case "name" : msg="이름이"; break;
                case "phone2" : msg="전화번호가"; break;
                case "email" : msg="이메일이"; break;
            }

            swal(msg + " 유효하지 않습니다.");
            $("#"+key).focus();
            // 기본 이벤트 제거
            return false;
        }
    }

}

// 주소 입력
$(function(){
    $("#postcodify_search_button").postcodifyPopUp();
});


// 아이디 중복 체크창 오픈
$("#idDupCheck").on("click",function () {
    window.open("idDupForm.do", "idDupForm","width=450, height=250");
     //               팝업 주소         팝업창 name            설정
 });
```
<br><br>







