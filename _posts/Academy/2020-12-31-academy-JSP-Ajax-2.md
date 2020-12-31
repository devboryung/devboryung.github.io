---
title: "2020년 12월 31일"
excerpt: "Ajax (jQuery방식-JSON,GSON)"
search: true
categories: 
  - Academy
tags: 
  - Ajax
  - JavaScript
  - JQuery
toc: true
---

# jQuery 방식의 Ajax
<br>

## POST 방식
버튼 클릭 시 POST 방식으로 서버에 데이터 전송 및 응답
<br>

### 출력 화면
![post버튼](https://user-images.githubusercontent.com/73421820/103411145-d634a100-4bb1-11eb-9719-149db95f6e14.png)

<br><br>

index.jsp
```html
<div class="list-group-item">
    <h5 class="list-group-item-heading">
        <span class="text-warning">2. 버튼 클릭 시 POST 방식으로 서버에 데이터 전송 및 응답</span><br><br>
        입력 : <input type="text" id="input2">
        <button id="jqBtn2" class="btn btn-warning float-right">POST 방식 전송</button>
    </h5>
</div>
```
<br>

jQuery 방식의 Ajax를 사용하려면 $.ajax({}); 모양으로 준비<br>
자바스크립트에서 {} 는 객체를 나타냄, 안에 key:value 로 작성

jQueryScript.js

```js
$("#jqBtn2").on("click",function(){
    $.ajax({
        // 필수 속성인 url :  어떤 주소로 요청을 보낼것인가?
        url : "jqTest2.do", // 요청주소
        data : {"input" : $("#input2").val()}, // 요청 시 전달할 값, 객체형태여서 값 여러개 보낼 수 있다.
        // 아이디가 input2인 요소가 가지고 있는 값을 input이라는 이름으로 전달 하겠다
        type : "post", // 데이터 전달 방식(GET/POST) , 대소문자 가리지 않음
        
        // dataType : 응답 데이터의 형식을 지정해주는 속성.
        // -->미작성 시 어느정도 자동으로 판별하여 지정됨.
        // text,number 등은 자동으로 지정되지만 json은 자동으로 판별하지 못 함
        dataType : "text",
        success : function(result){ // 통신 성공 시
                            // result : 응답 데이터
            $("#result-area").html(result);
        },
        error : function(){
            console.log("ajax 통신 실패");
        },
    }); 
});
```
<br>


JqueryAjaxServlet2<br>
jqTest2.do 요청을 받는 서블릿

```java
package com.kh.ajax.jquery.controller;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/jqTest2.do")
public class JqueryAjaxServlet2 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		// POST 방식 전송 시 문자 인코딩 변경
		request.setCharacterEncoding("UTF-8");
		
		// 입력받은 파라미터 얻어오기
		String input = request.getParameter("input"); // ajax  data에 작성한 키 값
		System.out.println("입력값 : " + input);
		
		// 응답 문자 인코딩 지정
		response.setCharacterEncoding("UTF-8");
		
		// 스트림 연결
		PrintWriter out = response.getWriter();
		
		for(int i=0; i<input.length(); i++) {
			out.append(input.charAt(i) + "<br>");
		}
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		doGet(request, response);
	}

}
```
<br>
<br><br>

## 아이디 유효성 검사
<br>

### 출력 화면
![아이디중복검사](https://user-images.githubusercontent.com/73421820/103411208-1ac03c80-4bb2-11eb-884e-83c96866e97a.gif)

<br><br> 

wsp_member.js
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
        // ajax를 이용한 실시간 아이디 중복 검사
        $.ajax({
            url :  "idDupCheck.do", // 상대 경로
            data : {"id" : value },
            type : "post",
            success : function(result){
                if(result == 0){ // 중복되지 않은 경우
                    $("#checkId").text("사용 가능한 아이디입니다.").css("color","green");
                    validateCheck.id = true;
                }else{
                    $("#checkId").text("이미 사용중인 아이디입니다.").css("color","red");
                    validateCheck.id = false;
                }
            },
            error : function(){
                console.log("아이디 중복검사 실패");
            }
        });
    }
});
```
<br>

IdDupCheckservlet.java

```java
package com.kh.wsp.member.controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.kh.wsp.member.model.service.MemberService;

@WebServlet("/member/idDupCheck.do")
public class IdDupCheckServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// post 형식으로 데이터를 전달받지만 ID에는 한글이 포함되지 않아
		// 인코딩을 변경하지 않아도 글자가 깨지지 않음.
		
		String id = request.getParameter("id");
		
		try {
			// 1) 비즈니스 로직을 호출하여 결과 반환 받기
			int result = new MemberService().inDupCheck(id);
			// Ajax로 중복 검사 시 사용
			// 스트림을 얻어와서  얻어온 result를 바로 전달함.
			response.getWriter().print(result);
			
		}catch(Exception e) {
			e.printStackTrace();
			String path = "/WEB-INF/views/common/errorPage.jsp";
			request.setAttribute("errorMsg", "아이디 중복 검사 과정에서 오류 발생.");
			
			RequestDispatcher view = request.getRequestDispatcher(path);
			view.forward(request, response);
		}
	}
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}
}

```

<br><br><br>

## JSON
<br>

### JSON이란?
- JavaScript Object Notation (자바스크립트 객체 표현법)
- 간단한 포맷<br>
  * 괄호 {} 내에 key : value 쌍으로 구성 -> { “key” : value } <br>
  * key -> 반드시 문자열 사용 (쌍 따옴표(“”) 표기 필수)<br>
  * value -> String, Number, Boolean, Array, Object, null 데이터 저장 가능(단, char 데이터는 저장 불가) <br>
- 객체{} 또는 배열[] 데이터를 효율적으로 표시 가능
<br>
자바스크립트 객체  {K:V}  바깥에 " " 붙여주면
자바스크립트 객체의 문자열이 됨  "{K:V}"    --> JSON <br>
JSON 스트링으로 이루어진 표기 법<br>
json 모양으로 넘어온것(String) 을 라이버리를 통해  객체(Object)로 만든다.<br>
*덩어리가 큰 object 를 String으로 압축해서 넘어온 것
객체 교환을 할 때 용이하다. <br>

### JSON 특징
-  Ajax 통신에서 Object 타입의 데이터 전송 시 XML 대비 용량이 작고 속도가 빠름 -> 경량 데이터 교환 방식
- 간단한 포맷을 가지고 있어 이해하기 쉬움
- 순수 TEXT 기반 <br>
  * 구조화된 TEXT 형식<br>
  * 대부분의 프로그래밍 언어에서 JSON 포맷 데이터를 핸들링 할 수 있는 라이브러리를 제공 -> 시스템간 객체 교환에 용이
<br><br>

### JSON 라이브러리 추가
 
1. [제이슨 라이브러리](https://code.google.com/archive/p/json-simple/downloads)

2. 다운로드 클릭<br>
![json 1 1 1](https://user-images.githubusercontent.com/73421820/103411530-dc2b8180-4bb3-11eb-9584-48b3f1006c55.png)

3. lib폴더에 추가<br>
![다운받은라이브러리lib에추가](https://user-images.githubusercontent.com/73421820/103411531-dcc41800-4bb3-11eb-9612-eaa7dffa3572.png) 
 <br><br>


## JSON 방식

### 서버로 데이터 전송, 응답받기
[JSON]서버로 기본형 데이터 전송 후, 응답을 객체(Obejct)로 받기
<br>

### 출력화면
![json](https://user-images.githubusercontent.com/73421820/103411836-7213dc00-4bb5-11eb-9cc7-5995ea71d1f8.gif)
<br><br>

jQuerySript.js

```js
$("#jqBtn3").on("click",function(){
$.ajax({
    url : "jqTest3.do",
    data : {"input" : $("#input3").val()},
    dataType: "json",
    success : function(user){ // user (json형태의 문자열)
        // 응답되는 데이터가 JSON형태임을 인식시키는 방법2
        // user = JSON.parse(user);
        console.log(user);

        var result = "";

        if(user!=null){ // 서버로부터 전달된 데이터가 있다면
            result = "번호 : " + user.no + "<br>"
                    + "이름 : " +  user.name + "<br>"
                    + "나이 : " +  user.age + "<br>"
                    + "성별 : " +  user.gender ;
        }
        else{
            result = "일치하는 사용자가 없습니다.";
        }
        $("#result-area").html(result);

    },
    error : function(){
        console.log("ajax 통신 실패");
    }
});
```
<br>

JqueryAjaxServlet3.java
jqTest3.do 요청을 받을 수 있는 서블릿

```java
package com.kh.ajax.jquery.controller;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.json.simple.JSONObject;

import com.kh.ajax.jquery.model.vo.User;

@WebServlet("/jqTest3.do")
public class JqueryAjaxServlet3 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		
		// 샘플데이터 생성
		List<User> list = new ArrayList<User>();
		list.add(new User(1,"박철수", 30, '남'));
	    list.add(new User(2,"김영희", 26, '여'));
	    list.add(new User(3,"오영심", 32, '여'));
	    list.add(new User(4,"이민기", 28, '남'));
	    list.add(new User(5,"홍길동", 33, '남'));
	    
	    // 파라미터 전달 받기
	    // 파라미터로 가져온 자료 자료형은 String
	    int input = Integer.parseInt(request.getParameter("input"));
	    
	    // JSON 라이브러리를 이용한 JSON 객체 생성하기
	    // https://code.google.com/archive/p/json-simple/downloads (라이브러리 다운로드)
	    
	    // JSONObject : 데이터를 K:V 형태로 저장하고
	    // 출력 시 JSON으로 내보내 줄 수 있는 객체
	    
	    JSONObject jsonUser = null;
	    
	    
	    // input과 회원 번호가 일치하는 회원 찾기
	    for(int i=0; i<list.size(); i++) {
	    	if(list.get(i).getNo() == input) {
	    		
	    		jsonUser = new JSONObject(); // JSONObject 객체 생성
	    		
	    		jsonUser.put("no", list.get(i).getNo());
	    		jsonUser.put("name", list.get(i).getName());
	    		jsonUser.put("age", list.get(i).getAge());
	    		jsonUser.put("gender", list.get(i).getGender()+"");
	    		// JavaScript에는 char자료형이 없음.-> String으로 변환
	    		
	    		break; // 검색 중지
	    	}
	    }
	    
	    // JSONObject에 저장된 내용 확인
	    System.out.println(jsonUser.toJSONString());
	    
	    response.setCharacterEncoding("UTF-8"); // 문자 인코딩 지정
	    
	    // 응답 데이터가 JSON 형태임을 인식 시키는 방법 1
	    // MIME TYPE 지정
	   // response.setContentType("application/json; charset=UTF-8");
	    
	    // 응답용 스트림 연결 및 출력
	    response.getWriter().print(jsonUser.toJSONString()); // -> JSON으로 인식 안 되고 문자열로 인식 된다.
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		doGet(request, response);
	}
}
```
<br>

User.vo

```java
package com.kh.ajax.jquery.model.vo;

public class User {
	private int no;
	private String name;
	private int age;
	private char gender;
	
	public User() {}

	public User(int no, String name, int age, char gender) {
		super();
		this.no = no;
		this.name = name;
		this.age = age;
		this.gender = gender;
	}

	public int getNo() {
		return no;
	}

	public void setNo(int no) {
		this.no = no;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public char getGender() {
		return gender;
	}

	public void setGender(char gender) {
		this.gender = gender;
	}

	@Override
	public String toString() {
		return "User [no=" + no + ", name=" + name + ", age=" + age + ", gender=" + gender + "]";
	}
}
```
<br><br>

### 실시간 회원 정보 조회
<br>

### 출력화면
![회원정보](https://user-images.githubusercontent.com/73421820/103411939-37f70a00-4bb6-11eb-8ab6-97f3ce89a9f5.gif)
<br><br>


jQueryScript.js

```js
$("#selectMemberBtn").on("click", function(){
   
    $.ajax({
       url : "member/selectMember.do",
       data : {"inputId" : $("#inputId2").val()},
       type : "get", 
       dataType : "json",
       success : function(member){
          console.log(member);

          if(member != null){
            var result = "";
            result += "<h4>" + member.memberId + "</h4>";
            result += "이름 : " + member.memberName + "<br>";
            result += "이메일 : " + member.memberEmail + "<br>";
            result += "관심분야 : " + member.memberInterest + "<br>";
            result += "가입일 : " + member.memberEnrollDate + "<br>";
            
            $("#result-area").html(result);
          }
       },
       error : function(){
          console.log("회원 정보 조회 중 문제 발생");
       }
    });
 });
```
<br>

SelectMemberServlet.java<br>
member/selectMember.do 요청을 받을수있는 servlet

```java
package com.kh.ajax.jquery.controller;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.json.simple.JSONObject;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.kh.ajax.jquery.model.service.MemberService;
import com.kh.ajax.jquery.model.vo.Member;

@WebServlet("/member/selectMember.do")
public class SelectMemberServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		
		// 파라미터 얻어오기 (입력된 ID)
		String inputId = request.getParameter("inputId");
		
		try {
			// 비즈니스 로직(회원정보 조회) 수행 후 결과 반환 받기
			Member member = new MemberService().selectMember(inputId);
			
			// 조회 성공 시
			if(member!=null) {
				// JSONObject를 이용하여 member 데이터를 json으로 변경
//				JSONObject jsonMember = new JSONObject();
//				
				jsonMember.put("memberId", member.getMemberId());
				jsonMember.put("memberName", member.getMemberName());
				jsonMember.put("memberEmail", member.getMemberEmail());
				jsonMember.put("memberInterest", member.getMemberInterest());
				jsonMember.put("memberEnrollDate", member.getMemberEnrollDate().toString());
				// JSON에서는 charm, Date, Timestamp를 사용할 수 없다.
				
				// JSON형식으로 내보내기
				response.setCharacterEncoding("UTF-8");
				response.getWriter().print(jsonMember.toJSONString());
				
			
			}
		}catch (Exception e) {
			e.printStackTrace();
		}
	}
	protected void doPost(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		doGet(request, response);
	}
}
```
<br>

Member.vo

```java
package com.kh.ajax.jquery.model.vo;

import java.sql.Timestamp;

public class Member {
	
	private int memberNo;	
	private String memberId;
	private String memberPwd;
	private String memberName;
	private String memberPhone;
	private String memberEmail;
	private String memberAddress;
	private String memberInterest; // 관심사
	private Timestamp memberEnrollDate; // 가입일
	private String memberStatus;   // 상태
	private String memberGrade;	   // 등급
	
	// 기본 생성자
	public Member() {}

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
			String memberEmail, String memberAddress, String memberInterest, Timestamp memberEnrollDate,
			String memberStatus, String memberGrade) {
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

	public int getMemberNo() {
		return memberNo;
	}

	public void setMemberNo(int memberNo) {
		this.memberNo = memberNo;
	}

	public String getMemberId() {
		return memberId;
	}

	public void setMemberId(String memberId) {
		this.memberId = memberId;
	}



	public String getMemberPwd() {
		return memberPwd;
	}

	public void setMemberPwd(String memberPwd) {
		this.memberPwd = memberPwd;
	}


	public String getMemberName() {
		return memberName;
	}

	public void setMemberName(String memberName) {
		this.memberName = memberName;
	}

	public String getMemberPhone() {
		return memberPhone;
	}

	public void setMemberPhone(String memberPhone) {
		this.memberPhone = memberPhone;
	}

	public String getMemberEmail() {
		return memberEmail;
	}

	public void setMemberEmail(String memberEmail) {
		this.memberEmail = memberEmail;
	}

	public String getMemberAddress() {
		return memberAddress;
	}

	public void setMemberAddress(String memberAddress) {
		this.memberAddress = memberAddress;
	}

	public String getMemberInterest() {
		return memberInterest;
	}

	public void setMemberInterest(String memberInterest) {
		this.memberInterest = memberInterest;
	}

	public Timestamp getMemberEnrollDate() {
		return memberEnrollDate;
	}

	public void setMemberEnrollDate(Timestamp memberEnrollDate) {
		this.memberEnrollDate = memberEnrollDate;
	}

	public String getMemberStatus() {
		return memberStatus;
	}

	public void setMemberStatus(String memberStatus) {
		this.memberStatus = memberStatus;
	}

	public String getMemberGrade() {
		return memberGrade;
	}

	public void setMemberGrade(String memberGrade) {
		this.memberGrade = memberGrade;
	}

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

MemberService.java

```java
/** 회원정보 조회 Service
* @param inputId
* @return member
* @throws Exception
*/
public Member selectMember(String inputId) throws Exception {
Connection conn = getConnection();

Member member = dao.selectMember(conn,inputId);

close(conn);


return member;
}
```
<br>

MemberDAO.java

```java
/** 회원정보 조회 DAO
* @param conn
* @param inputId
* @return member
* @throws Exception
*/
public Member selectMember(Connection conn, String inputId) throws Exception{
PreparedStatement pstmt = null;
ResultSet rset = null;
Member member = null;

String query = "SELECT MEMBER_ID, MEMBER_NM, MEMBER_EMAIL, " + 
        "MEMBER_INTEREST, MEMBER_ENROLL_DATE " + 
        "FROM MEMBER " + 
        "WHERE MEMBER_ID = ? " + 
        "AND MEMBER_STATUS = 'Y'";
try {
    
    pstmt = conn.prepareStatement(query);
    pstmt.setNString(1, inputId);
    rset = pstmt.executeQuery();
    
    if(rset.next()) {
    
        member = new Member();
        member.setMemberId(rset.getString("MEMBER_ID"));
        member.setMemberName(rset.getString("MEMBER_NM"));
        member.setMemberEmail(rset.getString("MEMBER_EMAIL"));
        member.setMemberInterest(rset.getString("MEMBER_INTEREST"));
        member.setMemberEnrollDate(rset.getTimestamp("MEMBER_ENROLL_DATE"));
        
        // db에 저장되어있는 data타입 (년,월,일,시,분,초) 
        // java.sql.Date 사용시 년,월,일 만 얻어와 짐   
        // --> java. sql.Timestamp를 활용하면 년,월,일,시,분,초 모두 꺼내올 수 있음.
        
    }
}finally {
    close(rset);
    close(pstmt);
}
return member;
}
```
<br>
<br>
<br>

## GSON
<br>

### GSON이란?
- Google JSON의 약어
- Google에서 만든 오픈 라이브러리로 JSON파일을 쉽게 읽고, 만들 수 있는 메소드 제공
- toJSON(Object, Appendable)
 * 매개변수 Object를 JSON으로 변환하여 Appendable에 연결된 출력스트림으로
출력하는 메소드
 * 기존 JSON방식으로 변환하기 번거로웠던 List, Map 객체를
별도의 방법이 아닌 toJson()메소드 하나도 쉽게 JSON으로 변환 가능
 * List, Map 뿐만 아닌 모든 Object 변환 가능.
 <br><br>


### GSON 라이브러리 추가
 
1. [GSON 라이브러리](https://github.com/google/gson)
2. 다운로드 클릭<br>
![클릭](https://user-images.githubusercontent.com/73421820/103412218-60333880-4bb7-11eb-9493-0986e7bbb329.png)<br>
![GSON다운](https://user-images.githubusercontent.com/73421820/103412220-60cbcf00-4bb7-11eb-880f-5b46e3f13b7b.png)<br>
![GSON다운2](https://user-images.githubusercontent.com/73421820/103412221-61646580-4bb7-11eb-925f-7912324d42ed.png)<br>
3. lib폴더에 추가<br>
![GSON lib추가](https://user-images.githubusercontent.com/73421820/103412234-6fb28180-4bb7-11eb-8c8f-4dfb53419012.png)
<br><br>

SelectMemberServlet.java

```java
package com.kh.ajax.jquery.controller;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.json.simple.JSONObject;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.kh.ajax.jquery.model.service.MemberService;
import com.kh.ajax.jquery.model.vo.Member;

@WebServlet("/member/selectMember.do")
public class SelectMemberServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		
		// 파라미터 얻어오기 (입력된 ID)
		String inputId = request.getParameter("inputId");
		
		try {
			// 비즈니스 로직(회원정보 조회) 수행 후 결과 반환 받기
			Member member = new MemberService().selectMember(inputId);
			
			// 조회 성공 시
			if(member!=null) {
				response.setCharacterEncoding("UTF-8");
				
				// 단순히 객체를 json으로 변경하여 응답할 때
				// new Gson().toJson(member, response.getWriter());
								// json으로 변환할 객체, 출력 스트림
				
				// 날짜 데이터 포맷 지정 
	            Gson gson = new GsonBuilder().setDateFormat("yyyy-MM-dd HH:mm:ss").create();
	            gson.toJson(member, response.getWriter());
			}
			
		}catch (Exception e) {
			e.printStackTrace();
		}
	}
	protected void doPost(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		doGet(request, response);
	}

}

```
<br><br><br>


### 공지사항

### 출력화면

![출력화면](https://user-images.githubusercontent.com/73421820/103412404-4fcf8d80-4bb8-11eb-87d4-b49348f866e8.png)
<br><br>

header.jsp

```html
<li class="nav-item">
    <a class="nav-link" href="${contextPath }/notice/list.do">Notice</a>
</li>
```
<br>

noticeList.jsp
공지사항 버튼을 눌렀을 때 보여지는 화면 

```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!-- JSTL core Tag 사용  -->
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>공지사항</title>
<style>
.pagination {
	justify-content: center;
}


/* 검색창 스타일 */
#searchForm > * {
	top: 0;
	vertical-align: top;
}

select[name='searchKey']{
	width: 100px; 
	display: inline-block;
}

input[name='searchValue']{
	width: 25%; 
	display: inline-block;
}

#searchBtn{
	width: 100px; 
	display: inline-block;
}

.list-wrapper{
	height: 540px;
}


</style>

</head>
<body>
	<jsp:include page="../common/header.jsp"></jsp:include>
	<div class="container my-5 ">
	
		<h1>공지사항</h1>

		<div class="list-wrapper">
			<table class="table table-hover table-striped my-5" id="list-table">
				<thead>
					<tr>
						<th>글번호</th>
						<th>제목</th>
						<th>작성자</th>
						<th>조회수</th>
						<th>작성일</th>
					</tr>
				</thead>

				<tbody>
					<!-- 공지사항 목록 -->
					<%-- 공지사항이 존재할 때와 존재하지 않을 때에 맞는 출력 형식을 지정해야 함. --%>
					
					<c:choose>
                  <%-- 공지사항이 없을 때 --%>
                  <c:when test="${empty list}"> 
                     <tr>
                        <td colspan="5" align="center">존재하는 공지사항이 없습니다.</td>
                     </tr>
                  </c:when>
                  
                  <%-- 공지사항이 존재할 때 --%>
                  <c:otherwise>
                     <c:forEach var="notice" items="${list}">
                        <tr>
                           <td>${ notice.noticeNo }</td>
                           <td>${notice.noticeTitle}</td>
                           <td>${notice.memberId}</td>
                           <td>${notice.readCount}</td>
                           <td>${notice.noticeCreateDate}</td>
                        </tr>
                     </c:forEach>
                  </c:otherwise>
               </c:choose>
					
				</tbody>
			</table>
		</div>

		<%-- 로그인된 계정이 관리자 등급인 경우 --%>
		<button type="button" class="btn btn-primary float-right" id="insertBtn" 
		               onclick="location.href = 'insertForm.do';">글쓰기</button>


		<div class="my-5">
			<ul class="pagination">
				<li><a class="page-link" href="#">&lt;</a></li>
				<li><a class="page-link" href="#">1</a></li>
				<li><a class="page-link" href="#">2</a></li>
				<li><a class="page-link" href="#">3</a></li>
				<li><a class="page-link" href="#">4</a></li>
				<li><a class="page-link" href="#">5</a></li>
				<li><a class="page-link" href="#">&gt;</a></li>
			</ul>
		</div>

		<div class="mb-5">
			<form action="search" method="GET" class="text-center" id="searchForm">
				<select name="searchKey" class="form-control">
					<option value="title">글제목</option>
					<option value="content">내용</option>
					<option value="titcont">제목+내용</option>
				</select>
				<input type="text" name="searchValue" class="form-control">
				<button class="form-control btn btn-primary" id="searchBtn">검색</button>
			</form>
		</div>
	</div>
	<jsp:include page="../common/footer.jsp"></jsp:include>

	<script>
		// 공지사항 상세보기 기능 (jquery를 통해 작업)
		
	</script>
</body>
</html>

```
<br>

NoticeController.java
<br>
컨트롤러 생성 -> 나눠진 Servlet을 하나로 묶어서 관리함<br>
Servlet이 유지보수가 편하지만, class가 너무 많아 관리가 힘들어 진다.

```java
package com.kh.wsp.notice.controller;

import java.io.IOException;
import java.util.List;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.kh.wsp.notice.model.service.NoticeService;
import com.kh.wsp.notice.model.vo.Notice;

// 모든 notice 관련 요청을 받아들임
@WebServlet("/notice/*")
public class NoticeController extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		// Front Controller 패턴
		// - 클라이언트의 요청을 한 곳으로 집중시켜 개발하는 패턴
		// - 요청마다 Servlet을 생성하는 것이 아닌 하나의 Servlet에 작성하여 관리가 용이해짐.
		
		// Controller의 역할 : 요청에 맞는 Service를 호출, 응답을 위한 View 선택
		
		String uri = request.getRequestURI(); // 전체 요청 주소
		// ex) /wsp/notice/list.do
		
		String contextPath = request.getContextPath();
		// ex) /wsp
		
		String command = uri.substring((contextPath + "/notice").length() );
		
//		System.out.println(uri);
//		System.out.println(contextPath);
//		System.out.println(command);
		
		// 컨트롤러 내에서 공용으로 사용할 변수 미리 선언
		String path = null; // forward 또는  redirect 경로를 저장할 변수
		RequestDispatcher view = null; // 요청 위임 객체
		
		// sweet alert 메세지 전달하는 용도
		String swalIcon = null;
		String swalTitle = null;
		String swalText = null;
		
		String errorMsg = null;
		
		try {
			NoticeService service = new NoticeService();
			
			// 공지사항 목록 조회 Controller
			if(command.equals("/list.do")) {
				errorMsg = "공지사항 목록 조회 중 오류 발생";
				
				// 비즈니스 로직 처리 후 결과 반환받기
				List<Notice> list = service.selectList();
				// list를 jsp로 가져가서 화면에 출력해야함
				
				// 요청을 위임할 jsp 경로 지정
				path ="/WEB-INF/views/notice/noticeList.jsp";
				
				// 요청 위임 시 전달할 값 세팅
				request.setAttribute("list", list);
				
				// 요청 위임 객체 생성 후 위임 진행
				view = request.getRequestDispatcher(path);
				view.forward(request, response);
			}
			
		}catch(Exception e) {
			e.printStackTrace();
			path = "/WEB-INF/views/common/errorPage.jsp";
			request.setAttribute("errorMsg", errorMsg);
			view = request.getRequestDispatcher(path);
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

Notice.vo

```java
package com.kh.wsp.notice.model.vo;

import java.sql.Date;

/**
 * @author boryung
 *
 */
public class Notice {
	private int noticeNo; // 공지사항 번호
	private String noticeTitle; // 제목
	private String noticeContent; // 내용
	private String memberId; // 작성자 아이디
	private int readCount; // 조회수
	private Date noticeCreateDate; // 작성일
	private String noticeFl; // 삭제여부
	
	public Notice() {	}

	public Notice(int noticeNo, String noticeTitle, String noticeContent, String memberId, int readCount,
			Date noticeCreateDate, String noticeFl) {
		super();
		this.noticeNo = noticeNo;
		this.noticeTitle = noticeTitle;
		this.noticeContent = noticeContent;
		this.memberId = memberId;
		this.readCount = readCount;
		this.noticeCreateDate = noticeCreateDate;
		this.noticeFl = noticeFl;
	}

	public int getNoticeNo() {
		return noticeNo;
	}

	public void setNoticeNo(int noticeNo) {
		this.noticeNo = noticeNo;
	}

	public String getNoticeTitle() {
		return noticeTitle;
	}

	public void setNoticeTitle(String noticeTitle) {
		this.noticeTitle = noticeTitle;
	}

	public String getNoticeContent() {
		return noticeContent;
	}

	public void setNoticeContent(String noticeContent) {
		this.noticeContent = noticeContent;
	}

	public String getMemberId() {
		return memberId;
	}

	public void setMemberId(String memberId) {
		this.memberId = memberId;
	}

	public int getReadCount() {
		return readCount;
	}

	public void setReadCount(int readCount) {
		this.readCount = readCount;
	}

	public Date getNoticeCreateDate() {
		return noticeCreateDate;
	}

	public void setNoticeCreateDate(Date noticeCreateDate) {
		this.noticeCreateDate = noticeCreateDate;
	}

	public String getNoticeFl() {
		return noticeFl;
	}

	public void setNoticeFl(String noticeFl) {
		this.noticeFl = noticeFl;
	}

	@Override
	public String toString() {
		return "Notice [noticeNo=" + noticeNo + ", noticeTitle=" + noticeTitle + ", noticeContent=" + noticeContent
				+ ", memberId=" + memberId + ", readCount=" + readCount + ", noticeCreateDate=" + noticeCreateDate
				+ ", noticeFl=" + noticeFl + "]";
	}
}
```
<br>

NoticeService.java

```java
package com.kh.wsp.notice.model.service;

import static com.kh.wsp.common.JDBCTemplate.*;

import java.sql.Connection;
import java.util.List;

import com.kh.wsp.notice.model.dao.NoticeDAO;
import com.kh.wsp.notice.model.vo.Notice;

public class NoticeService {

	private NoticeDAO dao = new NoticeDAO();

	
	
	/** 공지사항 목록 조회 Service
	 * @return list
	 * @throws Exception
	 */
	public List<Notice> selectList() throws Exception {
		Connection conn = getConnection();
		
		List<Notice> list = dao.selectList(conn);
		
		close(conn);
		
		return list;
	}
}

```
<br>

NoticeDAO.java

```java
package com.kh.wsp.notice.model.dao;

import static com.kh.wsp.common.JDBCTemplate.*;

import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Properties;

import com.kh.wsp.notice.model.vo.Notice;

public class NoticeDAO {
   
   // 자주 사용하는 JDBC 참조 변수 미리 선언
   private Statement stmt = null;
   private PreparedStatement pstmt = null;
   private ResultSet rset = null;
   
   
   // 외부 XML 파일에 작성된 sql을 읽어올 변수 선언
   Properties prop = null;
   
   // 기본 생성자로 NoticeDAO 객체 생성 시 SQL이 작성된 xml 파일 얻어오기
   public NoticeDAO() {
      String fileName = NoticeDAO.class.getResource("/com/kh/wsp/sql/notice/notice-query.xml").getPath();
      try {
         prop = new Properties();
         prop.loadFromXML(new FileInputStream(fileName)); 
      } catch (Exception e) {
         e.printStackTrace();
      }
   }
   
/**	공지사항 목록 조회 DAO
 * @param conn
 * @return list
 * @throws Exception
 */
public List<Notice> selectList(Connection conn) throws Exception {
	List<Notice> list = null;
	
	String query = prop.getProperty("selectList");
	
	try {
		stmt = conn.createStatement();
		rset = stmt.executeQuery(query);
		
		
		// sql 수행 후 DB관련 문제가 발생하지 않으면 
		// 조회 내용을 저장할 수 있는 ArrayList 생성
		list = new ArrayList<Notice>();
		
		while(rset.next()) {
			Notice notice = new Notice();
			notice.setNoticeNo(rset.getInt("NOTICE_NO"));
			notice.setNoticeTitle(rset.getString("NOTICE_TITLE"));
			notice.setMemberId(rset.getString("MEMBER_ID"));
			notice.setReadCount(rset.getInt("READ_COUNT"));
			notice.setNoticeCreateDate(rset.getDate("NOTICE_CREATE_DT"));
			
			list.add(notice);
		}
	}finally {
		close(rset);
		close(stmt);
	}
	return list;
    }
}
```
<br>

notice-query.xml

```sql
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>

<entry key="selectList">
SELECT * FROM V_NOTICE_LIST
WHERE NOTICE_FL='Y'
</entry>
</properties>
```
<br>
<br>
<br>