---
title: "[숙소] 상세조회"
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

## [숙소] 상세조회

- 출력화면

![image](https://user-images.githubusercontent.com/73421820/112588808-626a0380-8e43-11eb-87cf-d1d3a4e26cc3.png)

<br><br>

> 숙소 목록에서 원하는 숙소를 클릭하면 해당 숙소의 상세조회 페이지가 열린다.<br>
  해당 숙소에 첨부된 이미지와, 숙소의 정보, 조회수가 출력되며 <br>
  숙소 등록 시 체크한 출입가능 견종, 부대시설에 따라 출력된다. <br>
  숙소의 상세주소에 따라 지도 API가 출력된다.<br><br>


- 숙소 목록 조회 JSP의 상세조회 script

```java
$(".numberSelect").on("click",function() {
    var roomNo = $(this).children("span#boardNo").text();

    var url = "${contextPath}/room/view?cp=${pInfo.currentPage}&roomNo="+roomNo + "${searchStr}";

    location.href = url;
});
```

<br><br>

- /room/view 요청을 받는 controller

```java
else if(command.equals("/view")) {
    errorMsg = "숙소 상세 조회 과정에서 오류 발생";
    
    int roomNo = Integer.parseInt(request.getParameter("roomNo"));
    
    // 상세조회 비즈니스 로직 수행 후 결과 반환받기
    Room room = service.selectRoom(roomNo);
    
    if(room!=null) {
        // 상세조회 성공 시 (파일목록)
        
        // 해당 게시글에 포함된 이미지 파일 목록 조회 서비스 호출
        List<Attachment> fList = service.selectRoomFiles(roomNo);
        
        if(!fList.isEmpty()) { // 해당  숙소 이미지 정보가 DB에 있을 경우
            request.setAttribute("fList", fList);
        }
        
        path ="/WEB-INF/views/room/roomView.jsp";
        request.setAttribute("room", room);
        view = request.getRequestDispatcher(path);
        view.forward(request, response);
    }else {
        request.getSession().setAttribute("swalIcon", "error");
        request.getSession().setAttribute("swalTitle", "숙소 상세 조회 실패");
        response.sendRedirect("list");
    }
}
```

<br><br>

- 해당 숙소 정보 받아오는 Service

```java
public Room selectRoom(int roomNo) throws Exception {
    Connection conn = getConnection();
    
    Room room = dao.selectRoom(conn, roomNo);
    
    // 상세조회가 성공하면
    if(room!=null) {
        int result = dao.increaseReadCount(conn,roomNo);
        
        if(result>0) {
            commit(conn);
            room.setViewCount(room.getViewCount()+1);
        }
        else  rollback(conn);
    }
    
    close(conn);
    return room;
}
```

<br><br>

- 해당 숙소 정보 받아오는 DAO

```java
public Room selectRoom(Connection conn, int roomNo) throws Exception {
    Room room = null;
    String query = prop.getProperty("selectRoom");
    
    try {
        pstmt=conn.prepareStatement(query);
        pstmt.setInt(1, roomNo);
        rset = pstmt.executeQuery();
        
        if(rset.next()) {
            room = new Room();
            room.setRoomName(rset.getString("ROOM_NAME"));
            room.setLocation2(rset.getString("LOCATION2"));
            room.setPhone(rset.getString("PHONE"));
            room.setRoomInfo(rset.getString("ROOM_INFO"));
            room.setCheckin(rset.getString("CHECKIN"));
            room.setCheckout(rset.getString("CHECKOUT"));
            room.setFacility(rset.getString("FACILITY"));
            room.setDog(rset.getString("DOG"));
            room.setViewCount(rset.getInt("VIEW_COUNT"));
            room.setMemNo(rset.getInt("MEM_NO"));
        }
        
    }finally {
        close(rset);
        close(pstmt);
    }
    return room;
}
```

<br><br>

- 해당 숙소 정보 받아올 때 사용되는 sql문

```sql
<entry key="selectRoom">
SELECT ROOM_NAME,LOCATION2,PHONE,ROOM_INFO,CHECKIN,CHECKOUT,FACILITY,DOG,VIEW_COUNT,MEM_NO
FROM ROOM
WHERE ROOM_NO=?
AND ROOM_DEL_FL ='N'
</entry>
```

<br><br>

- 상세조회 성공 시 조회수 증가 DAO

```java
public int increaseReadCount(Connection conn, int roomNo) throws Exception {
    int result =0;
    
    String query = prop.getProperty("increaseReadCount");
    
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

- 조회 수 증가 sql문

```sql
<entry key="increaseReadCount">
UPDATE ROOM SET
VIEW_COUNT = VIEW_COUNT +1
WHERE ROOM_NO =?
</entry>
```

<br><br>

- 상세조회 성공 시 첨부된 이미지 조회 Service

```java
public List<Attachment> selectRoomFiles(int roomNo) throws Exception {
    Connection conn = getConnection();
    List<Attachment> fList = dao.selectRoomFiles(conn,roomNo);
    close(conn);
    return fList;
}
```

<br><br>

- 상세조회 성공 시 첨부된 이미지 조회 DAO

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

- 상세조회 성공 시 첨부된 이미지 조회 sql문

```java
<entry key="selectRoomFiles">
SELECT FILE_NO, FILE_NAME, FILE_LEVEL , FILE_PATH
FROM ROOM_IMG
WHERE ROOM_NO = ?
ORDER BY FILE_LEVEL
</entry>
```

<br><br>



- 상세조회 JSP

```java
<jsp:include page="/WEB-INF/views/common/otherHeader.jsp"></jsp:include>
<div class="wrapper">
    <c:choose>
        <c:when test="${!empty fList }">

            <div class="carousel slide boardImgArea imageArea" id="room-image">

                <!-- 이미지 선택 버튼 -->
                <ol class="carousel-indicators ">
                    <c:forEach var="file" items="${fList}" varStatus="vs">

                        <li data-slide-to="${vs.index }" data-target="#room-image"
                            <c:if test="${vs.first}"> class="active" </c:if>></li>
                    </c:forEach>
                </ol>


                <!-- 출력되는 이미지 -->
                <div class="carousel-inner ">
                    <c:forEach var="file" items="${fList}" varStatus="vs">
                        <div
                            class="carousel-item imageArea <c:if test="${vs.first}">active</c:if>">
                            <img class="d-block w-100 imageArea boardImg"
                                id="${file.fileNo}"
                                src="${contextPath}/resources/image/uploadRoomImages/${file.fileName}">
                        </div>
                    </c:forEach>
                </div>

                <!-- 좌우 화살표 -->
                <a class="carousel-control-prev" href="#room-image"
                    data-slide="prev"> <span class="carousel-control-prev-icon"></span>
                    <span class="sr-only">Previous</span>
                </a> <a class="carousel-control-next" href="#room-image"
                    data-slide="next"> <span class="carousel-control-next-icon"></span>
                    <span class="sr-only">Next</span>
                </a>
            </div>
        </c:when>

        <c:otherwise>
            <div class="imageArea">
                <img src="${contextPath}/resources/image/icon/nonImage.png">
            </div>
        </c:otherwise>

    </c:choose>



    <!-- 숙소 이름 -->
    <div class="row-item">
        <p id="roomName">${room.roomName }</p>
    </div>

    <!-- 조회수-->
    <div class="row-item">
        <div class="viewInfo iconArea" style="margin-left: 1020px;">
            <span><img src="${contextPath}/resources/image/icon/view.png"
                class="icon"></span>
            <div class="count">${room.viewCount }</div>
        </div>
    </div>

    <!-- 숙소 주소 -->
    <div class="row-item">
        <span><img class="icon"
            src="${contextPath}/resources/image/icon/site.png"></span> <span
            id="roomAddress">숙소 주소 : ${room.location2 } </span>
    </div>

    <!-- 숙소 전화번호 -->
    <div class="row-item">
        <span><img class="icon"
            src="${contextPath}/resources/image/icon/phone.png"></span> <span
            id="roomPhone">전화번호 : ${room.phone } </span>
    </div>

    <!-- 숙소 체크인/체크아웃시간 -->
    <div class="row-item">
        <span><img class="icon"
            src="${contextPath}/resources/image/icon/clock.png"></span> <span
            id="roomCheckIn">체크인 : ${room.checkin} </span>
    </div>
    <div class="row-item">
        <span><img class="icon"
            src="${contextPath}/resources/image/icon/clock2.png"></span> <span
            id="roomCheckOut">체크아웃 : ${room.checkout} </span>
    </div>

    <div class="row-item">
        <span><img class="icon"
            src="${contextPath}/resources/image/icon/dog.png"></span> <span
            id="roomCheckIn">출입가능 견종 : ${room.dog } </span>
    </div>

    <hr>



    /* 숙소 등록 시 체크한 부대시설에 맞게 아이콘 출력 */
    <c:set var="facilityArr" value="${fn:split(room.facility, ',') }" />

    <div class="row-item" style="margin: 0;">

        <c:forEach var="facility" items="${facilityArr }">
            <c:choose>
                <c:when test="${facility == 'WiFi'  }">
                    <div class="facility">
                        <div class="icon_area">
                            <img class="facility_icon"
                                src="${contextPath}/resources/image/icon/WiFi.png">
                        </div>
                        <div class="text_area">WiFi</div>
                    </div>
                </c:when>

                <c:when test="${facility == '주차장'  }">
                    <div class="facility">
                        <div class="icon_area">
                            <img class="facility_icon"
                                src="${contextPath}/resources/image/icon/park.png">
                        </div>
                        <div class="text_area">주차장</div>
                    </div>
                </c:when>

                <c:when test="${facility == '수영장'  }">
                    <div class="facility">
                        <div class="icon_area">
                            <img class="facility_icon"
                                src="${contextPath}/resources/image/icon/pool.png">
                        </div>
                        <div class="text_area">수영장</div>
                    </div>
                </c:when>

                <c:when test="${facility == 'BBQ' }">
                    <div class="facility">
                        <div class="icon_area">
                            <img class="facility_icon"
                                src="${contextPath}/resources/image/icon/BBQ.png">
                        </div>
                        <div class="text_area">BBQ</div>
                    </div>
                </c:when>

                <c:when test="${facility == '마당'  }">
                    <div class="facility">
                        <div class="icon_area">
                            <img class="facility_icon"
                                src="${contextPath}/resources/image/icon/yard.png">
                        </div>
                        <div class="text_area">마당</div>
                    </div>
                </c:when>
            </c:choose>
        </c:forEach>
    </div>



    <hr style="margin-bottom: 15px;">

    <div class="row-item">
        <span class="highlighter">숙소 정보</span>

        <div style="font-size: 15px;">${room.roomInfo }</div>
    </div>
    <hr style="margin-bottom: 15px;">


    <div class="row-item">

        <span class="highlighter">상세위치</span>

        <div id="map" style="height: 400px;">map</div>
    </div>


    <div class="row-item">

        <c:choose>
            <c:when test="${!empty param.sk && !empty param.sv }">
                <c:url var="goToList" value="../room/search">
                    <!--../ : 상위 주소로 이동 <상대경로>  -->
                    <c:param name="cp">${param.cp }</c:param>
                    <c:param name="sk">${param.sk }</c:param>
                    <c:param name="sv">${param.sv }</c:param>
                </c:url>
            </c:when>

            <c:otherwise>
                <c:url var="goToList" value="/room/list">
                    <c:param name="cp">${param.cp}</c:param>
                </c:url>
            </c:otherwise>
        </c:choose>


        <a href="${goToList }" class="btn_class" id="back">돌아가기</a>

    </div>


    <!-- 업체/관리자만 보이는 버튼 -->
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
</div>
<!-- wrapper -->

<jsp:include page="/WEB-INF/views/common/footer.jsp"></jsp:include>
```

<br><br>


- 카카오 지도 API

```html

<script type="text/javascript"
	src="//dapi.kakao.com/v2/maps/sdk.js?appkey=앱키&libraries=services,clusterer,drawing"></script>


<script>
    var mapContainer = document.getElementById('map'), // 지도를 표시할 div 
    mapOption = {
        center : new kakao.maps.LatLng(33.450701, 126.570667), // 지도의 중심좌표
        level : 3 // 지도의 확대 레벨
    };

    // 지도 생성  
    var map = new kakao.maps.Map(mapContainer, mapOption);
    // 주소-좌표 변환 객체 생성
    var geocoder = new kakao.maps.services.Geocoder();

    // 주소로 좌표 검색
    geocoder.addressSearch('${room.location2 }',function(result, status) {

        // 정상적으로 검색이 완료됐으면 
        if (status === kakao.maps.services.Status.OK) {
            var coords = new kakao.maps.LatLng(result[0].y, result[0].x);

            // 결과값으로 받은 위치를 마커로 표시합니다
            var marker = new kakao.maps.Marker({
                map : map,
                position : coords
            });

        // 인포윈도우로 장소에 대한 설명을 표시합니다
        var infowindow = new kakao.maps.InfoWindow(
                {
                    content : '<div style="font-size: 13px;width:150px;text-align:center;padding:6px 0;">${room.roomName }</div>'
                });
        infowindow.open(map, marker);

        // 지도의 중심을 결과값으로 받은 위치로 이동시킵니다
        map.setCenter(coords);
    } else {
        console.log(result);
    }
});
</script>
```

<br><br>


- CSS

```css
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
	margin-bottom: 15px;
	display: inline-block;
}

/* 이미지 */
.imageArea {
	width: 100%;
	height: 450px;
	text-align: center;
	margin-botton: 30px;
}

/* 숙소 이름 */
#roomName {
	margin-top: 10px;
	text-align: center;
	font-size: 30px;
	font-weight: bold;;
}

.viewInfo {
	display: inline-block;
}

.icon {
	width: 20px;
	height: 20px;
	vertical-align: -4px;
	margin-right: 5px;
}

.count {
	display: inline-block;
	width: 50px;
	font-size: 13px;;
}

/* 시설 출력 */
.facility {
	width: 50px;
	height: 70px;
	display: inline-block;
	margin: 5px 20px 5px 0;
}

.icon_area {
	width: 100%;
	height: 50px;
	vertical-align: center;
}

.facility_icon {
	width: 100%;
	height: 100%;
}

.text_area {
	height: 20px;
	font-size: 11px;
	text-align: center;
}

/* 지도 API */
.map {
	width: 100%;
	height: 400px;
	text-align: center;
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
}

.btn_item {
	margin-left: 955px;
}

.btn_class:hover {
	background-color: #17a2b8;
}

/* 이전으로 버튼  */
#back {
	display: block;
	width: 90px;
	text-align: center;
	color: #fff;
	line-height: 35px;
	margin: auto;
	margin-top: 20px;
}

/* 수정버튼  */
#updateBtn {
	display: inline-block;
	text-align: center;
	line-height: 35px;
	float: right;
}

/* 형광펜  */
.highlighter {
	display: block;
	width: 60px;
	background: linear-gradient(to top, #ffc75f 100%, transparent 100%);
	font-size: 14px;
	text-align: center;
	margin-bottom: 15px;
}

/* 이미지 */
.carousel-control-prev-icon {
	background-image:
		url("data:image/svg+xml;charset=utf8,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='%23000' viewBox='0 0 8 8'%3E%3Cpath d='M5.25 0l-4 4 4 4 1.5-1.5-2.5-2.5 2.5-2.5-1.5-1.5z'/%3E%3C/svg%3E")
		!important;
}

.carousel-control-next-icon {
	background-image:
		url("data:image/svg+xml;charset=utf8,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='%23000' viewBox='0 0 8 8'%3E%3Cpath d='M2.75 0l-1.5 1.5 2.5 2.5-2.5 2.5 1.5 1.5 4-4-4-4z'/%3E%3C/svg%3E")
		!important;
}
```