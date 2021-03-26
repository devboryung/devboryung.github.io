---
title: "[숙소] 등록하기 -1"
excerpt: "세미프로젝트 - 뭉개뭉개"
search: true
categories: 
  - SemiProject
tags: 
  - JSP
  - Servlet
  - Java
  - HTML
  - CSS
toc: true
---


## [숙소] 등록하기

- 출력화면
![image](https://user-images.githubusercontent.com/73421820/112480670-9bf62c80-8db9-11eb-9b8d-e324b87a359e.png)
<br> <br>
![image](https://user-images.githubusercontent.com/73421820/112481407-5423d500-8dba-11eb-84d1-a3f6603cfd3d.png)


<br><br>

> 등록하기 버튼은 업체 회원, 관리자에게만 보여진다.


- JSP

```html
<!-- 등록하기 버튼 (업체/관리자에게만 보임)  
loginMember는 session에 등록되어 있음-->
<c:if
  test="${!empty loginMember && loginMember.memberAdmin == 'C' || loginMember.memberAdmin == 'A'}">
  <div class="row-item">
    <button type="button" class="btn_class" id="insertRoom"
      onclick="location.href = '${contextPath}/room/insertForm'">등록하기</button>
  </div>
</c:if>
```
<br><br>

> 버튼을 클릭하면 등록하기 화면으로 전환된다.

- 숙소 등록 화면 전환 Controller

```java
else if(command.equals("/insertForm")) {
    path = "/WEB-INF/views/room/roomInsert.jsp";
    view = request.getRequestDispatcher(path);
    view.forward(request, response);
}
```
<br><br>

- 숙소 등록 CSS

```css
@charset "UTF-8";

* {
	font-family: 'Noto Sans KR', sans-serif;
	font-weight: 500;
	/* 굵기 지정(100, 300, 400, 500, 700) */
	font-size: 16px;
	color: #212529;
	box-sizing: border-box;
	margin: 0;
	/* border : 1px solid black;  */
}

div {
	/* border : 1px solid black;  */
	box-sizing: border-box;
}

.wrapper {
	width: 1100px;
	margin: 0 auto;
	/* border : 1px solid sienna; */
}

.row-item {
	width: 100%;
	box-sizing: border-box;
	/* border : 1px solid yellow; */
	margin-bottom: 25px;
}

.main {
	width: 900px;
	height: 100%;
	margin: auto;
	padding-bottom: 3%;
}

#hr_tag {
	margin: auto;
	width: 360px;
	border: solid 1px orange;
}

/* 병원 등록하기 */
#page_name {
	text-align: center;
	height: 60px;
	line-height: 70px;
	font-size: 30px;
}

.insert_room {
	width: 600px;
	padding: 0;
	margin: 0 auto;
	/* border : 1px solid blue; */
}

/* number 태그 화살표 제거 */
input[type="number"]::-webkit-outer-spin-button, input[type="number"]::-webkit-inner-spin-button
	{
	-webkit-appearance: none;
	margin: 0;
}

/* 입력창 */
.label_name {
	display: inline-block;
	width: 20%;
	font-size: 14px;
	line-height: 0;
}

.input_tag {
	display: inline-block;
	width: 60%;
	height: 40px;
}

.highlighter {
	background: linear-gradient(to top, #e4f7fa 50%, transparent 100%);
}

/* 가득 채우는 인풋창  */
.full_input {
	width: 100%;
	height: 100%;
}

/* 전화번호 인풋창 */
.phone {
	width: 105px;
	height: 100%;
}

/* 체크인/아웃 인풋창 */
.checkHours {
	display: inline-block;
	width: 171px;
	height: 100%;
}

/* 부대시설 체크박스 */
.facility, .dog {
	margin: 8px;
}

/* 숙소 소개글  */
.room_info {
	resize: none; /* 크기고정 */
	height: 100px;
	font-size: 14px;
}

/* --------------- 업로드 버튼 --------------- */

/* 파일 선택 필드 숨기기 */
#titleImgArea {
	height: 100px;
}

.hospitalArea:hover {
	cursor: pointer;
}

.imgarea {
	width: 65px;
	display: inline-block;
}

/* 등록 취소 버튼 */
.btn_class {
	border-radius: 5px;
	color: #fff;
	border: 1px solid #8bd2d6;
	background-color: #8bd2d6;
	cursor: pointer;
	outline: none;
	width: 70px;
	height: 40px;
	margin-top: 50px;
}

.btn_item {
	margin-left: 235px;
}

.btn_class:hover {
	background-color: #17a2b8;
}
```

<br><br>

- 숙소 등록 JSP
> 첨부파일이 있기 때문에 form 태그의 enctype을 "multipart/form-data"형식으로 지정해야 된다.
> comMember는 로그인 시 Session에 등록되어있다.

```html

<jsp:include page="/WEB-INF/views/common/otherHeader.jsp"></jsp:include>

<!-- 주소  -->
<c:set var="address" value="${fn:split(comMember.comAddress,',') }" />


<!-- 숙소 등록하기 -->
<div class="wrapper">
    <div class="main">

        <div class="row-item">
            <div id="page_name">숙소 등록</div>
            <hr id="hr_tag">
        </div>


        <div class="insert_room">
            <form action="${contextPath }/room/insert" method="post"
                enctype="multipart/form-data" role="form"
                onsubmit="return roomInsertValidate();">


                <div class="row-item">
                    <div class="label_name">
                        <label for="companyName"> <span class="highlighter">숙소명</span>
                        </label>
                    </div>
                    <div class="input_tag">
                        <input type="text" class="full_input" id="roomName"
                            name="roomName" value="${comMember.comName}"
                            style="border: none; outline: none;" readonly>
                    </div>
                </div>


                <div class="row-item">
                    <div class="label_name">
                        <label for="phone"> <span class="highlighter">전화번호</span>
                        </label>
                    </div>
                    <div class="input_tag">
                        <input type="text" class="full_input" id="phone" name="phone"
                            value="${comMember.comPhone}"
                            style="border: none; outline: none;" readonly>
                    </div>
                </div>


                <div class="row-item">
                    <div class="label_name">
                        <label for="location2"> <span class="highlighter">상세주소</span>
                        </label>
                    </div>
                    <div class="input_tag">
                        <input type="text" class="full_input" id="location2"
                            name="location2" value="${address[1]}"
                            style="border: none; outline: none;" readonly>
                    </div>
                </div>

                <div class="row-item">
                    <div class="label_name">
                        <label for="checkIn"> <span class="highlighter">체크인/체크아웃</span>
                        </label>
                    </div>
                    <div class="input_tag">
                        <input type="text" class="checkHours" id="checkIn" name="checkin"
                            placeholder="00:00" autocomplete="off" required> &nbsp;/
                        <input type="text" class="checkHours" id="checkOut"
                            name="checkout" placeholder="00:00" autocomplete="off" required>
                    </div>
                </div>

                <div class="row-item">
                    <div class="label_name">
                        <div for="facility">
                            <span class="highlighter">숙소 부대 시설</span>
                        </div>
                    </div>
                    <div class="input_tag">
                        <label for="WiFi" style="cursor: pointer"> <input
                            type="checkbox" class="facility" name="facility" id="WiFi"
                            value="WiFi">WiFi
                        </label> <label for="주차장" style="cursor: pointer"><input
                            type="checkbox" class="facility" name="facility" id="주차장"
                            value="주차장">주차장</label> <label for="수영장" style="cursor: pointer"><input
                            type="checkbox" class="facility" name="facility" id="수영장"
                            value="수영장">수영장</label> <label for="BBQ" style="cursor: pointer"><input
                            type="checkbox" class="facility" name="facility" id="BBQ"
                            value="BBQ">BBQ</label> <label for="마당" style="cursor: pointer"><input
                            type="checkbox" class="facility" name="facility" id="마당"
                            value="마당">마당</label>
                    </div>
                </div>

                <div class="row-item">
                    <div class="label_name">
                        <label for="dog"> <span class="highlighter">출입 가능
                                견종</span>
                        </label>
                    </div>
                    <div class="input_tag">
                        <label for="소형견" style="cursor: pointer"><input
                            type="checkbox" class="dog" name="dog" id="소형견" value="소형견">소형견</label>
                        <label for="중형견" style="cursor: pointer"><input
                            type="checkbox" class="dog" name="dog" id="중형견" value="중형견">중형견</label>
                        <label for="대형견" style="cursor: pointer"> <input
                            type="checkbox" class="dog" name="dog" id="대형견" value="대형견">대형견
                        </label>

                    </div>
                </div>

                <div class="row-item">
                    <div class="label_name" style="vertical-align: 80px;">
                        <label for="room_info"> <span class="highlighter">숙소
                                상세 정보</span>
                        </label>
                    </div>
                    <div class="input_tag">
                        <textarea class="full_input room_info" id="room_info" rows="10"
                            name="room_info" placeholder="숙소 상세 정보를 작성해 주세요."></textarea>
                    </div>
                </div>


                <!-- 파일 업로드  -->

                <div class="row-item">
                    <div class="label_name">
                        <label for="titleImgArea"> <span class="highlighter">썸네일</span>
                        </label>
                    </div>
                    <div class="roomImg input_tag" id="titleImgArea">
                        <img id="titleImg" width="360" height="100">
                    </div>
                </div>

                <div class="row-item">
                    <div class="label_name">
                        <label class="img"> <span class="highlighter">업로드
                                이미지</span>
                        </label>
                    </div>
                    <div class="input_tag">
                        <div class="roomImg imgarea" id="roomImgArea1"
                            style="margin-right: 5px;">
                            <img id="roomImg1" width="65" height="65">
                        </div>
                        <div class="roomImg imgarea" id="roomImgArea2"
                            style="margin-right: 4px;">
                            <img id="roomImg2" width="65" height="65">
                        </div>
                        <div class="roomImg imgarea" id="roomImgArea3"
                            style="margin-right: 5px;">
                            <img id="roomImg3" width="65" height="65">
                        </div>
                        <div class="roomImg imgarea" id="roomImgArea4"
                            style="margin-right: 5px;">
                            <img id="roomImg4" width="65" height="65">
                        </div>
                        <div class="roomImg imgarea" id="roomImgArea5">
                            <img id="roomImg5" width="65" height="65">
                        </div>
                    </div>
                </div>

                <!-- 파일 업로드 버튼 (숨기기) -->
                <div id="fileArea">
                    <input type="file" id="img0" name="img0"
                        onchange="LoadImg(this,0)">
                    <!-- multiple 속성 = 사진 여러개 선택 가능  -->
                    <input type="file" id="img1" name="img1"
                        onchange="LoadImg(this,1)"> <input type="file" id="img2"
                        name="img2" onchange="LoadImg(this,2)"> <input
                        type="file" id="img3" name="img3" onchange="LoadImg(this,3)">
                    <input type="file" id="img4" name="img4"
                        onchange="LoadImg(this,4)"> <input type="file" id="img5"
                        name="img4" onchange="LoadImg(this,5)">
                </div>




                <!-- 등록 / 취소 버튼  -->
                <div class="row-item">
                    <div class="btn_item">
                        <button class="btn_class" id="insertBtn" type="submit">등록</button>
                        <button class="btn_class" id="resetBtn" type="reset">취소</button>
                    </div>
                </div>
            </form>

        </div>

    </div>

</div>
<!-- wrapper -->


<jsp:include page="/WEB-INF/views/common/footer.jsp"></jsp:include>
```

<br><br>

- 취소 버튼 클릭 시 script

```java
$("#resetBtn").on("click",function(){
	
 	if( confirm("등록을 취소하고 목록으로 돌아갑니다.")){
 		
 		location.href = "${contextPath}/room/list";
 	}
```


<br><br>

- 이미지 첨부 script

```java
//이미지 영역을 클릭할 때 파일 첨부 창이 뜨도록 설정하는 함수
// 페이지 로딩이 끝나고나면 #fileArea 요소를 숨김.
$(function(){
	$("#fileArea").hide(); 
	
	$(".roomImg").on("click",function(){// 이미지 영역이 클릭 되었을 때
		// 클릭한 이미지 영역 인덱스 얻어오기
		var index = $(".roomImg").index(this);
		// 클릭된 요소가 .hospitalImg 중 몇 번째 인덱스인지 반환
		
		// console.log(index);

		// 클릭된 영역 인덱스에 맞는 input file 태그 클릭
		$("#img" + index).click();
	});
});


// 각 영역에 파일을 첨부했을 때 미리보기 가능하게 하는 함수
function LoadImg(value,num){
	// value.files == input태그에 파일이 업로드되어 있으면 true
	// value.files[0] : 여러 파일 중 첫번째 파일이 업로드 되어있으면 true
	if(value.files && value.files[0] ){ // 해당 요소에 업로드된 파일이 있을 경우
		  var reader = new FileReader();
			// 자바스크립트 FileReader
		    // 웹 애플리케이션이 비동기적으로 데이터를 읽기 위하여 
		    // 읽을 파일을 가리키는 File 혹은 Blob객체를 이용해 파일의 내용을 읽고 
		    // 사용자의 컴퓨터에 저장하는 것을 가능하게 해주는 객체
	    
	    reader.readAsDataURL(value.files[0]);
	 		// FileReader.readAsDataURL()
   			// 지정된의 내용을 읽기 시작합니다. 
    		// Blob완료되면 result속성 data:에 파일 데이터를 나타내는 URL이 포함 됨.
    
    reader.onload = function(e){
	    	// FileReader.onload
				// load 이벤트의 핸들러. 
				// 이 이벤트는 읽기 동작이 성공적으로 완료 되었을 때마다 발생함.
				
				// 읽어들인 내용(이미지 파일)을 화면에 출력
				$(".roomImg").eq(num).children("img").attr("src", e.target.result);
	    	// e.target.result : 이벤트가 발생한 요소의 결과 , 파일 읽기 동작을 성공한 요소가 읽어들인 파일 내용
		 }
	  }
}


```

<br><br>

- 유효성 검사 script

```java
function roomInsertValidate(){
	
	// 시간 입력 정규식 00:00
	var regExp = /^(0[0-9]|1[0-9]|2[0-4]):([0-5][0-9])$/;
	
	var checkIn = $("#checkIn").val();
	var checkOut = $("#checkOut").val();
	
	if(!regExp.test(checkIn) || !regExp.test(checkOut)){
		alert("체크인/체크아웃 시간의 형식이 유효하지 않습니다.");
		$("#checkIn").focus();
		return false;
	}
	
	
	
	
	/* 병원정보에 내용이 입력이 안 된다면*/
	
	if ($("#room_info").val().trim().length ==0){
		alert("숙소 정보를 입력해 주세요.");
		$("#room_info").focus();
		return false;
	}
	
	/* 견종 필수 체크  */
	if(!checkedDog()){
		return false;
	}
	
}


    /* 견종 필수 체크  */
function checkedDog(){
    var checkedDog = document.getElementsByName("dog");
    
    for(var i=0; i< checkedDog.length; i++){
        if(checkedDog[i].checked == true){
            return true;
        }
    }
    alert("출입 가능 견종을 하나 이상 선택해 주세요.");
    return false
}
```