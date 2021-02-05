---
title: "2020년 02월 05일"
excerpt: "Spring 5 (마이페이지, 비밀번호 변경)"
search: true
categories: 
  - Academy
tags: 
  - FrameWork
  - Java
  - Spring
toc: true
---


## 마이페이지

<br>

- mypage.jsp


```java
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>내정보</title>
<style>
input[type="number"]::-webkit-outer-spin-button, input[type="number"]::-webkit-inner-spin-button
	{
	-webkit-appearance: none;
	margin: 0;
}

#content-main {
	min-height: 770px;
}
</style>
</head>
<body>
<jsp:include page="../common/header.jsp" />
<div class="container mt-5 pt-5" id="content-main">

<div class="row">
<div class="col-sm-9">
  <h1>My Page</h1>
  <hr>
  <div class="bg-white rounded shadow-sm container p-3">
    <form method="POST" action="updateAction" name="updateForm" onsubmit="return updateValidate();" class="form-horizontal" role="form">
      <!-- 아이디 -->
      <div class="row mb-3 form-row">
        <div class="col-md-3">
          <h6>아이디</h6>
        </div>
        <div class="col-md-6">
          <h5 id="id">${loginMember.memberId }</h5>
        </div>
      </div>

      <!-- 이름 -->
      <div class="row mb-3 form-row">
        <div class="col-md-3">
          <h6>이름</h6>
        </div>
        <div class="col-md-6">
          <h5 id="name">${loginMember.memberName }</h5>
        </div>
      </div>

      <!-- 전화번호 -->
        <c:set var="phone" value="${fn:split(loginMember.memberPhone,'-')}"></c:set>
      
      
      
      <div class="row mb-3 form-row">
        <div class="col-md-3">
          <label for="phone1">전화번호</label>
        </div>

        <!-- 전화번호1 -->
        <!-- JSTL로 옵션 선택하기. -->
        <div class="col-md-3">
          <select class="custom-select" id="phone1" name="phone1">
            <option <c:if test="${phone[0] == '010'}">selected</c:if>>010</option>
            <option <c:if test="${phone[0] == '011'}">selected</c:if>>011</option>
            <option <c:if test="${phone[0] == '016'}">selected</c:if>>016</option>
            <option <c:if test="${phone[0] == '017'}">selected</c:if>>017</option>
            <option <c:if test="${phone[0] == '019'}">selected</c:if>>019</option>
          </select>
        </div>
        


        <!-- 전화번호2 -->
        <div class="col-md-3">
          <input type="number" class="form-control phone" id="phone2" name="phone2" maxlength="4" value="${phone[1] }">
        </div>

        <!-- 전화번호3 -->
        <div class="col-md-3">
          <input type="number" class="form-control phone" id="phone3" name="phone3" maxlength="4" value="${phone[2] }">
        </div>
      </div>

      <!-- 이메일 -->
      <div class="row mb-3 form-row">
        <div class="col-md-3">
          <label for="memberEmail">Email</label>
        </div>
        <div class="col-md-6">
          <input type="email" class="form-control" id="email" name="memberEmail" value="${loginMember.memberEmail }">
        </div>
      </div>
      <br>

      <!-- 주소 -->
      <!-- 오픈소스 도로명 주소 API -->
      <!-- https://www.poesis.org/postcodify/ -->
      
        <c:set var="address" value="${fn:split(loginMember.memberAddress, ',')}"></c:set>
      
      
      <div class="row mb-3 form-row">

        <div class="col-md-3">
          <label for="postcodify_search_button">우편번호</label>
        </div>
        <div class="col-md-3">
          <input type="text" name="post" id="post" class="form-control postcodify_postcode5" value="${address[0] }">
        </div>
        <div class="col-md-3">
          <button type="button" class="btn btn-success" id="postcodify_search_button">검색</button>
        </div>
      </div>

      <div class="row mb-3 form-row">
        <div class="col-md-3">
          <label for="address1">도로명 주소</label>
        </div>
        <div class="col-md-9">
          <input type="text" class="form-control postcodify_address" name="address1" id="address1" value="${address[1] }">
        </div>
      </div>

      <div class="row mb-3 form-row">
        <div class="col-md-3">
          <label for="address2">상세주소</label>
        </div>
        <div class="col-md-9">
          <input type="text" class="form-control postcodify_details" name="address2" id="address2" value="${address[2] }">
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
            <input type="checkbox" name="memberInterest" id="sports" value="운동" class="form-check-input custom-control-input"> <label class="form-check-label custom-control-label" for="sports">운동</label>
          </div>
          <div class="form-check form-check-inline">
            <input type="checkbox" name="memberInterest" id="movie" value="영화" class="form-check-input custom-control-input"> <label class="form-check-label custom-control-label" for="movie">영화</label>
          </div>
          <div class="form-check form-check-inline">
            <input type="checkbox" name="memberInterest" id="music" value="음악" class="form-check-input custom-control-input"> <label class="form-check-label custom-control-label" for="music">음악</label>
          </div>
          <br>
          <div class="form-check form-check-inline">
            <input type="checkbox" name="memberInterest" id="cooking" value="요리" class="form-check-input custom-control-input"> <label class="form-check-label custom-control-label" for="cooking">요리</label>
          </div>
          <div class="form-check form-check-inline">
            <input type="checkbox" name="memberInterest" id="game" value="게임" class="form-check-input custom-control-input"> <label class="form-check-label custom-control-label" for="game">게임</label>
          </div>

          <div class="form-check form-check-inline">
            <input type="checkbox" name="memberInterest" id="etc" value="기타" class="form-check-input custom-control-input"> <label class="form-check-label custom-control-label" for="etc">기타</label>
          </div>
        </div>



      </div>

      <hr class="mb-4">
      <button class="btn btn-success btn-lg btn-block" type="submit">수정</button>
    </form>
  </div>
</div>

  <jsp:include page="sideMenu.jsp" />
</div>
</div>
<jsp:include page="../common/footer.jsp" />

<!-- 도로명 주소 API -->
<script src="//d1p7wdleee1q2z.cloudfront.net/post/search.min.js"></script>
<script>

// 도로명 주소 API
// 검색 단추를 누르면 팝업 레이어가 열리도록 설정한다.
$(function() {
  $("#postcodify_search_button")
      .postcodifyPopUp();
});

// 전화번호 숫자 4글자 이상 작성 방지
$(".phone").on("input", function() {
  if ($(this).val().length > $(this).prop("maxLength")) {
    $(this).val($(this).val().slice(0, $(this).prop("maxLength")));
  }
});



// 각 유효성 검사 결과를 저장할 객체
var validateCheck = {
  "phone2" : false,
  "email" : false
};


// submit 동작
function updateValidate() {
  
  var $phone2 = $("#phone2");
  var $phone3 = $("#phone3");
  var $email = $("#email");

  // 전화번호 유효성 검사
  var regExp1 = /^\d{3,4}$/; // 숫자 3~4 글자
  var regExp2 = /^\d{4,4}$/; // 숫자 4 글자

  if (!regExp1.test($phone2.val()) || !regExp2.test($phone3.val())) {
    validateCheck.phone2 = false;
  } else {
    validateCheck.phone2 = true;
  }

  // 이메일 유효성 검사
  var regExp3 = /^[\w]{4,}@[\w]+(\.[\w]+){1,3}$/;

  if (!regExp3.test($email.val())) {
    validateCheck.email = false;
  } else {
    validateCheck.email = true;
  }

  for ( var key in validateCheck) {
    if (!validateCheck[key]) {
      var str;
      switch (key) {
      case "phone2": str = "전화번호"; break;
      case "email": str = "이메일";	break;
      }

      swal({icon : "warning",	title : str + " 형식이 유효하지 않습니다."})
      .then(function() {
        var id = "#" + key;
        $(id).focus();
      });

      return false;
    }
  }

  $memberPhone = $("<input>", {
            type : "hidden",
            name : "memberPhone",
            value : $("#phone1").val() + "-" + $("#phone2").val() + "-" + $("#phone3").val()
          });

  $memberAddress = $("<input>", {
            type : "hidden",
            name : "memberAddress",
            value : $("#post").val() + "," + $("#address1").val() + "," + $("#address2").val()
          });

  $("form[name='updateForm']").append($memberPhone).append($memberAddress);
  
}

//******************** 전화 번호 체크  ***************************
/*
스크립트로 하는 방법
(function(){
  $("#phone1>option").each(function(index,item){
    if("${phone[0]}"==$(item).text()){
      $(item).prop("selected",true);
    }
  });
  
})();
*/		
//******************** 전화 번호 체크  ***************************




//******************** 관심 분야 체크  ***************************
  (function(){
      var interest = "${loginMember.memberInterest}".split(",");
      $("input[name='memberInterest']").each(function(index, item){
        
          // 인덱스 번호를 반환 함.  해당 인덱스를 체크 함.
        if(interest.indexOf($(item).val()) != -1){
            $(item).prop("checked", true);
        }
      });
      
  })();


//******************** 관심 분야 체크  ***************************
</script>

</body>
</html>

```

<br><br><br>


- MemberController2.java

```java
// 내 정보 페이지 전환용 Controller
@RequestMapping("mypage")
public String myPage() {
  return "member/mypage";
}




// 회원 정보 수정 Controller

// updateAction 요청, post 방식만 받아들인다.
// 나중에 주소는 같지만 전달받은 방식에 따라서 처리하는 방법을 다르게 하는 메소드가 생길 수 있음.
@RequestMapping(value="updateAction", method=RequestMethod.POST)
public String updateAction(@ModelAttribute Member updateMember, Model model,
            @ModelAttribute(name="loginMember", binding=false) Member loginMember, RedirectAttributes ra ) {
          // binding 속성 : 요청 파라미터를 해당 객체에 반영 할 것 인가?(파라미터를 얻어와 세팅하는 현상을 막아서 session의 값대로 남아있음)
  
  // updateMember : 이메일, 전화번호, 주소, 관심분야
  // 세션에서 회원번호를 얻어와서, 해당 회원의 정보를 업데이트 함.
  // 세션에서 회원번호를 얻어오는 방법.
  // 1. HttpSession의 getAttribute("loginMember")
  // 2. Model, @SessionAttributes로 세션에 등록된 값은 반대로 얻어오는 것도 가능함.
  //    Member loginMember = (Member)model.getAttribute("loginMember");
  // 3. @ModelAttribute 이용하여 Model로 세팅한 값을 반대로 얻어오는 것도 가능함.
  //  매개변수에 @ModelAttribute("모델로 등록한 key값") 자료형  변수명
  //  세팅된 값이 자동으로 변수에 들어간다.
  // System.out.println(loginMember); 
  
  

  // 수정된 회원정보 + 로그인된 회원의 번호를 가지고  '회원정보 수정Service' 수행
  
  // 매개변수 하나만 보내기 위해  loginMember에서 회원 번호를 얻어와  updateMember에 세팅
  updateMember.setMemberNo(loginMember.getMemberNo());
  
  int result = service.updateAction(updateMember);
  
  if(result>0) {
    // 성공시  swal창이 뜨며 session에 저장된 변경 전 회원정보를 수정된 내용으로 변경
    swalIcon ="success";
    swalTitle = "회원 정보 수정 성공";
    
    // updateMember에는 email.phone,address,interest 업데이트 되는 정보들만 담겨져있다.
    // loginMember에는 비밀번호 뺀 나머지 정보들이 담겨져 있음, 합쳐서 loginMember 에 저장하기.
      loginMember.setMemberEmail(updateMember.getMemberEmail());
      loginMember.setMemberPhone(updateMember.getMemberPhone());
      loginMember.setMemberAddress(updateMember.getMemberAddress());
      loginMember.setMemberInterest(updateMember.getMemberInterest());
      
      // 변경된 정보가 담긴 loginMember 변수를 다시 Session에 추가
      model.addAttribute("loginMember",loginMember);
  }
  else {
    swalIcon ="error";
    swalTitle = "회원 정보 수정 실패";
  }
  
  ra.addFlashAttribute("swalIcon",swalIcon);
  ra.addFlashAttribute("swalTitle",swalTitle);
  
  return "redirect:mypage"; // 상대경로
}

```

<br><br><br>

- MemberService2

```java
/** 회원정보 수정 Service
  * @param updateMember
  * @return result
  */
int updateAction(Member updateMember);
```
<br><br><br>


- MemberService2Impl

```java
// 회원정보 수정 Service 구현
@Transactional(rollbackFor = Exception.class) // 모든 예외가 발생하면 처리
@Override
public int updateAction(Member updateMember) {
  return dao.updateAction(updateMember);
}
```
<br><br><br>

- MemberDAO2

```java
/** 회원정보 수정 DAO
  * @param updateMember
  * @return result
  */
public int updateAction(Member updateMember) {
  return sqlSession.update("memberMapper2.updateAction", updateMember);
}

```
<br><br><br>

- member-mapper2

```sql
<!-- 회원정보 수정  -->
<update id="updateAction" parameterType="Member">
  UPDATE MEMBER SET
  MEMBER_PHONE = #{memberPhone},
  MEMBER_EMAIL = #{memberEmail},
  MEMBER_ADDR = #{memberAddress}, 
  MEMBER_INTEREST = #{memberInterest}
  WHERE MEMBER_NO = #{memberNo}
</update>
```
<br><br><br>

<br><br><br><br>

## 비밀번호 변경


- changePwd.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>비밀번호 변경</title>
</head>
<style>
#content-main{
  min-height: 770px;
}
</style>
<body>
<jsp:include page="../common/header.jsp"/>
<div class="container mt-5 pt-5" id="content-main">

<div class="row">
  <div class="col-sm-9">
    <h1>Change Password</h1>
    <hr>
    <div class="bg-white rounded shadow-sm container p-3">
      <form method="POST" action="updatePwd" onsubmit="return validate();" class="form-horizontal" role="form">
        <!-- 아이디 -->
        <div class="row mb-3 form-row">
          <div class="col-md-3">
            <h6>아이디</h6>
          </div>
          <div class="col-md-6">
            <h5 id="id">${loginMember.memberId}</h5>
          </div>
        </div>

        <hr>
        <!-- 현재 비밀번호 -->
        <div class="row mb-3 form-row">
          <div class="col-md-3">
            <h6>현재 비밀번호</h6>
          </div>
          <div class="col-md-6">
            <input type="password" class="form-control" id="memberPwd" name="memberPwd">
          </div>
        </div>

        <!-- 새 비밀번호 -->
        <div class="row mb-3 form-row">
          <div class="col-md-3">
            <h6>새 비밀번호</h6>
          </div>
          <div class="col-md-6">
            <input type="password" class="form-control" id="newPwd1" name="newPwd1">
          </div>
        </div>

        <!-- 새 비밀번호 확인-->
        <div class="row mb-3 form-row">
          <div class="col-md-3">
            <h6>새 비밀번호 확인</h6>
          </div>
          <div class="col-md-6">
            <input type="password" class="form-control" id="newPwd2" name="newPwd2">
          </div>
        </div>
        
        <hr class="mb-4">
        <button class="btn btn-success btn-lg btn-block" type="submit">변경하기</button>
      </form>
    </div>
  </div>
    
    <jsp:include page="sideMenu.jsp"/>
  </div>
</div>
<jsp:include page="../common/footer.jsp"/>

<script>
  // submit 동작
  function validate() {
    // 비밀번호  유효성 검사
    //영어 대,소문자 + 숫자, 총 6~12글자
    var regExp = /^[A-Za-z0-9]{6,12}$/;
    if(!regExp.test($("#newPwd1").val())){ 
      alert("유효하지 않은 비밀번호 입니다.");
      $("#newPwd1").focus();
      
      return false;
          }
    
    if($("#newPwd1").val() != $("#newPwd2").val()){
      alert("새 비밀번호가 일치하지 않습니다.");
      $("#newPwd2").focus();
      
      return false;
    }

  }
  
</script>

</body>
</html>

```

<br><br><br>



- MemberContorller2

```java
// 비밀번호 변경 화면 전환용 Controller
@RequestMapping("changePwd")
public String changePwd() {
  return "member/changePwd";
}



// 비밀번호 변경 Controller
@RequestMapping(value="updatePwd", method=RequestMethod.POST)
public String updatePwd(@RequestParam("memberPwd") String memberPwd, 
            @RequestParam("newPwd1") String newPwd,
            @ModelAttribute(name="loginMember",binding=false) Member loginMember,
            RedirectAttributes ra) {
  
  
  // Map을 이용하여 필요한 데이터를 하나로 묶어둠
  Map<String,Object> map = new HashMap<String,Object>();
  map.put("memberPwd",memberPwd);
  map.put("newPwd",newPwd);
  map.put("memberNo",loginMember.getMemberNo());
  
  
  // 비밀번호 변경 Service 호출
  int result = service.updatePwd(map);
  
  String url=null;
  // 성공 시 마이페이지 재요청
  if(result>0) {
    swalIcon="success";
    swalTitle="비밀번호 변경 성공";
    
    url="mypage";
  
  // 실패 시 비밀번호 변경 페이지 재요청
  }else {
    swalIcon="error";
    swalTitle ="비밀번호 변경 실패";
    
    url="changePwd";
  }
  
  ra.addFlashAttribute("swalIcon",swalIcon);
  ra.addFlashAttribute("swalTitle",swalTitle);
  
  return "redirect:" + url;
}
```

<br><br><br>


- MemberService2

```java
/** 비밀번호 변경 Service
  * @param map
  * @return result
  */
int updatePwd(Map<String, Object> map);
```
<br><br>


- MemberService2Impl

```java
// 비밀번호 변경 Service 구현
@Transactional(rollbackFor = Exception.class)
@Override
public int updatePwd(Map<String, Object> map) {
  // 현재 비밀번호, 새 비밀번호, 회원 번호
  
  // 1. 현재 비밀번호가 일치하는지 확인(본인 확인)
  // bcrypt 암호화가 적용되어 있기 때문에 DB에서 비밀번호를 조회해서 비교해야 한다. == 현재 비밀번호 조회 DAO
  String savePwd = dao.selectPwd((int)map.get("memberNo"));
  
  
  // 결과 저장용 변수 선언
  int result =0;
  
  if(savePwd!=null) { // 비밀번호 조회 성공 시
    
    // 비밀번호 비교
    if(enc.matches((String)map.get("memberPwd"), savePwd)) {
      // 2. 현재 비밀번호 일치 확인 시 새 비밀번호로 변경(새 비밀번호 암호화)
      // == 비밀번호를 수정할 DAO 필요
      
      // 새 비밀번호 암호화 진행
      String encPwd = enc.encode((String)map.get("newPwd")); 
      // 암호화 된 비밀번호를 map에 세팅
      map.put("newPwd",encPwd);
      
      // 새 비밀번호로 수정 DAO
      result = dao.updatePwd(map);
    }
  }
  return result;
}
```
<br><br><br>

- MemberDAO2

```java
/**	비밀번호 조회 DAO
  * @param memberNo
  * @return savePwd
  */
public String selectPwd(int memberNo) {
  return sqlSession.selectOne("memberMapper2.selectPwd", memberNo);
}




/** 비밀번호 수정 DAO
  * @param map
  * @return
  */
public int updatePwd(Map<String, Object> map) {
  return sqlSession.update("memberMapper2.updatePwd",map);
}
```
<br><br><br>

- member-mapper2

```sql
<!-- 비밀번호 조회  -->
<select id="selectPwd" parameterType="_int" resultType="string">
  SELECT MEMBER_PWD FROM MEMBER
  WHERE  MEMBER_NO = #{memberNo}
</select>


  <!-- 비밀번호 변경 -->
<update id="updatePwd" parameterType="map" >
      UPDATE MEMBER SET
      MEMBER_PWD = #{newPwd} <!-- map에 저장된 데이터의 key 값을 직접 작성 -->
      WHERE MEMBER_NO = #{memberNo}
</update>
```
<br><br><br><br><br>
