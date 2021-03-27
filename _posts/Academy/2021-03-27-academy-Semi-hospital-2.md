---
title: "[동물병원] 수정,삭제"
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

> 관리자에게만 수정, 삭제 버튼이 보인다.

- 출력화면

![image](https://user-images.githubusercontent.com/73421820/112722364-66ce1380-8f4c-11eb-8ddb-6fa4d3b121ca.png)


<br><br>

- JSP

```java
<c:if test="${!empty loginMember && loginMember.memberAdmin == 'A'   }">
    <div class="row-item" style="margin-top:50px;margin-bottom: 50px;">
        <div class="btn_item">
            
            <!-- 	수정 버튼 클릭 -> 수정 화면 -> 수정 성공 -> 상세조회 화면
            검색 -> 검색목록 -> 상세조회 -> 수정 버튼 클릭 -> 수정화면 -> 수정 성공 -> 상세조회 화면
                -->	 
            <%-- 게시글 수정 후 상세조회 페이지로 돌아오기 위한 url 조합 --%>
                <c:if test="${!empty param.sv && !empty param.sk }">
                    <%-- 검색을 통해 들어온 상세 조회 페이지인 경우 --%>
                <c:set var="searchStr" value="&sk=${param.sk}&sv=${param.sv}"/>
                </c:if>
            <a href="updateForm?cp=${param.cp}&hospitalNo=${param.hospitalNo}${searchStr}" 
                        class= "btn_class"  id="updateBtn">수정</a>
            <button class= "btn_class"  id="deleteBtn" type="button">삭제</button>
        
    </div>
</c:if>
```

<br><br>


## [동물병원] 삭제 


- 삭제 버튼을 클릭했을 때 실행되는 script

```java
<script>
	$("#deleteBtn").on("click",function(){
		if(confirm("해당 병원을 삭제하시겠습니까?")){
			location.href="delete?hospitalNo=${param.hospitalNo}"
		}
	});
</script>
```
<br><br>

- /hospital/delete 요청을 전달받는 Controller

```java
else if(command.equals("/delete")) {
    errorMsg = "삭제 과정에서 오류 발생";

    // 병원번호 얻어오기
    
    int hospitalNo = Integer.parseInt(request.getParameter("hospitalNo"));
    // 병원 삭제 (DELETE_FL -> 'Y'로 변경)
    int result = service.deleteHospital(hospitalNo);
    
    
    if(result>0) {
        swalIcon = "success";
            swalTitle="삭제 성공";
            
            path = "list";
    }else {
            swalIcon = "error";
            swalTitle = "삭제 실패";
            
            path = request.getHeader("referer");
    }
        request.getSession().setAttribute("swalIcon", swalIcon);
        request.getSession().setAttribute("swalTitle", swalTitle);
        
        response.sendRedirect(path);
    
}
```

<br><br>

- 동물병원 삭제 Service

```java
public int deleteHospital(int hospitalNo) throws Exception {
    Connection conn = getConnection();
    
    int result = dao.deleteHospital(conn, hospitalNo);
    
    if(result>0) commit(conn);
    else		rollback(conn);
    
    close(conn);
    return result;
}
```

<br><br>

- 동물병원 삭제 DAO

```java
public int deleteHospital(Connection conn, int hospitalNo) throws Exception {
    int result =0;
    String query = prop.getProperty("deleteHospital");
    
    try {
        pstmt = conn.prepareStatement(query);
        pstmt.setInt(1, hospitalNo);
        
        result = pstmt.executeUpdate();
        
    }finally {
        close(pstmt);
    }
    return result;
}
```

<br><br>


- 동물병원 삭제 sql문

```sql
<entry key="deleteHospital">
UPDATE HOSPITAL SET
HOSP_DEL_FL = 'Y'
WHERE HOSP_NO = ?
</entry>
```
<br><br>


## [동물병원] 수정

- 출력화면

![image](https://user-images.githubusercontent.com/73421820/112722452-ff649380-8f4c-11eb-8f38-9710ea7d33f7.png)


<br><br>

- 수정 버튼 클릭시 수정 화면 전환 Controller

```java
else if(command.equals("/updateForm")) {
    errorMsg ="동물병원 수정 화면 불러오는 과정에서 오류 발생";
    
    // 병원번호를 이용하여 이전에 작성했던 내용 조회해 옴
    int hospitalNo = Integer.parseInt(request.getParameter("hospitalNo"));
    Hospital hospital = service.updateView(hospitalNo);
    
    
    if(hospital!=null) {
        // 첨부된 파일을 가져 옴
        List<Attachment> fList = service.selectHospitalFiles(hospitalNo);
        
        if(!fList.isEmpty()) {
            request.setAttribute("fList", fList);
        }
        
        path = "/WEB-INF/views/hospital/hospitalUpdate.jsp";
        request.setAttribute("hospital", hospital);
        view = request.getRequestDispatcher(path);
        view.forward(request, response);
        
    }else {
        request.getSession().setAttribute("swalIcon", "error");
        request.getSession().setAttribute("swalTitle", "동물 병원 수정 화면 전환에 실패했습니다.");
        response.sendRedirect(request.getHeader("referer"));
    }
    
    
}
```

<br><br>

- 기존 동물병원의 정보를 조회해오는 Service

```java
public Hospital updateView(int hospitalNo) throws Exception {
    Connection conn = getConnection();
    
    Hospital hospital = dao.selectHospital(conn, hospitalNo);
    
    // 크로스 스크립팅 방지를 위해 개행문자가 <br>로 바뀌어 있는 상태 -> \r\n 으로 바꿈
    hospital.setHospInfo(hospital.getHospInfo().replaceAll("<br>", "\r\n"));
    
    close(conn);
    
    return hospital;
}
```

<br><br>

- 기존 병원의 정보를 조회해오는 DAO, sql문은 상세조회와 동일


<br><br>

- 기존 병원의 정보를 화면에 뿌려준다
JSP

```html
<jsp:include page="/WEB-INF/views/common/otherHeader.jsp"/>

<!-- 전화번호를 구분자를 이용하여 분리된 배열 형태로 저장  -->
<c:set var="phone" value="${fn:split(hospital.phone,'-') }"/>

<!-- 동물병원 수정 -->
<div class="wrapper">
        <div class="main">
            <div class="row-item">
                <div id="page_name">병원 수정</div>
                <hr id="hr_tag">
            </div>
            
         <c:if test="${!empty param.sk && !empty param.sv}">
			<c:set var="searchStr" value="&sk=${param.sk}&sv=${param.sv}"/>
		</c:if>
            
            
            <div class="update_hospital">
                <form action="${contextPath}/hospital/update?cp=${param.cp}&hospitalNo=${param.hospitalNo}${searchStr}" method="post"  
                      enctype="multipart/form-data"  role="form" onsubmit="return hospitalInsertValidate();">
                    
                    <div class="row-item">
                        <div class="label_name">
                            <label for="location1">
                        	 	<span class="highlighter">지역</span>
                            </label>
                        </div>
                        <div class="input_tag">
                            <select class="full_input" id="location1" name="location1" required>
                                <option value="강원도">강원도</option>
                                <option value="경기도">경기도</option>
                                <option value="경상도">경상도</option>
                                <option value="광주">광주</option>
                                <option value="대구">대구</option>
                                <option value="대전">대전</option>
                                <option value="부산">부산</option>
                                <option value="서울">서울</option>
                                <option value="세종">세종</option>
                                <option value="울산">울산</option>
                                <option value="인천">인천</option>
                                <option value="전라도">전라도</option>
                                <option value="제주">제주</option>
                                <option value="충청도">충청도</option>
                            </select>
                        </div>
                    </div>


                    <div class="row-item">
                        <div class="label_name">
                            <label for="companyName">
                            	<span class="highlighter">병원명</span>
                            </label>
                        </div>
                        <div class="input_tag">
                            <input type="text" class="full_input" id="companyName"  name="hospNm" value="${hospital.hospNm }" autocomplete="off" required>
                        </div>
                    </div>

                    <div class="row-item">
                        <div class="label_name">
                            <label for="phone">
                            	<span class="highlighter">전화번호</span>
                            </label>
                        </div>
                        <div class="input_tag">
                            <select class="phone" id="phone1" name="phone1" required> 
                                <option>02</option>
                                <option>031</option>
                                <option>032</option>
                                <option>033</option>
                                <option>041</option>
                                <option>042</option>
                                <option>043</option>
                                <option>044</option>
                                <option>051</option>
                                <option>052</option>
                                <option>053</option>
                                <option>054</option>
                                <option>055</option>
                                <option>061</option>
                                <option>062</option>
                                <option>063</option>
                                <option>064</option>
                                <option>070</option>
                            </select>
                            &nbsp;-&nbsp;&nbsp;
                            <input type="number" class="phone phoneTest" id="phone2" name="phone2" value="${phone[1] }" required>
                            &nbsp;-&nbsp;
                            <input type="number" class="phone phoneTest" id="phone3" name="phone3" value="${phone[2] }"  required>
                        </div>
                    </div>

                    
                    <div class="row-item">
                        <div class="label_name">
                            <label for="location2">
                            	<span class="highlighter">상세주소</span>
                            </label>
                        </div>
                        <div class="input_tag">
                            <input type="text" class="full_input" id="location2" name="location2" value="${hospital.location2 }" autocomplete="off" required>
                        </div>
                    </div>

                    <div class="row-item">
                        <div class="label_name">
                            <label for="office_hours">
                            	<span class="highlighter">영업시간</span>
                            </label>
                        </div>
                        <div class="input_tag">

                            <input type="text" class="office_hours" id="openTime" placeholder="00:00" name="openTime" value="${hospital.openingTime }" autocomplete="off" required>
                            &nbsp;~&nbsp;&nbsp;
                            <input type="text" class="office_hours" id="closeTime" placeholder="00:00" name="closeTime" value="${hospital.closingTime }" autocomplete="off" required>
                        </div>
                    </div>

                    <div class="row-item">
                        <div class="label_name">
                            <label for="facility">
                            	<span class="highlighter">병원 시설</span>
                            </label>
                        </div>
                        <div class="input_tag">
                           <label for="Wifi" style="cursor:pointer"><input type="checkbox" class="facility" name="hosp_facility" id="Wifi" value="WiFi">WiFi</label>
                           <label for="parcking" style="cursor:pointer"><input type="checkbox" class="facility" name="hosp_facility" id="parcking" value="주차">주차</label>
                           <label for="appointment" style="cursor:pointer"><input type="checkbox" class="facility" name="hosp_facility" id="appointment" value="예약">예약</label>
                          <label for="24hour" style="cursor:pointer"> <input type="checkbox" class="facility" name="hosp_facility" id="24hour" value="24시간">24시간</label>
                        </div>
                    </div>


                    <div class="row-item">
                        <div class="label_name" style="vertical-align:80px;" >
                            <label for="hospital_info" >
                            	<span class="highlighter">동물병원 정보</span>
                            </label>
                        </div>
                        <div class="input_tag">
                            <textarea class="full_input hospital_info" id="hospital_info" rows="10"
                                name ="hospital_info">${hospital.hospInfo }</textarea>
                        </div>
                    </div>
                    
					<!-- 파일 업로드  -->
                    <div class="row-item">
                    	<div class="label_name">
							<label for="titleImgArea">
								<span class="highlighter">썸네일</span>
							</label>
                    	</div>
					<div class="hospitalImg input_tag" id="titleImgArea">
						<img id="titleImg" width="360" height="100" >
					</div>
				</div>

				<div class="row-item"  >
					<div class="label_name">
						<label class="img">
							<span class="highlighter">업로드 이미지</span>
						</label>
					</div>
					<div class="input_tag">
						<div class="hospitalImg imgarea" id="hotpitalImgArea1" style="margin-right:5px;">
							<img id="hospitalImg1" width="65" height="65" >
						</div>
						<div class="hospitalImg imgarea" id="hotpitalImgArea2" style="margin-right:4px;">
							<img id="hospitalImg2" width="65" height="65" >
						</div>
						<div class="hospitalImg imgarea" id="hotpitalImgArea3" style="margin-right:5px;"> 
							<img id="hospitalImg3" width="65" height="65" >
						</div>
						<div class="hospitalImg imgarea" id="hotpitalImgArea4" style="margin-right:5px;">	
							<img id="hospitalImg4" width="65" height="65" >
						</div>	
						<div class="hospitalImg imgarea" id="hotpitalImgArea5">	
							<img id="hospitalImg5" width="65" height="65">
						</div>
					</div>
				</div>
			
			<!-- 파일 업로드 버튼 (숨기기) -->
					<div id="fileArea">
						<input type="file" id="img0" name="img0" onchange="LoadImg(this,0)">
						<!-- multiple 속성 = 사진 여러개 선택 가능  --> 
						<input type="file" id="img1" name="img1" onchange="LoadImg(this,1)"> 
						<input type="file" id="img2" name="img2" onchange="LoadImg(this,2)"> 
						<input type="file" id="img3" name="img3" onchange="LoadImg(this,3)">
						<input type="file" id="img4" name="img4" onchange="LoadImg(this,4)">
						<input type="file" id="img5" name="img5" onchange="LoadImg(this,5)">
					</div>

                    <!-- 등록 / 취소 버튼  -->
                    <div class="row-item">
                        <div class="btn_item">
                            <button class= "btn_class"  id="updateBtn" type="submit">수정</button>
                            <button class= "btn_class"  id="resetBtn" type="button">취소</button>
                        </div>
                    </div>
                </form>
	        </div>
	    </div>
	</div>
<jsp:include page="/WEB-INF/views/common/footer.jsp"/>
```


<br><br>

- 병원 수정 시 사용되는 script

```java
<script>
/* 취소 버튼이 눌리면 확인창이 뜬다.  */
$("#resetBtn").on("click",function(){
	
 	if( confirm("수정을 취소합니다.")){
 		
 		location.href = "${header.referer}";
 	}
});

/* --------------------유효성 검사---------------  */

function hospitalInsertValidate(){
	
	// 시간 입력 정규식 00:00
	var regExp = /^(0[0-9]|1[0-9]|2[0-4]):([0-5][0-9])$/;
	
	var open = $("#openTime").val();
	var close = $("#closeTime").val();
	
	if(!regExp.test(open) || !regExp.test(close)){
		alert("영업 시간의 형식이 유효하지 않습니다.");
		return false;
	}
	
	/* 병원정보에 내용이 입력이 안 된다면*/
	
	if ($("#hospital_info").val().trim().length ==0){
		alert("동물 병원 정보를 입력해 주세요.");
		$("#hospital_info").focus();
		return false;
	}
	
	
	/* 전화번호 3/4글자 입력  */
	var regExp1 = /^\d{3,4}$/;
	var regExp2 = /^\d{4}$/;
	 var v1 = $("#phone2").val();
	 var v2 = $("#phone3").val();
	 if(!regExp1.test(v1) || !regExp2.test(v2)){
			alert("전화번호를 다시 입력해 주세요.");
			$("#phone2").focus();
		    return false;
		 }	
	
}


/* 전화번호 4글자 이상 입력 안 되게 지정  */
$(".phoneTest").on("input",function(){
	 if ($(this).val().length > 4) {
	    $(this).val( $(this).val().slice(0, 4));
	 }  
	
})


/* --------------------이미지 첨부---------------  */

//이미지 영역을 클릭할 때 파일 첨부 창이 뜨도록 설정하는 함수
// 페이지 로딩이 끝나고나면 #fileArea 요소를 숨김.
$(function(){
	$("#fileArea").hide(); 
	
	$(".hospitalImg").on("click",function(){// 이미지 영역이 클릭 되었을 때
		// 클릭한 이미지 영역 인덱스 얻어오기
		var index = $(".hospitalImg").index(this);
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
			$(".hospitalImg").eq(num).children("img").attr("src", e.target.result);
	    	// e.target.result : 이벤트가 발생한 요소의 결과 , 파일 읽기 동작을 성공한 요소가 읽어들인 파일 내용
	  
		 }
	  }
}


// 이미지 배치
		<c:forEach var="file" items="${fList}">
			$(".hospitalImg").eq(${file.fileLevel}).children("img").attr("src","${contextPath}/resources/image/uploadHospitalImages/${file.fileName}");
		</c:forEach>



/* ******초기값 지정***** */
//지역 초기값 지정
(function(){
	$("#location1>option").each(function(index,item){
		if($(item).val() == "${hospital.location1}"){
			$(item).prop("selected",true);
		}
	});
})();



// 번호 앞 부분 지정
(function(){
		// #phone1의 자식 중 option 태그들을 반복 접근
	$("#phone1 > option").each(function(index, item){
		// index : 현재 접근중인 인덱스
		// item : 현재 접근중인 요소
		
			// 현재 접근한 요소에 써져있는 값과 전화번호 배열의 첫번째 값이 같다면
		if( $(item).text() == "${phone[0]}"){
			// 현재 접근한 요소에 seleted라는 옵션을 추가
			$(item).prop("selected",true);
		}
	});
})();	



//*** 등록된 부대시설  체크하기 ***
(function(){

// 부대시설에서 문자열을 얻어와 ' , '를 구분자로 하여 분리하기
var facility = "${hospital.hospFacility}".split(",");

// 체크 박스 요소를 모두 선택하여 반복 접근
$("input[name='hosp_facility']").each(function(index, item){
   
   // interest 배열 내에
   // 현재 접근 중인 체크박스의 value와 일치하는 요소가 있는지 확인
   if(facility.indexOf( $(item).val()) != -1 ){
      $(item).prop("checked", true);
   }
});
})();

</script>
```


<br><br>


- /hospital/update 주소를 전달받는 Controller


```java
else if(command.equals("/update")) {
    errorMsg = "동물병원 수정 과정에서 오류 발생";
    
    // 1. MultipartRequest 객체 생성에 필요한 값 설정
    int maxSize = 20 * 1024 * 1024; // 최대 크기 20MB
    String root = request.getSession().getServletContext().getRealPath("/");
    String filePath = root + "resources/image/uploadHospitalImages/";
        
    // 2. MultipartRequest 객체 생성
    // -> 생성과 동시에 전달받은 파일이 서버에 저장됨
    MultipartRequest multiRequest = new MultipartRequest(request, filePath, maxSize, "UTF-8", new MyFileRenamePolicy());
        
    // 3. 파일 정보를 제외한 파라미터 얻어오기
    String location1 = multiRequest.getParameter("location1"); //지역1
    String hospNm = multiRequest.getParameter("hospNm"); // 병원명
    String phone1 = multiRequest.getParameter("phone1"); // 전화번호
    String phone2 = multiRequest.getParameter("phone2"); // 전화번호
    String phone3 = multiRequest.getParameter("phone3"); // 전화번호
    String phone = phone1+"-" + phone2 + "-" + phone3; // 전화번호 합치기
        
    String location2 = multiRequest.getParameter("location2"); //상세주소
    String openTime = multiRequest.getParameter("openTime"); //오픈시간
    String closeTime = multiRequest.getParameter("closeTime"); //오픈시간
        
    String[] facilityArr = multiRequest.getParameterValues("hosp_facility"); // 병원시설 배열로 받기
    String facility = null;
        
    if(facilityArr!=null) { //병원 시설 배열이 비어있지 않다면.
        facility= String.join(",", facilityArr);
    }
    String hospitalInfo = multiRequest.getParameter("hospital_info");
    
    int hospitalNo = Integer.parseInt(multiRequest.getParameter("hospitalNo"));
        
        
    // 4. 전달받은 파일 정보를 List에 저장
    List<Attachment> fList = new ArrayList<Attachment>();
    Enumeration<String> files = multiRequest.getFileNames();
    // input type="file"인 모든 요소의 name 속성 값을 반환받아 files에 저장
    
    while(files.hasMoreElements()) {
        // 현재 접근중인 name속성값읇 변수에 저장
            String name = files.nextElement();
            
            // 현재 name 속성이 일치하는 요소로 업로드된 파일이 있다면
            if(multiRequest.getFilesystemName(name) != null) {
                
            Attachment temp = new Attachment();
            
            // 변경된 파일 이름 temp에 저장
            temp.setFileName(multiRequest.getFilesystemName(name));
            
            // 지정한 파일 경로 tmep에 저장
            temp.setFilePath(filePath);
            
            // 해당 게시글 번호 temp에 저장
            temp.setHospNo(hospitalNo);
            
            // 파일 레벨 temp에 저장
            switch(name) {
            case "img0" : temp.setFileLevel(0); break;
            case "img1" : temp.setFileLevel(1); break;
            case "img2" : temp.setFileLevel(2); break;
            case "img3" : temp.setFileLevel(3); break;
            case "img4" : temp.setFileLevel(4); break;
            case "img5" : temp.setFileLevel(5); break;
            }
            // temp를 fList에 추가
            fList.add(temp);
            // 이미지를 변경한 부분들만 fList에 추가가 된다.
        }
    }
    
    // 5. Session에서 로그인한 회원의 번호를 얻어와 저장 (작성자)
    Member loginMember = (Member)request.getSession().getAttribute("loginMember");
    int memberNo = loginMember.getMemberNo(); 
    
    // 6. 준비된 값들을 하나의 Map에 저장
    Map<String,Object> map = new HashMap<String,Object>();
    
    map.put("fList",fList);
    map.put("hospitalNo",hospitalNo);
    map.put("location1", location1);
    map.put("hospNm", hospNm);
    map.put("phone", phone);
    map.put("location2", location2);
    map.put("openTime", openTime);
    map.put("closeTime", closeTime);
    map.put("facility", facility);
    map.put("hospitalInfo", hospitalInfo);
    map.put("memberNo", memberNo);
    
    
    // 7. 준비된 값을 매개변수로 하여 게시글 수정 Service 호출
    int result = service.updateHospital(map);
        
    // 8. result 값에 따라 View 연결 처리
    path ="view?cp="+cp+"&hospitalNo="+hospitalNo;
    String sk = multiRequest.getParameter("sk");
    String sv = multiRequest.getParameter("sv");
    
    // 전달된 sk,sv가 존재 할 때 (검색을 통한 접근일 때)
    if(sk != null && sv != null ) {
        path += "&sk="+sk + "&sv=" + sv;
    }

    if (result>0) {
        swalIcon = "success";
        swalTitle = "수정 성공";
    }else {
         swalIcon = "error";
        swalTitle = "수정 실패";
    }
    request.getSession().setAttribute("swalIcon", swalIcon);
    request.getSession().setAttribute("swalTitle", swalTitle);
        
    response.sendRedirect(path);
}
```
<br><br>


- 동물병원 수정 Service

```java
	public int updateHospital(Map<String, Object> map) throws Exception {
		Connection conn = getConnection();
		
		int result =0; // Service 수정 결과 저장할 변수
		
		List<Attachment> deleteFiles =null; // 삭제할 파일 정보 저장변수 선언
		
		 // 1. 크로스 사이트 스크립팅 방지 처리
		String hospNm = (String)map.get("hospNm");
		String hospitalInfo = (String)map.get("hospitalInfo");
		
		hospNm = replaceParameter(hospNm);
		hospitalInfo = replaceParameter(hospitalInfo);
		
		// 2. 내용에 개행문자 변경처리.
		
		hospitalInfo = hospitalInfo.replaceAll("\r\n", "<br>");
		
		// 처리된 내용을 다시 map에 추가
		map.put("hospNm",hospNm);
		map.put("hospitalInfo",hospitalInfo);
		
		try {
			// 3. 게시글 부분 수정 DAO 호출
			result = dao.updateHospital(conn,map);
			
			// 4. 게시글 수정이 성공하고 fList가 비어있지 않으면  파일 정보 수정 DAO를 호출함
			List<Attachment> newFileList = (List<Attachment>)map.get("fList");
					
			
			if(result>0 && !newFileList.isEmpty()) {
				//DB에서 해당 게시글의 수정 전 파일 목록 조회
				List<Attachment> oldFileList = dao.selectHospitalFiles(conn,(int)map.get("hospitalNo"));
				
				// newFileList -> 수정된 썸네일(lv.0)
	            // oldFileList -> 썸네일(lv.0), 이미지 (lv.1), 이미지2(lv.2)
	            
	            // 기존 썸네일(lv.0) -> 수정된 썸네일(lv.0)로 변경됨
	            // -> DB에서 기존 썸네일의 데이터를 수정된 썸네일로 변경
	            //   --> DB에서 기존 썸네일 정보가 사라짐
	            
				
				result =0; // 재활용
				deleteFiles = new ArrayList<Attachment>(); // 삭제될 파일정보 저장
				
				// 새로운 이미지 정보 반복 접근
				for(Attachment newFile : newFileList) {
		               
		               // flag가 false인 경우 : 새 이미지와 기존 이미지의 파일 레벨이 중복되는 경우 -> update
		               // flag가 true인 경우 : 새 이미지와 기존 이미지의 파일 레벨이 중복되지 않는 경우 -> insert
		               boolean flag = true;
		               
		               // 기존 이미지 정보 반복 접근
		               for(Attachment oldFile : oldFileList) {
		                  
		                  // 새로운 이미지와 기존 이미지의 파일 레벨이 동일한 파일이 있다면
		                  if(newFile.getFileLevel() == oldFile.getFileLevel()) {
		                     
		                     // 기존 파일을 삭제 List에 추가
		                     deleteFiles.add(oldFile);
		                     
		                     // 새 이미지 정보에 이전 파일 번호를 추가 -> 파일 번호를 이용한 수정 진행
		                     newFile.setFileNo(oldFile.getFileNo());
		                     
		                     flag = false;
		                     
		                     break;
		                  }
		               }
				
	            // flag 값에 따라 파일 정보 insert 또는 update수행
	               if(flag) {
	                  result = dao.insertAttachment(conn, newFile);
	               }else {
	                  result = dao.updateAttachment(conn, newFile);
	               }
	               
	               // 파일 정보 삽입 또는 수정 중 실패 시
	               if(result == 0) {
	                  // 강제로 사용자 정의 예외 발생
	                  throw new FileInsertFailedException("파일 정보 삽입 또는 수정 실패");
	               }
	            }
	         }
		
		} catch(Exception e) {
			
			 // 게시글 수정 중 실패 또는 오류 발생 시
	         // 서버에 미리 저장되어 있던 이미지 파일 삭제
	         List<Attachment> fList = (List<Attachment>)map.get("fList");
	         
	         if(!fList.isEmpty()) {
	            for(Attachment at : fList) {
	               String filePath = at.getFilePath();
	               String fileName = at.getFileName();
	               
	               File deleteFile = new File(filePath + fileName);
	               
	               if(deleteFile.exists()) {
	                  // 해당 경로에 해당 파일이 존재하면
	                  deleteFile.delete(); // 해당 파일 삭제
	               }
	            }
	         }
	         
	         // 에러페이지가 보여질 수 있도록 catch한 Exception을 Controller로 던져줌
	         throw e; 
	     }	
				
		 // 5. 트랜잭션 처리 및 삭제 목록에 있는 파일 삭제
	      if(result > 0) {
	         commit(conn);	
				
	         // DB 정보와 맞지 않는 파일(deleteFiles) 삭제 진행
	         if(deleteFiles !=null) {
	        	 
		         for(Attachment at : deleteFiles) {
		            String filePath = at.getFilePath();
		            String fileName = at.getFileName();
		            
		            File deleteFile = new File(filePath + fileName);
		            
		            if(deleteFile.exists()) {
		               deleteFile.delete();
		            }
		         }
	         }
	      }else {
	         rollback(conn);
	      }
	      return result;
	   }
```
<br><br>

- 동물병원 정보 수정 DAO

```java
public int updateHospital(Connection conn, Map<String, Object> map) throws Exception {
    int result =0;
    String query = prop.getProperty("updateHospital");
    
    try {
        pstmt = conn.prepareStatement(query);
        pstmt.setString(1,(String)map.get("hospNm"));
        pstmt.setString(2,(String)map.get("location1"));
        pstmt.setString(3,(String)map.get("location2"));
        pstmt.setString(4,(String)map.get("phone"));
        pstmt.setString(5,(String)map.get("openTime"));
        pstmt.setString(6,(String)map.get("closeTime"));
        pstmt.setString(7,(String)map.get("facility"));
        pstmt.setString(8,(String)map.get("hospitalInfo"));
        pstmt.setInt(9, (int)map.get("hospitalNo"));
        
        result = pstmt.executeUpdate();
        
    }finally{
        close(pstmt);
    }
    
    return result;
}
```

<br><br>

- 동물병원 정보 수정 sql문

```sql
<entry key="updateHospital">
UPDATE HOSPITAL SET
HOSP_NM =? , 
LOCATION1 =?, 
LOCATION2 =? ,
PHONE =?, 
OPENING_TIME=?,
CLOSING_TIME=?,
HOSP_FACILITY=?,
HOSP_INFO=?
WHERE HOSP_NO=?
</entry>
```

<br><br>
 
- 기존 파일 정보 조회 DAO

```java
public List<Attachment> selectHospitalFiles(Connection conn, int hospitalNo) throws Exception {
    List<Attachment> fList=null;
    String query = prop.getProperty("selectHospitalFiles");
    
    try {
        pstmt= conn.prepareStatement(query);
        pstmt.setInt(1, hospitalNo);
        
        rset = pstmt.executeQuery();
        
        fList = new ArrayList<Attachment>();
        
        while(rset.next()) {
            Attachment at = new Attachment(
                    rset.getInt("FILE_NO"),
                    rset.getString("IMG_NAME"),
                    rset.getInt("IMG_LEVEL"));
            
            at.setFilePath(rset.getString("IMG_PATH"));
            
            fList.add(at);
        }
        
    }finally{
        close(rset);
        close(pstmt);
    }
    return fList;
}
```


<br><br>

- 기존 파일 정보 조회 sql문

```sql
<entry key="selectHospitalFiles">
SELECT FILE_NO, IMG_NAME, IMG_LEVEL, IMG_PATH
FROM HOSPITAL_IMG
WHERE HOSP_NO = ?
ORDER BY IMG_LEVEL
</entry>
```


<br><br>

- 새로운 파일 추가 DAO

```java
public int insertAttachment(Connection conn, Attachment at) throws Exception {
    int result =0;
    
    String query = prop.getProperty("insertAttachment");
    
    try {
        pstmt = conn.prepareStatement(query);
        pstmt.setString(1, at.getFilePath());
        pstmt.setString(2, at.getFileName());
        pstmt.setInt(3, at.getFileLevel());
        pstmt.setInt(4, at.getHospNo());
        
        result = pstmt.executeUpdate();
    }finally {
        close(pstmt);
    }
    return result;
}
```

<br><br>

- 새로운 파일 추가 sql문

```sql
<entry key="insertAttachment">
INSERT INTO HOSPITAL_IMG
VALUES(SEQ_HOSIMG.NEXTVAL, ?, ?, ?, ?)
</entry>
```

<br><br>

- 기존 파일 정보 수정 DAO

```java
public int updateAttachment(Connection conn, Attachment newFile) throws Exception {
    int result =0;
    
    String query = prop.getProperty("updateAttachment");
    
    try {
        pstmt = conn.prepareStatement(query);
        pstmt.setString(1, newFile.getFilePath());
        pstmt.setNString(2, newFile.getFileName());
        pstmt.setInt(3, newFile.getFileNo());
        
        result = pstmt.executeUpdate();
    }finally {
        close(pstmt);
    }
    
    return result;
}
```

<br><br>

- 기존 파일 정보 수정 sql문

```sql
<entry key="updateAttachment">
UPDATE HOSPITAL_IMG SET
IMG_PATH = ?,
IMG_NAME = ?
WHERE FILE_NO =?
</entry>
```

<br><br>