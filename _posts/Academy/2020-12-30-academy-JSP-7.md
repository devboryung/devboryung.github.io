---
title: "2020년 12월 30일"
excerpt: "WSP-5"
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

## 비밀번호 수정

### 출력화면

![비밀번호 변경 화면](https://user-images.githubusercontent.com/73421820/103325098-907eb800-4a8d-11eb-8af9-3e890506d0e3.png)
<br><br>

### 비밀번호 변경을 눌렀을때  
changePwd.do 서버로 요청을 보냄.
<br>
sideMenu.jsp
```html
<li class="list-group-item list-group-item-action"><a href="changePwd.do">비밀번호 변경</a></li>
```
<br>



### 요청을 위임해주는 Servlet
changePwd.do로 요청을 위임해주는 서블릿<br>
비밀번호 변경해주는 폼을 보이게 해주는 서블릿이다.
```java
package com.kh.wsp.member.controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

// changePwd.do로 요청을 위임해주는 서블릿
// 비밀번호 변경해주는 폼을 보이게 해주는 서블릿이다.


// Servlet 서블릿에서는 절대경로 형식으로 적어야함
@WebServlet("/member/changePwd.do")
public class ChangePwdFormServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		// changePwd.jsp로 요청 위임 : 
		// changePwd.jsp가 존재하는 파일 경로
		String path = "/WEB-INF/views/member/changePwd.jsp";
				
	    // 요청 위임 객체
		RequestDispatcher view = request.getRequestDispatcher(path);
		view.forward(request, response);
	
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}
```
<br>

### 비밀번호 변경을 위한 폼



action="updatePwd.do" :  updatePwd.do 로 요청을 보냄<br>
onsubmit="return pwdValidate();"  : 유효성 검사<br><br>
<br>

changePwd.jsp
```html
<form method="POST" action="updatePwd.do" onsubmit="return pwdValidate();" class="form-horizontal" role="form">
```
<br>

### 스크립트의 경로를 설정
새로운 비밀번호 유효성 검사를 해야되는데<br>
changePwd.jsp에는 자바스크립트 경로 설정이 안 되어있음<br>
body태그 제일 아래에 추가해준다.<br>
changePwd.jsp
```html
<script src="${contextPath }/resources/js/wsp_member.js"></script>
```
<br>

### 비밀번호 변경 유효성 검사
```js
// 비밀 번호 수정 -----------------------------------------------
function pwdValidate(){
    
    var regExp = /^[a-zA-Z\d]{6,12}$/;

    // 새로운 비밀번호 유효성 검사
    if(!regExp.test( $("#newPwd1").val() ) ){
        swal("비밀번호 형식이 유효하지 않습니다.");
        $("#newPwd1").focus();

        return false;
    }

    // 새로운 비밀번호와 비밀번호 확인이 일치하지 않을 때
    if( $("#newPwd1").val() != $("#newPwd2").val() ){
        swal("새로운 비밀번호가 일치하지 않습니다.");
       
        $("#newPwd1").focus();
        // 입력한 비밀번호가 전부 지워짐
        $("#newPwd1").val("");
        $("#newPwd2").val("");
        
        return false;
    }
}
```
<br>

### 유효성 검사 화면
![유효성검사](https://user-images.githubusercontent.com/73421820/103325205-16026800-4a8e-11eb-8a3e-c39d6aa27ec0.png)
<br><br>

## 비밀번호 변경 버튼 클릭시 요청 받음
비밀번호 변경하기 버튼을 누르면 요청을 받기 위한 서블릿
<br>

UpdatePwdServlet.java
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

@WebServlet("/member/updatePwd.do")
public class UpdatePwdServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
  throws ServletException, IOException {
		// 비밀번호 변경을 위해 필요한 데이터 얻어오기 : 현재 비밀번호, 새로운 비밀번호, 회원 번호
		String currentPwd = request.getParameter("currentPwd");
		String newPwd1 = request.getParameter("newPwd1");
		
		// 회원번호는 로그인한 회원의 정보 = session에 담겨져 있다.
		// 현재 로그인한 회원의 정보를 얻어 옴.
		HttpSession session = request.getSession();
		Member loginMember = (Member)session.getAttribute("loginMember");
		// session.getAttribute("loginMember") : object타입 -> Member로 형변환
		
		
		try {
			// DB로 넘어갈 데이터 3개(현재 비밀번호,바꿀 비밀번호, 회원번호) 객체로 가져가면 편하다.
			// 비밀번호는 로그인멤버에 셋팅하지 않음, 자리가 비어져있음
			// 임시로 loginMember에 현재 비밀번호를 담아 매개변수로 전달함.
			loginMember.setMemberPwd(currentPwd);
			// 비즈니스 로직 처리 후 결과 반환 받기
			int result = new MemberService().updatePwd(loginMember, newPwd1);
			// result = -1, 0, 1
``` 
<br>

### updatePwd Service
```java
/**	비밀번호 변경 Service
	 * @param loginMember
	 * @param newPwd1
	 * @return result
	 * @throws Exception
	 */
	public int updatePwd(Member loginMember, String newPwd1) throws Exception {
		Connection conn = getConnection();
		
		// 1) 현재 비밀번호가 일치하는지 검사
		// 비밀번호가 암호화 되어 있음. EncryptWrapper.java에  currentPwd, newPwd1  case 추가
		// 현재 비밀번호 loginMember에 담겨져있음.
		int result = dao.checkCurrentPwd(conn, loginMember);
		
		// 2) 현재 비밀번호 일치 시 새 비밀번호로 수정
		if(result>0) { // 현재 비밀번호 일치
			// 누구의 비밀번호를 변경해야하는지? 회원 정보를 가져가야 한다.
			// loginMember의 비밀번호 필드에 newPwd1를 세팅하여 재활용.
			loginMember.setMemberPwd(newPwd1);
			result = dao.updatePwd(conn, loginMember);
			
			// 트랜잭션 처리
			if(result>0) commit(conn);
			else 		rollback(conn);
		
			
		}else { // 현재 비밀번호 불일치
			result = -1;
		}
		
		close(conn);
		return result; // result -1, 1, 0
	}
```
<br>

### 비밀번호 암호화 진행
암호화 순서 : <br> 
member/updatePwd.do로 요청이 오면
request객체만 wrapper로 감싸서
getParameter 오버라이딩해서 키값이 일치하는 것이 있으면
암호화를 진행함.<br>
1. EncryptFilter에 비밀번호 변경url 추가<br>
EncryptFilter
```java
@WebFilter(urlPatterns =
 {"/member/login.do","/member/signUp.do", "/member/updatePwd.do"})
```
<br>

2. EncryptWrapper에  case 추가<br>
EncryptWrapper
```java
switch(name) {
			case "memberPwd" : // 로그인 시 비밀번호 name 속성 값
			case "pwd1" : 	   // 회원가입 시 비밀번호 name 속성 값
			case "currentPwd": // 비밀번호 변경 시 현재 비밀번호 name 속성 값
			case "newPwd1" : // 비밀번호 변경 시 새 비밀번호 name 속성 값
           encPwd = getSha512(super.getParameter(name)) ;break;
```
<br><br><br>

### 현재 비밀번호 검사 DAO
```java
/** 현재 비밀번호 검사 DAO
* @param conn
* @param loginMember
* @return result
* @throws Exception
*/
public int checkCurrentPwd(Connection conn, Member loginMember) throws Exception {
int result =0;
String query = prop.getProperty("checkCurrentPwd");

try {
  pstmt = conn.prepareStatement(query);
  pstmt.setInt(1, loginMember.getMemberNo());
  pstmt.setString(2,loginMember.getMemberPwd());
  
  rset = pstmt.executeQuery();

  if(rset.next()) {
    result = rset.getInt(1); 
    // 조회된 한 행의 데이터 중에서 첫번째 컬럼의 값을 result에 저장
  }
  
}finally {
  close(rset);
  close(pstmt);
}
return result;
}
``` 
<br>

### 현재 비밀번호 검사 SQL
```sql
<entry key="checkCurrentPwd">
SELECT COUNT(*) FROM MEMBER
WHERE MEMBER_NO =?
AND MEMBER_PWD =?
<!--STATUS='Y' 조건은 이미 로그인에서 걸러졌으니 더이상 검사 할 필요 없다.  -->
</entry>
```
<br>

### 비밀번호 변경 DAO
```java
/** 비밀번호 변경 DAO
* @param conn
* @param loginMember
* @return result
* @throws Exception
*/
public int updatePwd(Connection conn, Member loginMember) throws Exception{
int result = 0;
String query = prop.getProperty("updatePwd");

try {
  pstmt = conn.prepareStatement(query);
  pstmt.setString(1, loginMember.getMemberPwd());
  pstmt.setInt(2, loginMember.getMemberNo());
  
  result = pstmt.executeUpdate();
  
}finally {
  close(pstmt);
}

return result;
}

```
<br>

### 비밀번호 변경 SQL
```sql
<entry key="updatePwd">
UPDATE MEMBER
SET MEMBER_PWD = ?
WHERE MEMBER_NO = ? 
</entry>
```
<br>

### Service에서 return받은 result로 알람창 출력
UpdatePwdServlet.java
```java
			String swalIcon = null;
			String swalTitle = null;
			
			if(result >0) { // 비밀번호 변경 성공
				swalIcon ="success";
				swalTitle ="비밀번호가 성공적으로 변경되었습니다.";
			
			}else if(result == 0) { // 비밀번호 변경 실패
				swalIcon = "error";
				swalTitle ="비밀번호 변경 실패했습니다.";
				
			}else { // 현재 비밀번호 불일치
				swalIcon = "warning";
				swalTitle ="현재 비밀번호를 다시 입력해주세요.";
			}
			
			session.setAttribute("swalIcon", swalIcon);
			session.setAttribute("swalTitle", swalTitle);
			
			// 현재 요청했던 페이지 그대로 두기
			// 요청을 했던 페이지로 다시 돌아가는 구문.
			response.sendRedirect(request.getHeader("referer"));
			
		}catch(Exception e) {
			e.printStackTrace();
			String path = "/WEB-INF/views/common/errorPage.jsp";
			
			request.setAttribute("errorMsg", "비밀번호 변경 과정에서 오류 발생");
			
			RequestDispatcher view = request.getRequestDispatcher(path);
			view.forward(request, response);
					
		}
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) 
  throws ServletException, IOException {
		doGet(request, response);
	}

}

```
<br>

### 화면
1. 비밀번호 변경 전 DB
<br>
![변경 전](https://user-images.githubusercontent.com/73421820/103327152-c70d0080-4a96-11eb-99ce-8349f3277a93.png)
<br>

2. 현재 비밀번호가 틀렸을 때 
<br>
![현재 비밀번호 틀렸을 때](https://user-images.githubusercontent.com/73421820/103327153-c7a59700-4a96-11eb-9bb5-50ca0674a929.png)
<br>

3. 비밀번호 변경 성공
<br>
![비밀번호 변경 성공](https://user-images.githubusercontent.com/73421820/103327157-c83e2d80-4a96-11eb-9427-0c260118b173.png)
<br>

4. 변경 후 DB
<br>
![변경 후](https://user-images.githubusercontent.com/73421820/103327159-c8d6c400-4a96-11eb-8442-c020d7e73941.png)
<br>
<br><br>




## 회원 탈퇴

### 회원 탈퇴 버튼
회원 탈퇴 버튼을 누르면 secession.do로 요청을 보냄
<br>
sideMenu.jsp
```html
<li class="list-group-item list-group-item-action">
  <a href="secession.do">회원 탈퇴</a>
</li>
```
<br>

### 회원 탈퇴 화면으로 요청 위임 보내는 서블릿
SecessionFormServlet
<br>

```java
package com.kh.wsp.member.controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/member/secession.do")
public class SecessionFormServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		String path = "/WEB-INF/views/member/secession.jsp";
		RequestDispatcher view = request.getRequestDispatcher(path);
		view.forward(request, response);
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		doGet(request, response);
	}
}
```
<br>

### 회원 탈퇴 화면
![회원탈퇴 화면](https://user-images.githubusercontent.com/73421820/103334502-ed419900-4ab4-11eb-9ced-279cff02f575.png)
<br>

### 회원약관 동의 체크된 상태에서만 회원 탈퇴 가능
secession.jsp
```html
</div>
    <div class="checkbox pull-right">
      <div class="custom-checkbox">
        <div class="form-check">
          <input type="checkbox" name="agree" id="agree"
            class="form-check-input custom-control-input"> <br>
          <label class="form-check-label custom-control-label"
            for="agree">위 약관에 동의합니다.</label>
        </div>
      </div>
    </div>
  </div>
</div>
</div>
```
<br>

### 자바스크립트 연결
secession.jsp
```html
<script src="${contextPath }/resources/js/wsp_member.js"></script>
```
<br>

### 회원약관 동의 체크 확인
wsp_member.js
```js
// 회원 탈퇴 약관 동의 체크 확인 --------------------------------------
function secessionValidate(){
    if(!$("#agree").prop("checked")){
        // #agree 체크박스가 체크되어 있지 않다면
        swal("약관에 동의해 주세요.");
        return false;
    }else{
        return confirm("정말로 탈퇴 하시겠습니까?");
        /* confirm : alert 창이 뜨면서 확인/취소 버튼이 있다.
          확인 누르면 true 반환, 취소 누르면 false 반환 */
    }
}
```
<br>

약관 동의 체크 하지 않고 탈퇴 버튼을 눌렀을 때<br>
![약관동의 체크 안했을때](https://user-images.githubusercontent.com/73421820/103340833-77472d00-4ac8-11eb-8e86-4c9e5f999b8b.png)
<br>



<br>

### 탈퇴버튼을 누르면
updateStatus.do로 요청이 전달 됨.
<br>
secession.jsp
```html
<form method="POST" action="updateStatus.do" 
onsubmit="return secessionValidate();" 			class="form-horizontal" role="form">
```
<br>

### 요청을 받을 수 있는 servlet
UpdateStatusServlet.java
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
import javax.websocket.Session;

import com.kh.wsp.member.model.service.MemberService;
import com.kh.wsp.member.model.vo.Member;

@WebServlet("/member/updateStatus.do")
public class UpdateStatusServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		
		// 회원 탈퇴에 필요한 정보 
		// 현재 비밀번호, Session에 있는 로그인 회원 정보
		String currentPwd = request.getParameter("currentPwd");
		
		HttpSession session = request.getSession();
		Member loginMember = (Member)session.getAttribute("loginMember");
		
		// loginMember에 현재 비밀번호 값 셋팅
		loginMember.setMemberPwd(currentPwd);

		try {
	
			// 비즈니스 로직 수행 후 결과 반환 받기
			int result = new MemberService().updateStatus(loginMember);
			// result = -1,0,1
```
<br>

### 비밀번호 암호화를 위해 url 추가
EncryptFilter에 회원 탈퇴 url 추가
<br>
EncryptFilter.java

```java
@WebFilter(urlPatterns = {"/member/login.do","/member/signUp.do", 
"/member/updatePwd.do", "/member/updateStatus.do"})
```
<br>

### 회원 탈퇴 DAO
MemberDAO.java
```java
/** 회원탈퇴 DAO
* @param conn
* @param loginMember
* @return result
* @throws Exception
*/
public int updateStatus(Connection conn, Member loginMember) throws Exception {
int result=0;
      
String query = prop.getProperty("updateStatus");
try {
  pstmt = conn.prepareStatement(query);
  pstmt.setInt(1, loginMember.getMemberNo());
  
  result = pstmt.executeUpdate();
  
}finally {
  close(pstmt);
}
return result;
}
```

### 회원 탈퇴 SQL
member-query.xml
```sql
<entry key="updateStatus">
UPDATE MEMBER SET
MEMBER_STATUS = 'N'
WHERE MEMBER_NO=?
</entry>
```
<br>


### 비즈니스 로직 수행 후 반환받은 result
UpdateStatusServlet.java
```java
			String swalIcon = null;
			String swalTitle = null;
			String url = null;
			
			if(result >0) { // 탈퇴 완료
				swalIcon ="success";
				swalTitle ="회원 탈퇴가 성공적으로 완료되었습니다.";
				// 탈퇴 성공 시 -> 로그아웃 + 메인페이지로 전환
				
				// 로그 아웃 == 세션 무효화
				session.invalidate();
				// 문제점 : 세션 무효화 시 현재 얻어온 세션을 사용할 수 없는 문제점이 있다.
				// swal이 세팅 될 세션이 없다.
				
				// 새로운 세션 얻어오기
				session = request.getSession();
				
				url = request.getContextPath();
				
				
			}else if(result == 0) { //탈퇴 실패
				swalIcon ="error";
				swalTitle ="회원 탈퇴가 실패했습니다.";
				
				// 탈퇴 실패 시 -> 현재 화면 유지
				url = "secession.do";
			
			}else { // 현재 비밀번호 불일치
				swalIcon ="warning";
				swalTitle ="현재 비밀번호가 일치하지 않습니다.";
				
				// 탈퇴 실패 시 -> 현재 화면 유지
				url = "secession.do";
			}
			
			session.setAttribute("swalIcon", swalIcon);
			session.setAttribute("swalTitle", swalTitle);
			
			// 회원탈퇴 성공시 메인화면/ 실패시 현재화면으로 이동함.
			response.sendRedirect(url);
			
			
		}catch(Exception e) {
			e.printStackTrace();
			
			String path = "/WEB-INF/views/common/errorPage.jsp";
			
			request.setAttribute("errorMsg", "회원탈퇴 과정에서 오류 발생");
			
			RequestDispatcher view = request.getRequestDispatcher(path);
			view.forward(request, response);
		}
		
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		doGet(request, response);
	}

}
```
<br>

### 회원 탈퇴 화면
1. 비밀번호를 잘못 입력했을 때<br>
![회원탈퇴 비밀번호 일치하지않음](https://user-images.githubusercontent.com/73421820/103340874-96de5580-4ac8-11eb-9c27-43e6cf2a801f.png)
<br>

2. 회원 탈퇴 성공 <br>
![회원탈퇴 성공](https://user-images.githubusercontent.com/73421820/103340876-9776ec00-4ac8-11eb-8bcc-4773a2edfc8b.png)
<br>

3. DB 정보 수정<br>
![db변경](https://user-images.githubusercontent.com/73421820/103340877-980f8280-4ac8-11eb-91bb-134f3cd5849f.png)
<br><br><br>




## 전체 changePwd.jsp
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>내정보</title>
</head>
<style>
	#content-main{
	height: 830px;}
</style>
<body>
	<div class="container" id="content-main">
		<jsp:include page="../common/header.jsp"></jsp:include>

<div class="row my-5">
  <jsp:include page="sideMenu.jsp"></jsp:include>

  <div class="col-sm-8">
    <h3>비밀번호 변경</h3>
    <hr>
    <div class="bg-white rounded shadow-sm container p-3">
      <form method="POST" action="updatePwd.do" 
      onsubmit="return pwdValidate();" 
      class="form-horizontal" role="form">
        <!-- 아이디 -->
        <div class="row mb-3 form-row">
          <div class="col-md-3">
            <h6>아이디</h6>
          </div>
          <div class="col-md-6">
            <h5 id="id">${loginMember.memberId }</h5>
          </div>
        </div>

        <hr>
        <!-- 현재 비밀번호 -->
        <div class="row mb-3 form-row">
          <div class="col-md-3">
            <h6>현재 비밀번호</h6>
          </div>
          <div class="col-md-6">
            <input type="password" class="form-control" 
            id="currentPwd" name="currentPwd">
          </div>
        </div>

        <!-- 새 비밀번호 -->
        <div class="row mb-3 form-row">
          <div class="col-md-3">
            <h6>새 비밀번호</h6>
          </div>
          <div class="col-md-6">
            <input type="password" class="form-control" 
            id="newPwd1" name="newPwd1">
          </div>
        </div>

        <!-- 새 비밀번호 확인-->
        <div class="row mb-3 form-row">
          <div class="col-md-3">
            <h6>새 비밀번호 확인</h6>
          </div>
          <div class="col-md-6">
            <input type="password" class="form-control" 
            id="newPwd2" name="newPwd2">
          </div>
        </div>
        
        <hr class="mb-4">
        <button class="btn btn-primary btn-lg btn-block" 
        type="submit">변경하기</button>
      </form>
    </div>
  </div>
</div>
</div>
<jsp:include page="../common/footer.jsp"></jsp:include>


<script src="${contextPath }/resources/js/wsp_member.js"></script>
	
</body>
</html>
```
<br>

## 전체 secession.jsp
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>내정보</title>
</head>
<style>
#content-main {
	height: 830px;
}
</style>
<body>
<div class="container" id="content-main">
<jsp:include page="../common/header.jsp"></jsp:include>

<div class="row my-5">
  <jsp:include page="sideMenu.jsp"></jsp:include>

<div class="col-sm-offset-2 col-sm-8">
  <h3>회원 탈퇴</h3>
  <div class="bg-white rounded shadow-sm container p-3">
    <form method="POST" action="updateStatus.do" onsubmit="return secessionValidate();" 
      class="form-horizontal" role="form">
      <!-- 아이디 -->
      <div class="row mb-3 form-row">
        <div class="col-md-3">
          <h6>아이디</h6>
        </div>
        <div class="col-md-6">
          <h5 id="id">${loginMember.memberId }</h5>
        </div>
      </div>

      <!-- 비밀번호 -->
      <div class="row mb-3 form-row">
        <div class="col-md-3">
          <h6>비밀번호</h6>
        </div>
        <div class="col-md-6">
          <input type="password" class="form-control" id="currentPwd"
            name="currentPwd">
        </div>
      </div>

      <hr>
      <div class="panel panel-default">

        <div class="panel-body">
          <div class="form-group pull-left">
            <label class="control-label"> 
                  회원 탈퇴 약관 
            </label>
            <div class="col-xs-12">
              <textarea class="form-control" readonly rows="10" cols="100">
제1조
이 약관은 샘플 약관입니다.

① 약관 내용 1

② 약관 내용 2

③ 약관 내용 3

④ 약관 내용 4


제2조
이 약관은 샘플 약관입니다.

① 약관 내용 1

② 약관 내용 2

③ 약관 내용 3

④ 약관 내용 4
</textarea>
        </div>
        <div class="checkbox pull-right">
          <div class="custom-checkbox">
            <div class="form-check">
              <input type="checkbox" name="agree" 
              id="agree"  class="form-check-input custom-control-input"> 
              <br>
              <label class="form-check-label custom-control-label"
                for="agree">위 약관에 동의합니다.</label>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <hr class="mb-4">
  <button class="btn btn-primary btn-lg btn-block" 
  id="btn"
    type="submit">탈퇴</button>
</form>
</div>
</div>
</div>
</div>
	<%@ include file="../common/footer.jsp"%>


	<script src="${contextPath }/resources/js/wsp_member.js"></script>

</body>
</html>
```
<br><br><br><br>