---
title: "[숙소] 수정,삭제"
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


> 본인이 작성한 게시글에만 수정,삭제 버튼이 출력 됨.

- 출력화면

![image](https://user-images.githubusercontent.com/73421820/112613008-f5fefc80-8e62-11eb-97a8-edec647df546.png)


<br><br>


```java
<c:if
  test="${!empty loginMember && (loginMember.memberAdmin == 'C' && room.memNo == loginMember.memberNo) || loginMember.memberAdmin == 'A'}">
  <div class="row-item" style="margin-top: 50px;">
    <div class="btn_item">

      <%-- 게시글 수정 후 상세조회 페이지로 돌아오기 위한 url 조합 --%>
      <c:if test="${!empty param.sv && !empty param.sk }">
        <%-- 검색을 통해 들어온 상세 조회 페이지인 경우 --%>
        <c:set var="searchStr" value="&sk=${param.sk}&sv=${param.sv}" />
      </c:if>
      <a
        href="updateForm?cp=${param.cp}&roomNo=${param.roomNo}${searchStr}"
        class="btn_class" id="updateBtn">수정</a>
      <button class="btn_class" id="deleteBtn" type="button">삭제</button>
    </div>
  </div>
</c:if>
```


<br><br>


## [숙소] 삭제

- 삭제 버튼을 클릭했을 때 실행되는 script

```java
<script>
  $("#deleteBtn").on("click", function() {
    if (confirm("해당 병원을 삭제하시겠습니까?")) {
      location.href = "delete?roomNo=${param.roomNo}"
    }
  });
</script>
```

<br><br>

- /room/delete 요청 전달받는 controller

```java
else if(command.equals("/delete")) {
  errorMsg ="삭제 과정에서 오류 발생";
  
  int roomNo = Integer.parseInt(request.getParameter("roomNo"));
  int result = service.deleteRoom(roomNo);
  
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


- 숙소 삭제 Service

```java
public int deleteRoom(int roomNo) throws Exception {
  Connection conn = getConnection();
  
  int result = dao.deleteRoom(conn, roomNo);
  
  if(result>0) commit(conn);
  else 		 rollback(conn);
  
  close(conn);
  return result;
}
```

<br><br>

- 숙소 삭제 DAO

```java
public int deleteRoom(Connection conn, int roomNo) throws Exception {
  int result =0;
  String query = prop.getProperty("deleteRoom");
  
  try {
    pstmt = conn.prepareStatement(query);
    pstmt.setInt(1, roomNo);
    
    result = pstmt.executeUpdate();
  }finally {
    close(pstmt);
  }
  return result;
}
```

<br><br>

- 숙소 삭제 sql문

```sql
<entry key="deleteRoom">
UPDATE ROOM SET
ROOM_DEL_FL = 'Y'
WHERE ROOM_NO =?
</entry>
```
<br><br>



## [숙소] 수정

- 출력화면

![image](https://user-images.githubusercontent.com/73421820/112591782-4026b480-8e48-11eb-9d58-1e2ba1cad8dd.png)


<br><br>

- 수정 버튼 클릭시 수정 화면 전환 Controller

```java
else if(command.equals("/updateForm")) {
  errorMsg ="숙소 수정 화면 불러오는 과정에서 오류 발생";
  
  int roomNo = Integer.parseInt(request.getParameter("roomNo"));
  Room room = service.updateView(roomNo);
  
  if(room!=null) {
    
    List<Attachment> fList = service.selectRoomFiles(roomNo);
    
    if(!fList.isEmpty()) {
      request.setAttribute("fList", fList);
    }
    path = "/WEB-INF/views/room/roomUpdate.jsp";
    request.setAttribute("room", room);
    view = request.getRequestDispatcher(path);
    view.forward(request, response);
  }else {
    request.getSession().setAttribute("swalIcon", "error");
    request.getSession().setAttribute("swalTitle", "숙소 수정 화면 전환에 실패했습니다.");
    response.sendRedirect(request.getHeader("referer"));
  }
}
```
<br><br>

- 숙소 수정 화면 전환 시 숙소 정보 가져오는 Service

```java
public Room updateView(int roomNo) throws Exception {
  Connection conn = getConnection();
  
  Room room = dao.selectRoom(conn, roomNo);
  
  // 크로스 스크립팅 방지를 위해 개행문자가 <br>로 바뀌어 있는 상태 -> \r\n 으로 바꿈
  room.setRoomInfo(room.getRoomInfo().replaceAll("<br>", "\r\n"));
  
  close(conn);
  
  return room;
}
```
<br><br>

- 숙소 수정 화면 전환 시 숙소 정보 가져오는 DAO,sql 문은 숙소 상세조회랑 동일함.


<br><br>

- 선택된 숙소의 정보를 가져와서  화면에 뿌려준다. JSP

```html
<jsp:include page="/WEB-INF/views/common/otherHeader.jsp"></jsp:include>


<!-- 주소  -->
<c:set var="address" value="${fn:split(comMember.comAddress,',') }" />

<!-- 숙소 등록하기 -->
<div class="wrapper">
  <div class="main">

    <div class="row-item">
      <div id="page_name">숙소 수정</div>
      <hr id="hr_tag">
    </div>

    <c:if test="${!empty param.sk && !empty param.sv}">
      <c:set var="searchStr" value="&sk=${param.sk}&sv=${param.sv}" />
    </c:if>

    <div class="insert_room">
      <form
        action="${contextPath}/room/update?cp=${param.cp}&roomNo=${param.roomNo}${searchStr}"
        method="post" enctype="multipart/form-data" role="form"
        onsubmit="return roomUpdateValidate();">


        <div class="row-item">
          <div class="label_name">
            <label for="companyName"> <span class="highlighter">숙소명</span>
            </label>
          </div>
          <div class="input_tag" style="font-size: 20px; font-weight: bold;">
            ${comMember.comName}</div>
        </div>


        <div class="row-item">
          <div class="label_name">
            <label for="phone"> <span class="highlighter">전화번호</span>
            </label>
          </div>
          <div class="input_tag">${comMember.comPhone }</div>
        </div>


        <div class="row-item">
          <div class="label_name">
            <label for="location2"> <span class="highlighter">상세주소</span>
            </label>
          </div>
          <div class="input_tag">${address[1]}</div>
        </div>

        <div class="row-item">
          <div class="label_name">
            <label for="checkIn"> <span class="highlighter">체크인/체크아웃</span>
            </label>
          </div>
          <div class="input_tag">
            <input type="text" class="checkHours" id="checkIn" name="checkin"
              value="${room.checkin }" autocomplete="off" required>
            &nbsp;/ <input type="text" class="checkHours" id="checkOut"
              name="checkout" value="${room.checkout }" autocomplete="off"
              required>
          </div>
        </div>

        <div class="row-item">
          <div class="label_name">
            <label for="facility"> <span class="highlighter">숙소
                부대 시설</span>
            </label>
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
              name="room_info">${room.roomInfo }</textarea>
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
        <input type="file" id="img1" name="img1" onchange="LoadImg(this,1)"> 
          <input type="file" id="img2" name="img2" onchange="LoadImg(this,2)"> 
          <input	type="file" id="img3" name="img3" onchange="LoadImg(this,3)">
        <input type="file" id="img4" name="img4" onchange="LoadImg(this,4)"> 
          <input type="file" id="img5" name="img5" onchange="LoadImg(this,5)">
      </div>



        <!-- 수정 / 취소 버튼  -->
        <div class="row-item">
          <div class="btn_item">
            <button class="btn_class" id="updateBtn" type="submit">수정</button>
            <button class="btn_class" id="resetBtn" type="button">취소</button>
          </div>
        </div>
      </form>

    </div>

  </div>

</div>
<!-- wrapper -->
<jsp:include page="/WEB-INF/views/common/footer.jsp"></jsp:include>
```

- 숙소 수정 시 사용되는 script

```java
<script>
  function roomUpdateValidate(){
    
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

  // *** 등록된 부대시설  체크하기 ***
  (function(){
    
    // 부대시설에서 문자열을 얻어와 ' , '를 구분자로 하여 분리하기
    var facility = "${room.facility}".split(",");
    
    // 체크 박스 요소를 모두 선택하여 반복 접근
    $("input[name='facility']").each(function(index, item){
      // interest 배열 내에
      // 현재 접근 중인 체크박스의 value와 일치하는 요소가 있는지 확인
      if(facility.indexOf( $(item).val()) != -1 ){
        $(item).prop("checked", true);
      }
    });
  })();

  // *** 등록된 견종 체크하기
  (function(){
    
    // 부대시설에서 문자열을 얻어와 ' , '를 구분자로 하여 분리하기
    var dog = "${room.dog}".split(",");
    
    // 체크 박스 요소를 모두 선택하여 반복 접근
    $("input[name='dog']").each(function(index, item){
        
        // interest 배열 내에
        // 현재 접근 중인 체크박스의 value와 일치하는 요소가 있는지 확인
        if(dog.indexOf( $(item).val()) != -1 ){
          $(item).prop("checked", true);
        }
    });
  })();



  /* 취소 버튼이 눌리면 확인창이 뜬다.  */
  $("#resetBtn").on("click",function(){
    if( confirm("수정을 취소합니다.")){
      location.href = "${header.referer}";
    }
  });

    
    
  /* --------------------이미지 첨부---------------  */

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

// 이미지 배치
<c:forEach var="file" items="${fList}">
		$(".roomImg").eq(${file.fileLevel}).children("img").attr("src","${contextPath}/resources/image/uploadRoomImages/${file.fileName}");
</c:forEach>

</script>

```

<br><br>


- /room/update 주소를 전달받는 Controller

```java
else if(command.equals("/update")) {
  errorMsg = "숙소 수정 과정에서 오류 발생";
  
  // 1. MultipartRequest 객체 생성에 필요한 값 설정
  int maxSize = 20 * 1024 * 1024; // 최대 크기 20MB
  String root = request.getSession().getServletContext().getRealPath("/");
  String filePath = root + "resources/image/uploadRoomImages/";
  
  // 2. MultipartRequest 객체 생성
  // -> 생성과 동시에 전달받은 파일이 서버에 저장됨
  MultipartRequest multiRequest = new MultipartRequest(request, filePath, maxSize, "UTF-8", new MyFileRenamePolicy());
  
  // 3.파일정보를 제외한 게시글 정보를 얻어와 저장하기
      
  String checkin = multiRequest.getParameter("checkin");
  String checkout = multiRequest.getParameter("checkout");
  
  String[] facilityArr = multiRequest.getParameterValues("facility");
  String facility = null;
  if(facilityArr!=null) { // 숙소 시설 배열이 비어있지 않다면.
    facility= String.join(",", facilityArr);
  }
  
  
  String[] dogArr = multiRequest.getParameterValues("dog");
  String dog = null;
  if(dogArr!=null) { // 견종이 비어있지 않다면.
    dog= String.join(",", dogArr);
  }
  
  
  String roomInfo = multiRequest.getParameter("room_info");
  
  int roomNo = Integer.parseInt(multiRequest.getParameter("roomNo"));
  
  // 4. 전달받은 파일 정보를 List에 저장
  List<Attachment> fList = new ArrayList<Attachment>();
  Enumeration<String> files = multiRequest.getFileNames();
  // input type="file"인 모든 요소의 name 속성 값을 반환받아 files에 저장
  
  
  while(files.hasMoreElements()) {
    // 현재 접근중인 name속성값을 변수에 저장
      String name = files.nextElement();
          
        // 현재 name 속성이 일치하는 요소로 업로드된 파일이 있다면
        if(multiRequest.getFilesystemName(name) != null) {
          
        Attachment temp = new Attachment();
        
        // 변경된 파일 이름 temp에 저장
        temp.setFileName(multiRequest.getFilesystemName(name));
        
        // 지정한 파일 경로 tmep에 저장
        temp.setFilePath(filePath);
        
        // 해당 게시글 번호 temp에 저장
        temp.setRoomNo(roomNo);
        
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
  
  // 5. Session에서 로그인한 회원 번호를 얻어와 저장(작성자)
  Member loginMember = (Member)request.getSession().getAttribute("loginMember");
  int memberNo = loginMember.getMemberNo(); 
  
  // 6. 준비된 값들을 하나의 Map에 저장
  Map<String,Object> map = new HashMap<String,Object>();
  map.put("fList",fList);
  map.put("checkin", checkin);
  map.put("checkout", checkout);
  map.put("facility", facility);
  map.put("dog", dog);
  map.put("roomInfo", roomInfo);
  map.put("memberNo", memberNo);
  map.put("roomNo", roomNo);
  
  // 7. 준비된 값을 매개변수로 하여 게시글 수정 Service 호출
    int result = service.updateRoom(map);
    
  // 8. result 값에 따라 View 연결 처리
    path ="view?cp="+cp+"&roomNo="+roomNo;
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

- 숙소 수정 Service


```java
public int updateRoom(Map<String, Object> map) throws Exception {
  Connection conn = getConnection();
  
  int result =0;
  
  List<Attachment> deleteFiles = null; // 삭제할 파일 정보 저장
  
  // 크로스 사이트 스크립팅 방지
  String roomInfo = (String)map.get("roomInfo");
  
  roomInfo = replaceParameter(roomInfo);
  
  // 내용에 개행문자 변경처리.
  
  roomInfo = roomInfo.replaceAll("\r\n", "<br>");
  
  // 처리된 내용을 다시 map에 추가
  map.put("roomInfo",roomInfo);
  
  try {
    
    // 수정 DAO
    result = dao.updateRoom(conn,map);
    
    List<Attachment> newFileList = (List<Attachment>)map.get("fList");
  
    if(result>0 && !newFileList.isEmpty()) {
      List<Attachment> oldFileList = dao.selectRoomFiles(conn, (int)map.get("roomNo"));
      
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
    
    
  }catch(Exception e) {

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

- 숙소 정보 수정 DAO

```java
public int updateRoom(Connection conn, Map<String, Object> map) throws Exception {
  int result =0;
  String query = prop.getProperty("updateRoom");
  
  try {
    pstmt = conn.prepareStatement(query);
    pstmt.setString(1, (String)map.get("roomInfo"));
    pstmt.setString(2, (String)map.get("checkin"));
    pstmt.setString(3, (String)map.get("checkout"));
    pstmt.setString(4, (String)map.get("facility"));
    pstmt.setString(5, (String)map.get("dog"));
    pstmt.setInt(6, (int)map.get("roomNo"));
    
    result = pstmt.executeUpdate();
  }finally {
    close(pstmt);
  }
  return result;
}
```

<br><br>

- 숙소 정보 수정 sql 문

```sql
<entry key="updateRoom">
UPDATE ROOM SET
ROOM_INFO = ?,
CHECKIN=?,
CHECKOUT=?,
FACILITY=?,
DOG=?
WHERE ROOM_NO =?
</entry>
```

<br><br>

- 숙소 기존 파일 정보 가져오는 DAO

```java
public List<Attachment> selectRoomFiles(Connection conn, int roomNo) throws Exception {
  List<Attachment> fList = null;
  String query = prop.getProperty("selectRoomFiles");
  
  try {
    pstmt = conn.prepareStatement(query);
    pstmt.setInt(1,roomNo);
    
    rset = pstmt.executeQuery();
    
    fList = new ArrayList<Attachment>();
    
    while(rset.next()) {
      Attachment at = new Attachment( rset.getInt("FILE_NO"),
                      rset.getString("FILE_NAME"),
                      rset.getInt("FILE_LEVEL"));
      
        at.setFilePath(rset.getString("FILE_PATH") );
      
      fList.add(at);
    }
    
  }finally {
    close(rset);
    close(pstmt);
  }
  return fList;
}
```

<br><br>

- 숙소 기존 파일 정보 가져오는 sql문

```sql
<entry key="selectRoomFiles">
SELECT FILE_NO, FILE_NAME, FILE_LEVEL , FILE_PATH
FROM ROOM_IMG
WHERE ROOM_NO = ?
ORDER BY FILE_LEVEL
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
    pstmt.setInt(4, at.getRoomNo());
    
    result = pstmt.executeUpdate();
  }finally {
    close(pstmt);
  }
  return result;
}
```

<br><br>

- 새로운 파일 추가 sql 문

```sql
<entry key="insertAttachment">
INSERT INTO ROOM_IMG
VALUES(SEQ_ROOMIMG.NEXTVAL,?,?,?,?)
</entry>
```

<br><br>

- 파일 정보 수정 DAO

```java
public int updateAttachment(Connection conn, Attachment newFile) throws Exception {
  int result =0;
  
  String query = prop.getProperty("updateAttachment");
  try {
    pstmt = conn.prepareStatement(query);
    pstmt.setString(1, newFile.getFilePath());
    pstmt.setString(2, newFile.getFileName());
    pstmt.setInt(3, newFile.getFileNo());
    
    result = pstmt.executeUpdate();
  }finally {
    close(pstmt);
  }
  return result;
}
```

<br><br>


- 파일 정보 수정 sql문

```sql
<entry key="updateAttachment">
UPDATE ROOM_IMG SET
FILE_PATH =?,
FILE_NAME=?
WHERE FILE_NO =?
</entry>
```

<br><br>
