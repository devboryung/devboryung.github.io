---
title: "2020년 12월 23일"
excerpt: "Servlet - 4"
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

## 화면

### 메인화면
![메인화면](https://user-images.githubusercontent.com/73421820/103096516-82710780-4647-11eb-9963-cff73c7f1472.PNG)
<br><br>

### 로그인 모달창
![로그인모달창](https://user-images.githubusercontent.com/73421820/103096517-83a23480-4647-11eb-8bc4-6f64e56cd587.PNG)

<br><br>

### 로그인 실패
![로그인 실패](https://user-images.githubusercontent.com/73421820/103096519-84d36180-4647-11eb-9316-555aa3b81bfc.PNG)
<br><br>

### 로그인 후 화면
![로그인 후 화면](https://user-images.githubusercontent.com/73421820/103096522-8735bb80-4647-11eb-975c-8c16727b4e24.PNG)
<br><br>

## 배포시 시작 프로그램 지정

web.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>wsp</display-name>
  
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
  
</web-app> 
```
<br>

index.jsp
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
  <style>
      .bg-image-full {
        background: no-repeat center center scroll;
        -webkit-background-size: cover;
        -moz-background-size: cover;
        background-size: cover;
        -o-background-size: cover;
      }

      div.bg-image-full{
        height: 300px;
        text-align: center;
      }

      @font-face { font-family: 'GmarketSansBold'; src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_2001@1.1/GmarketSansBold.woff') format('woff'); font-weight: normal; font-style: normal; }
      div.bg-image-full>h1{
        color : white;
        position: relative;
        top : 75px;
        font-size: 3em;
        font-family: 'GmarketSansBold';
        text-shadow: -2px 0 black, 0 2px black, 2px 0 black, 0 -2px black;
      }
  </style>
</head>
<body>
	
	<!-- WEB-INF/view/common/header.jsp 삽입 
		 동적 Include
	-->
	<jsp:include page="WEB-INF/views/common/header.jsp"></jsp:include>
	
	
	<!-- 메인 화면 이미지 -->
	<div class="py-5 bg-image-full" style="background-image: url('https://iei.or.kr/resources/images/intro/intro_bg.jpg');">
	    <h1>Servlet/JSP<br>Web Application</h1>
	</div>
	
	<!-- 내용 작성 부분 -->
	<div class="py-5">
	  <div class="container">
	    <h1>Section Heading</h1>
	    <p class="lead">Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
	    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Aliquid, suscipit, rerum quos facilis repellat architecto commodi officia atque nemo facere eum non illo voluptatem quae delectus odit vel itaque amet.</p>
	  </div>
	</div>
	
	<div class="py-5">
	  <div class="container">
	    <h1>Section Heading</h1>
	    <p class="lead">Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
	    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Aliquid, suscipit, rerum quos facilis repellat architecto commodi officia atque nemo facere eum non illo voluptatem quae delectus odit vel itaque amet.</p>
	  </div>
	</div>
	
	<!-- WEB-INF/view/common/footer.jsp 삽입 -->
	<jsp:include page="WEB-INF/views/common/footer.jsp"></jsp:include>
	
</body>
</html>
```

<br>

## header 와 footer 연결

header
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
<title>WebServer Project</title>

<!-- Bootstrap core CSS -->
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css">

<!-- Bootstrap core JS -->
<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ho+j7jyWK8fNQe+A12Hb8AhRq26LrZ/JpcUGGOn+Y7RsweNrtN/tE3MoK7ZeZDyx" crossorigin="anonymous"></script>

<!-- sweetalert : alert창을 꾸밀 수 있게 해주는 라이브러리 https://sweetalert.js.org/ -->
<script src="https://unpkg.com/sweetalert/dist/sweetalert.min.js"></script>

<style>
body {
	padding-top: 56px;
}
</style>

</head>

<body>
	<%-- 
		프로젝트의 시작 주소 (context root)를 얻어와 간단하게 사용할 수 있도록
		별도의 변수를 생성
	--%>
	<c:set var="contextPath" scope="application" value="${pageContext.servletContext.contextPath }"/>
```
### 로그인 실패시 경고창 출력
```html
	<%-- 로그인 실패 등의 서버로 부터 전달 받은 메세지를 경고창으로 출력하기 
		
		1) 서버로 부터 전달받은 매세지가 있는지 검사
	--%>
	<c:if test="${!empty sessionScope.swalTitle }">
		<script>
			swal({icon : "${swalIcon}" , title:"${swalTitle}",
				  text : "${swalText}"});
		</script>
		
		<%-- 2)한번 출력한 메세지를 Session에서 삭제 --%>
		<c:remove var="swalIcon" />
		<c:remove var="swalTitle" />
		<c:remove var="swalText" />
	</c:if>
```

### 로고 클릭시 메인화면으로 이동 / 로그인 시 네비 바에 이름 출력과 로그아웃으로 변경
```html
	<!-- Navigation으로 된 header -->
	<div class="header navbar navbar-expand-lg navbar-dark bg-dark fixed-top">
		<div class="container">
			<a class="navbar-brand" href="${contextPath}">WebServer Project</a>
			<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarResponsive" aria-controls="navbarResponsive" aria-expanded="false" aria-label="Toggle navigation">
				<span class="navbar-toggler-icon"></span>
			</button>
			<div class="collapse navbar-collapse" id="navbarResponsive">
				<ul class="navbar-nav ml-auto">
					<li class="nav-item"><a class="nav-link" href="#">Notice</a></li>
					<li class="nav-item"><a class="nav-link" href="#">Board</a></li>
					
					<c:choose>
                  <%-- 로그인이 되어있지 않을 때 == session에 loginMember라는 값이 없을 때 --%>
                  <c:when test="${empty sessionScope.loginMember}">
                  
                     <!-- 헤더에 있는 login 버튼 클릭 시 #modal-container-1 이라는 아이디를 가진 요소를 보여지게 함. -->
                     <li class="nav-item active">
                        <a class="nav-link" data-toggle="modal" href="#modal-container-1">
                           Login
                        </a>
                     </li>
                  </c:when>
                  
                  <%-- 로그인이 되어 있을 때 --%>
                  <c:otherwise>
                     <li class="nav-item active">
                        <%-- 로그인 회원의 이름을 가져와 출력 --%>
                        <a class="nav-link" href="#">${loginMember.memberName}</a>
                     </li>
                     <li class="nav-item active">
                        <a class="nav-link" href="${contextPath}/member/logout.do">Logout</a>
                     </li>
                  </c:otherwise>
                  
               </c:choose>

				</ul>
			</div>
		</div>
	</div>
```
### 로그인 모달 창
```html
	<%-- Modal창에 해당하는 html 코드는 페이지 마지막에 작성하는게 좋다 --%>
	<div class="modal fade" id="modal-container-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
		<div class="modal-dialog" role="document">

			<div class="modal-content">

				<div class="modal-header">
					<h5 class="modal-title" id="myModalLabel">로그인 모달창</h5>
					<button type="button" class="close" data-dismiss="modal">
						<span aria-hidden="true">×</span>
					</button>
				</div>

				<div class="modal-body">
					<form class="form-signin" method="POST" action="${contextPath}/member/login.do">


						<input type="text" class="form-control" id="memberId" name="memberId" placeholder="아이디" value="">
						<br>
						<input type="password" class="form-control" id="memberPwd" name="memberPwd" placeholder="비밀번호">
						<br>

						<div class="checkbox mb-3">
							<label> 
								<input type="checkbox" name="save" id="save"> 아이디 저장
							</label>
						</div>

						<button class="btn btn-lg btn-primary btn-block" type="submit">로그인</button>
						<a class="btn btn-lg btn-secondary btn-block" href="#">회원가입</a>
					</form>
				</div>
				<div class="modal-footer">
					<button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
				</div>
			</div>
		</div>
	</div>

</body>

</html>
```
<br>

footer
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
</head>
<body>
	<!-- Footer -->
	<div class="py-5 bg-dark footer">
	  <div class="container">
	    <p class="m-0 text-center text-white">Copyright &copy; KH Information Educational Institute A-Class</p>
	  </div>
	</div>
</body>
</html>
```

<br>

## Template생성
```java
package com.kh.wsp.common;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.naming.NamingException;
import javax.sql.DataSource;

public class JDBCTemplate {
	// 1. 반복되는 Connection 객체의 생성을 간소화
	// 2. 트랜잭션 처리, close()처리의 간소화

	
	// 모든 필드, 메소드를 static으로 선언하여
	// 프로그램 구동 시 static 메모리 영역에
	// 모든 클래스 내용을 로드하여 하나의 객체 모양을 띄게함.
	
	// 하나의 공용 커넥션 참조 변수 선언
	private static Connection conn = null;
	// private 선언 이유 : 직접 접근 시 null 값이나
	// 닫혀진 커넥션 객체를 가져갈 수 있는 확률이 있어서 이를 미연에 차단.
	
	// 해당 클래스의 내용은 모두 static에서 객체의 모양을 이루기 때문에
	// 추가적인 객체 생성을 막기위해 private 사용
	private JDBCTemplate() { }
	
	
	public static Connection getConnection() throws NamingException, SQLException {
		
		/* 이전 JDBC 수업때는 요청 시 마다 Connection을 새로 만들어 반환하는 과정을 거쳤다.
		 * --> 요청 시간이 지연됨. (비효율적)
		 * 
		 * WAS(Web Application Server, Tomcat(== servlet Container)) 가
		 * 미리 DB에 접속할 수 있는 객체(Connection)를 일정 개수를 만들어 두고
		 * 요청이 올 때 마다 만들어 둔 객체를 전달하고 사용 완료 후 반환받는
		 * Connection Pool을 사용함.
		 * */
		
	      // JNDI(Java Naming and Directory Interface API)
	      /*디렉터리 서비스에 접근하는데 사용하는 API
	      어플리케이션은 JNDI를 사용하여 서버의 resource를 찾는다.
	      특히 JDBC resource를 data source라고 부른다.
	      
	      Resource를 서버에 등록할 때 고유한 JNDI 이름을 붙이는데, JNDI 이름은 디렉터리 경로 형태를 가진다.
	      예를 들어 data source의 JNDI 이름은 'jdbc/mydb' 형식으로 짓는다.
	      
	       서버에서 'jdbc/oracle'라는 DataSource를 찾으려면 
	      'java:comp/env/jdbc/oracle'라는 JNDI 이름으로 찾아야 한다. 
	      즉 lookup() 메소드에 'java:comp/env/jdbc/oracle'를 인자값으로 넘긴다.
	      
	      */
	      
	      // Servers에 존재하는 context.xml 파일을 찾는 작업
	      Context initContext = new InitialContext();
	      Context envContext  = (Context)initContext.lookup("java:/comp/env");  // java:comp/env   응용 프로그램 환경 항목
	      
	      // context.xml 파일에서 name이 "jdbc/oracle"인 DataSource를 얻어옴
	      // DataSource : DriverManager를 대체하는 객체로 
	      // Connection 생성, Connectoin pool을 구현하는 객체
	      DataSource ds = (DataSource)envContext.lookup("jdbc/oracle");

	      conn = ds.getConnection(); // DataSource에 의해 미리 만들어진 Connection 중 하나를 얻어옴.
	      
	      conn.setAutoCommit(false);
		
		
		return conn;
	}
	
	
	// 트랜잭션 처리(commit, rollback)도 공통적으로 사용함
	// static으로 미리 선언 코드길이 감소 효과 + 재사용성
	public static void commit(Connection conn) {
		try {
			if(conn != null && !conn.isClosed()) {
				// 참조하고 있는 커넥션이 닫혀있지 않은 경우
				conn.commit();
			}
		}catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void rollback(Connection conn) {
		try {
			if(conn != null && !conn.isClosed()) {
				// 참조하고 있는 커넥션이 닫혀있지 않은 경우
				conn.rollback();
			}
		}catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	// DB 연결 자원 반환 구문도 static으로 작성
	public static void close(Connection conn) {
		try {
			if(conn != null && !conn.isClosed()) {
				// 참조하고 있는 커넥션이 닫혀있지 않은 경우
				conn.close();
			}
		}catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void close(ResultSet rset) {
		try {
			if(rset != null && !rset.isClosed()) {
				rset.close();
			}
		}catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	// Statement, PreparedStatement 두 객체를 반환하는 메소드
	// 어떻게? PreparedStatement는 Statement의 자식이므로
	// 매개변수 stmt에 다형성이 적용되서 자식 객체인 PreparedStatement를 참조 가능
	public static void close(Statement stmt) {
		try {
			if(stmt != null && !stmt.isClosed()) {
				stmt.close();
			}
		}catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```
<br>

### JDBC resource
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>wsp</display-name>
  
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
  
  
  <resource-ref>
    <description>Oracle Datasource</description>
    <res-ref-name>jdbc/oracle</res-ref-name>
    <res-type>javax.sql.DataSource</res-type>
    <res-auth>Container</res-auth>
  </resource-ref>
  
</web-app>
```
<br>

### Member.vo
```java
package com.kh.wsp.member.model.vo;

import java.sql.Date;

public class Member {
	
	private int memberNo;	
	private String memberId;
	private String memberPwd;
	private String memberName;
	private String memberPhone;
	private String memberEmail;
	private String memberAddress;
	private String memberInterest; // 관심사
	private Date memberEnrollDate; // 가입일
	private String memberStatus;   // 상태
	private String memberGrade;	   // 등급

	
	// 기본 생성자
	public Member() {}
	

	public Member(int memberNo, String memberId, String memberName, String memberPhone, String memberEmail,
			String memberAddress, String memberInterest, String memberGrade) {
		super();
		this.memberNo = memberNo;
		this.memberId = memberId;
		this.memberName = memberName;
		this.memberPhone = memberPhone;
		this.memberEmail = memberEmail;
		this.memberAddress = memberAddress;
		this.memberInterest = memberInterest;
		this.memberGrade = memberGrade;
	}




	public Member(int memberNo, String memberId, String memberPwd, String memberName, String memberPhone,
			String memberEmail, String memberAddress, String memberInterest, Date memberEnrollDate, String memberStatus,
			String memberGrade) {
		super();
		this.memberNo = memberNo;
		this.memberId = memberId;
		this.memberPwd = memberPwd;
		this.memberName = memberName;
		this.memberPhone = memberPhone;
		this.memberEmail = memberEmail;
		this.memberAddress = memberAddress;
		this.memberInterest = memberInterest;
		this.memberEnrollDate = memberEnrollDate;
		this.memberStatus = memberStatus;
		this.memberGrade = memberGrade;
	}

	// getter,setter
	public int getMemberNo() {	return memberNo; }

	public void setMemberNo(int memberNo) {	this.memberNo = memberNo;	}

	public String getMemberId() {	return memberId;	}

	public void setMemberId(String memberId) {	this.memberId = memberId;	}

	public String getMemberPwd() {	return memberPwd;	}

	public void setMemberPwd(String memberPwd) {	this.memberPwd = memberPwd;	}

	public String getMemberName() {	return memberName;	}

	public void setMemberName(String memberName) {	this.memberName = memberName;	}

	public String getMemberPhone() {	return memberPhone;	}

	public void setMemberPhone(String memberPhone) {	this.memberPhone = memberPhone;	}

	public String getMemberEmail() {	return memberEmail;	}

	public void setMemberEmail(String memberEmail) {	this.memberEmail = memberEmail;	}

	public String getMemberAddress() {	return memberAddress;	}

	public void setMemberAddress(String memberAddress) {	this.memberAddress = memberAddress;	}

	public String getMemberInterest() {	return memberInterest;	}

	public void setMemberInterest(String memberInterest) {	this.memberInterest = memberInterest;	}

	public Date getMemberEnrollDate() {	return memberEnrollDate;	}

	public void setMemberEnrollDate(Date memberEnrollDate) {	this.memberEnrollDate = memberEnrollDate;	}

	public String getMemberStatus() {	return memberStatus;	}

	public void setMemberStatus(String memberStatus) {	this.memberStatus = memberStatus;	}

	public String getMemberGrade() {	return memberGrade;	}

	public void setMemberGrade(String memberGrade) {	this.memberGrade = memberGrade;	}


	@Override
	public String toString() {
		return "Member [memberNo=" + memberNo + ", memberId=" + memberId + ", memberPwd=" + memberPwd + ", memberName="
				+ memberName + ", memberPhone=" + memberPhone + ", memberEmail=" + memberEmail + ", memberAddress="
				+ memberAddress + ", memberInterest=" + memberInterest + ", memberEnrollDate=" + memberEnrollDate
				+ ", memberStatus=" + memberStatus + ", memberGrade=" + memberGrade + "]";
	}
	
}
```
<br>


### 로그인용 Servlet
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

// @WebServlet에 작성되는 요청 주소에서 
// 제일 앞에 있는 '/'는 contextPath를 의미한다.
@WebServlet("/member/login.do")
public class LoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
// 	  기본 생성자 없어도 자동으로 작성 됨.    
//    public LoginServlet() {
//        super();
//    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		// 1. POST 방식으로 전달된 데이터의 문자 인코딩 변경
		request.setCharacterEncoding("UTF-8");
	
		// 2. 파라미터를 얻어와 변수에 저장
		String memberId = request.getParameter("memberId");
		String memberPwd = request.getParameter("memberPwd");
		String save = request.getParameter("save");
		// 체크시 on, 아니면 null
		
//		System.out.println(memberId + "/" + memberPwd + "/" + save);
		
		// JDBC 수행
		// 아이디와 비밀번호를 하나의 vo에 담아서 service로 전달
		// 3. 아이디와 비밀번호를 Member 객체에 세팅
		Member member = new Member();
		member.setMemberId(memberId);
		member.setMemberPwd(memberPwd);

		// * try/catch를 이용하여 service, dao에서 발생한 예외를 처리한다.
		// -> 응답 화면을 에러 페이지로 연결
		
		try {
			
			// 4. Member객체를 Service로 전달하여 결과를 반환받기
			// (로그인 이란?  id/pwd 가 일치하는 회원 정보를 DB에서 조회해 오는 것)
			
			Member loginMember = new MemberService().loginMember(member);
			
//			System.out.println(loginMember);
			
			// 로그인 정보는 로그아웃 또는 브라우저가 종료될 때 까지 유지되어야 한다.
			// --> Session을 활용함
			
			
			// 5. 응답 화면 문서 타입 지정
			response.setContentType("text/html; charset=UFT-8");
			
			// 6. Session 객체를 얻어와 로그인 정보를 추가함
			HttpSession session = request.getSession();
			
			// 6-1. 로그인이 성공 했을때만 Session에 로그인 정보 추가하기
			if(loginMember !=null) {
				
				// 6-2. 30분 동안 동작이 없을 경우 Session을 만료 시킴
//				session.setMaxInactiveInterval(60*30);
				session.setMaxInactiveInterval(60*1); //테스트 용 1분 후 만료
				
				// 6_3. Session에 로그인 정보 추가
				session.setAttribute("loginMember", loginMember);
				
			}  else {
	            // 7. 로그인이 실패했을 때 "아이디 또는 비밀번호를 확인해주세요."라고 경고창 띄우기
	            // -> Session에 "아이디 또는 비밀번호를 확인해주세요."라는 문자열을 담아서 redirect하기
	            
	            // session.setAttribute("text", "아이디 또는 비밀번호를 확인해주세요.");
	           
				// sweet alert 사용하기
				session.setAttribute("swalIcon", "error");
				session.setAttribute("swalTitle", "로그인 실패");
				session.setAttribute("swalText", "아이디 또는 비밀번호를 확인해주세요.");
	         }
			
			
			// redirect 방식을 이용하여 로그인을 요청했던 페이지로 이동
			
			/*	forward 같은 경우에는 이동하는 페이지로
			 * request, response 객체를 그대로 위임하고,
			 * 주소를 위임전 주소로 유지하지만,
			 * 
			 * redirect는 이전 request, response를 폐기하고
			 * 새롭게 만들어서 지정된 주소로 새로운 요청을 보냄.
			 * ->새롭게 요청을 보내기 때문에 이전 요청 주소가 아닌
			 * 	 새로운 요청 주소가 주소창 나타남
			 * 
			 * */
			
			// request.getHeader("referer") : 요청 전 페이지 주소가 담겨있음.
			 response.sendRedirect(request.getHeader("referer"));
		
		}catch(Exception e) {
			e.printStackTrace();
			
			// 로그인 과정에서 오류 발생 시 errorPage.jsp로  forward 진행
			
			request.setAttribute("errorMsg", "로그인 과정에서 오류가 발생했습니다.");
			
			// 요청 위임 객체 생성
			RequestDispatcher view = request.getRequestDispatcher("/WEB-INF/views/common/errorPage.jsp");
			
			view.forward(request, response);
			// redirect는 요청 주소를 적지만 (/wsp/login.do)
			// forward는 요청할 페이지의 파일 경로를 적음.
			
		}
		
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}
```
<br>

### Service
```java
package com.kh.wsp.member.model.service;

import static com.kh.wsp.common.JDBCTemplate.*;

import java.sql.Connection;

import com.kh.wsp.member.model.dao.MemberDAO;
import com.kh.wsp.member.model.vo.Member;

public class MemberService {
	private MemberDAO dao = new MemberDAO();
	
	/** 로그인 Service
	 * @param member
	 * @return loginMember
	 * @throws Exception
	 */
	public Member loginMember(Member member) throws Exception {
		// 1) Connection 얻어오기
		Connection conn = getConnection();
		
		// 2) DAO 메소드를 수행하여 결과 반환받기
		Member loginMember = dao.loginMember(conn, member);
		
		// 3) Connection 반환하기
		
		close(conn);
		
		// 4) DAO 수행 결과를 Controller 반환
				
		return loginMember;
	}

}
```
<br>

### DAO
```java
package com.kh.wsp.member.model.dao;

import static com.kh.wsp.common.JDBCTemplate.*;

import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.Properties;

import com.kh.wsp.member.model.vo.Member;

public class MemberDAO {
	
	// DAO에서 자주 사용되는 JDBC 참조 변수 선언
	private Statement stmt = null;
	private PreparedStatement pstmt = null;
	private ResultSet rset = null;
	private Properties prop = null;
	
	public MemberDAO() {
		// 외부에 저장된 XML 파일로 부터 SQL을 얻어옴 --> 유지보수성 향상
		
		try {	
			String filePath = 
					MemberDAO.class.getResource("/com/kh/wsp/sql/member/member-query.xml").getPath();
			
			prop = new Properties();
			
			prop.loadFromXML(new FileInputStream(filePath));
		}catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	/** 로그인 DAO
	 * @param conn
	 * @param member
	 * @return	loginMember
	 * @throws Exception
	 */
	public Member loginMember(Connection conn, Member member) throws Exception {
		
		// 결과 저장용 변수 선언.
		Member loginMember = null;
		
		String query = prop.getProperty("loginMember");
		
		try {
			// 1) PreparedStatement 객체를 얻어와 query 세팅
			pstmt = conn.prepareStatement(query);
			
			// 2) 위치홀더(?)에 알맞은 값 세팅
			pstmt.setString(1, member.getMemberId());
			pstmt.setString(2, member.getMemberPwd());
			
			// 3) SQL 수행 후 결과를 반환받아 저장
			rset = pstmt.executeQuery();
			
			// 4) 조회 결과가 있을 경우 Member 객체에 조회 내용을 저장
			if(rset.next()) {
				loginMember = new Member(rset.getInt("MEMBER_NO"), 
										rset.getString("MEMBER_ID"), 
										rset.getString("MEMBER_NM"), 
										rset.getString("MEMBER_PHONE"), 
										rset.getString("MEMBER_EMAIL"), 
										rset.getString("MEMBER_ADDR"), 
										rset.getString("MEMBER_INTEREST"), 
										rset.getString("MEMBER_GRADE"));
			}
		}finally {
			// 사용한 DB 자원 반환
			close(rset);
			close(pstmt);
		}
		return loginMember;
	}

}
```
<br>

### 로그인용 sql

```sql
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>

<entry key="loginMember">
SELECT MEMBER_NO, MEMBER_ID, MEMBER_NM, MEMBER_PHONE,
       MEMBER_EMAIL, MEMBER_ADDR, MEMBER_INTEREST, MEMBER_GRADE
FROM MEMBER
WHERE MEMBER_ID = ? 
AND MEMBER_PWD = ?
AND MEMBER_STATUS = 'Y'
</entry>

</properties>
```
<br>

### 로그아웃용 Servlet
```java
package com.kh.wsp.member.controller;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/member/logout.do")
public class LogoutServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		// 로그인 상태 -> DB에서 조회한 회원 정보가 Session에 존재하는 것
		// 로그아웃 상태 -> Session에 회원 정보가 없는 것
		
		// 세션 만료(세션 무효화)
		request.getSession().invalidate();
		
		// 로그 아웃 후 메인 or 로그아웃을 수행한 페이지로 리다이렉트 
		// response.sendRedirect(request.getContextPath()); // 메인
		response.sendRedirect(request.getHeader("referer")); // 로그아웃을 수행한 페이지
	
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}
```

