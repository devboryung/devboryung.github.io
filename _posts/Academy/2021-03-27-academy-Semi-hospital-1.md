---
title: "[동물병원] 상세조회"
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

## [동물병원] 상세조회

- 출력화면 <br>

![image](https://user-images.githubusercontent.com/73421820/112722751-9bdb6580-8f4e-11eb-87a7-78d8253f85ac.png)

<br><br>

> 원하는 동물 병원을 클릭하면 해당 병원의 상세조회 페이지가 열린다.<br>
  해당 병원에 첨부된 이미지와, 조회수가 나타나며<br>
  병원 등록 시 체크한 편의시설 및 정보가 출력된다. <br>
  병원의 상세주소에 따라 지도 API가 출력된다.<br><br>


- 병원 목록에서 상세조회로 넘어가는 script

```java
$(".numberSelect").on("click", function(){
	var hospitalNo = $(this).siblings("span").text();
	
	var url = "${contextPath}/hospital/view?cp=${pInfo.currentPage}&hospitalNo="+ hospitalNo +"${searchStr}";
	
	location.href=url;
});
```

<br><br>

- /hospital/view 요청을 전달받는 controller

```java
else if(command.equals("/view")) {
    errorMsg = "동물병원 상세 조회 과정에서 오류 발생";
    
    int hospitalNo = Integer.parseInt(request.getParameter("hospitalNo"));
    
    // 상세조회 비즈니스 로직 수행 후 결과 반환받기
    Hospital hospital = service.selectHospital(hospitalNo);
    
    if(hospital!=null) { // 상세조회 성공 시
        
        // 해당 게시글에 포함된 이미지 파일 목록 조회 서비스 호출
        List<Attachment> fList = service.selectHospitalFiles(hospitalNo);
        
        if(!fList.isEmpty()) { // 해당 동물병원 이미지 정보가 DB에 있을 경우
            request.setAttribute("fList", fList);
        }
                                                
        
        path ="/WEB-INF/views/hospital/hospitalView.jsp";
        request.setAttribute("hospital", hospital);
        view = request.getRequestDispatcher(path);
        view.forward(request, response);
    }else {
        request.getSession().setAttribute("swalIcon", "error");
        request.getSession().setAttribute("swalTitle", "동물병원 상세 조회 실패");
        response.sendRedirect("list");
    }
    
}
```

<br><br>

- 동물 병원 상세조회 Service

```java
	public Hospital selectHospital(int hospitalNo) throws Exception {
		Connection conn = getConnection();
		
		Hospital hospital = dao.selectHospital(conn, hospitalNo);
				
		// 상세조회가 성공하면
		if(hospital != null) {
			
			
        //조회 수 증가
		int result = dao.increaseReadCount(conn,hospitalNo);
			
		if(result>0) {
			commit(conn);
        // 반환되는 병원 데이터에도 조회수를 1 추가해준다.
			hospital.setViewCount(hospital.getViewCount() +1);
		}
			else rollback(conn);
		}
		
		close(conn);
		return hospital;
	}
```

<br><br>


- 동물 병원 상세조회 DAO

```java
public Hospital selectHospital(Connection conn, int hospitalNo) throws Exception {
    Hospital hospital = null;
    
    String query = prop.getProperty("selectHospital");
    
    try {
        pstmt=conn.prepareStatement(query);
        pstmt.setInt(1, hospitalNo);
        rset = pstmt.executeQuery();
        
        if(rset.next()) {
            hospital = new Hospital();
            hospital.setHospNm(rset.getString("HOSP_NM"));
            hospital.setLocation1(rset.getString("LOCATION1"));
            hospital.setLocation2(rset.getString("LOCATION2"));
            hospital.setPhone(rset.getString("PHONE"));
            hospital.setOpeningTime(rset.getString("OPENING_TIME"));
            hospital.setClosingTime(rset.getString("CLOSING_TIME"));
            hospital.setHospInfo(rset.getString("HOSP_INFO"));
            hospital.setViewCount(rset.getInt("VIEW_COUNT"));
            hospital.setHospFacility(rset.getString("HOSP_FACILITY"));
            
        }
    }finally {
        close(rset);
        close(pstmt);
    }
    return hospital;
}
```

<br><br>

- 동물 병원 상세조회 sql문

```sql
<entry key="selectHospitalList">
SELECT * FROM 
	(SELECT ROWNUM RNUM, H.*
	 FROM (SELECT * FROM HOSPITAL WHERE HOSP_DEL_FL='N' ORDER BY HOSP_NO DESC)H)
WHERE  RNUM BETWEEN ? AND ?
</entry>
```
<br><br>

- 해당 동물 병원 조회수 증가 DAO

```java
public int increaseReadCount(Connection conn, int hospitalNo)throws Exception {
    int result =0;
    
    String query = prop.getProperty("increaseReadCount");
    
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

- 조회수 증가 sql문

```sql
<entry key="increaseReadCount">
UPDATE HOSPITAL SET
VIEW_COUNT = VIEW_COUNT +1
WHERE HOSP_NO = ?
</entry>
```

<br><br>


- 해당 동물 병원의 첨부 파일 조회 Service

```java
public List<Attachment> selectHospitalFiles(int hospitalNo) throws Exception {
    Connection conn = getConnection();
    
    List<Attachment> fList = dao.selectHospitalFiles(conn,hospitalNo);
    
    close(conn);
            
    return fList;
}
```

<br><br>

- 해당 동물 병원의 첨부 파일 조회 DAO

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

- 첨부파일 조회 sql문

```sql
<entry key="selectHospitalFiles">
SELECT FILE_NO, IMG_NAME, IMG_LEVEL, IMG_PATH
FROM HOSPITAL_IMG
WHERE HOSP_NO = ?
ORDER BY IMG_LEVEL
</entry>
```

<br><br>


- 상세조회 JSP

```html
<jsp:include page="/WEB-INF/views/common/otherHeader.jsp"/>

<div class="wrapper">
    <!-- 이미지 출력 -->
    <c:choose>
        <c:when test="${!empty fList }">
            <div class="carousel slide boardImgArea imageArea" id="hospital-image">
                
                <!-- 이미지 선택 버튼 -->
                <ol class="carousel-indicators ">
                    <c:forEach var="file" items="${fList}" varStatus="vs">
                        
                        <li data-slide-to="${vs.index }" data-target="#hospital-image"  
                            <c:if test="${vs.first}"> class="active" </c:if> >
                        </li>
                    </c:forEach>
                </ol>
                
                
                <!-- 출력되는 이미지 -->
                <div class="carousel-inner ">
                    <c:forEach var="file" items="${fList}" varStatus="vs">
                        <div class="carousel-item imageArea <c:if test="${vs.first}">active</c:if>">
                        <img class="d-block w-100 imageArea boardImg" id="${file.fileNo}" 
                            src="${contextPath}/resources/image/uploadHospitalImages/${file.fileName}">
                        </div>
                    </c:forEach>
                
                </div> 
                
                <!-- 좌우 화살표 -->
                <a class="carousel-control-prev" href="#hospital-image" data-slide="prev">
                    <span class="carousel-control-prev-icon"></span> <span class="sr-only">Previous</span>
                </a> 
                <a class="carousel-control-next" href="#hospital-image" data-slide="next">
                    <span class="carousel-control-next-icon"></span> <span class="sr-only">Next</span>
                </a>
            </div>
        </c:when>
        
        <c:otherwise>
            <div class="imageArea" >
                <img  src="${contextPath}/resources/image/icon/nonImage.png">
            </div>
        </c:otherwise>
    </c:choose>
    

    <!-- 동물병원 이름 -->
    <div class="row-item" >
        <p id="hospitalName">${hospital.hospNm }</p>
    </div> 

    <!-- 조회수 -->
    <div class="row-item">
        <div class="viewInfo iconArea" style="margin-left: 1020px;">
            <span><img src="${contextPath}/resources/image/icon/view.png" class="icon" style="margin-right: 0px;"></span>
            <div class="count">${hospital.viewCount }</div><!-- 최대 999,999 -->
        </div>
    </div>
    
    <!-- 동물병원 주소 -->
    <div class="row-item" >
        <span><img class="icon" src="${contextPath}/resources/image/icon/site.png"></span>
        <span id="hospitalAddress">${hospital.location2 } </span>
    </div> 
    
    
    

    <!-- 동물병원 전화번호 -->
    <div class="row-item" >
        <span><img class="icon" src="${contextPath}/resources/image/icon/phone.png"></span>
        <span id="hospitalPhone">전화번호 : ${hospital.phone } </span>
    </div> 

    <!-- 동물병원 운영시간 -->
    <div class="row-item" >
        <span><img class="icon" src="${contextPath}/resources/image/icon/clock.png"></span>
        <span id="hospitalHours">운영시간 : ${hospital.openingTime } ~ ${hospital.closingTime }</span>
    </div> 

    <hr>




    <!-- 부대시설 출력  -->
    <%-- 부대시설을  구분자를 이용하여 분리된 배열 형태로 저장 --%>
    <c:set var="facilityArr" value="${fn:split(hospital.hospFacility,',') }"/>
    <!-- ${facility[0]}  : WiFi  -->

    <div class="row-item" style="margin:0;">
    
    <c:forEach var="facility" items="${facilityArr }">
        <c:choose>
                <c:when test="${facility == 'WiFi' }">
                    <div class="facility">
                        <div class="icon_area">
                            <img class="facility_icon" src="${contextPath}/resources/image/icon/WiFi.png">
                        </div>
                        <div class="text_area"> 
                            WiFi
                        </div>
                    </div>
                </c:when>
                
                <c:when test="${facility == '주차' }">    
                    <div class="facility">
                        <div class="icon_area">
                            <img class="facility_icon" src="${contextPath}/resources/image/icon/park.png">
                        </div>
                        <div class="text_area"> 
                            주차
                        </div>
                    </div>
                </c:when>    
                
                <c:when test="${facility == '예약' }">    
                        <div class="facility">
                            <div class="icon_area">
                                <img class="facility_icon" src="${contextPath}/resources/image/icon/appointment.png">
                            </div>
                            <div class="text_area"> 
                                예약
                            </div>
                        </div>
                    
                    </c:when>   
                
                <c:when test="${facility == '24시간' }">      
                    <div class="facility">
                        <div class="icon_area">
                            <img class="facility_icon" src="${contextPath}/resources/image/icon/24hour.png">
                        </div>
                        <div class="text_area"> 
                            24시
                        </div>
                    </div>
                </c:when>    
            </c:choose>
        </c:forEach> 
    </div> 
    
    <hr style="margin-bottom: 15px;">

    <div class="row-item" >
            <span class="highlighter">병원 정보</span>
        <div style="font-size:15px;">
                ${hospital.hospInfo }
        </div>
    </div>
    <hr style="margin-bottom: 15px;">


    <div class="row-item">
        <span class="highlighter">상세위치</span>
        <div id="map">
        </div>
    </div>
    


<div class="row-item">
    <c:choose>
        <c:when test="${!empty param.sk && !empty param.sv }">
            <c:url var="goToList" value="../hospital/search">
                <!--../ : 상위 주소로 이동 <상대경로>  -->
                <c:param name="cp">${param.cp }</c:param>
                <c:param name="sk">${param.sk }</c:param>
                <c:param name="sv">${param.sv }</c:param>
            </c:url>
        </c:when>
        <c:otherwise>
            <c:url var="goToList" value="/hospital/list">
                <c:param name="cp">${param.cp}</c:param>
            </c:url>
        </c:otherwise>	
    </c:choose>
    <a href="${goToList }" class="btn_class" id="back">돌아가기</a>
</div>
        <!-- 관리자만 보이는 버튼 -->
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

</div><!-- wrapper -->


<jsp:include page="/WEB-INF/views/common/footer.jsp"/>
```


<br><br>

- 카카오 지도 API

```java
<script type="text/javascript" src="//dapi.kakao.com/v2/maps/sdk.js?appkey=앱키&libraries=services,clusterer,drawing">
</script>



<script>
    var mapContainer = document.getElementById('map'), // 지도를 표시할 div 
    mapOption = {
        center : new kakao.maps.LatLng(33.450701, 126.570667), // 지도의 중심좌표
        level : 3
    // 지도의 확대 레벨
    };

    // 지도를 생성합니다    
    var map = new kakao.maps.Map(mapContainer, mapOption);
    // 주소-좌표 변환 객체를 생성합니다
    var geocoder = new kakao.maps.services.Geocoder();

    // 주소로 좌표를 검색합니다
    geocoder.addressSearch('${hospital.location2 }',function(result, status){
        // 정상적으로 검색이 완료됐으면 
        if (status === kakao.maps.services.Status.OK) {

            var coords = new kakao.maps.LatLng(result[0].y,
                    result[0].x);

            // 결과값으로 받은 위치를 마커로 표시합니다
            var marker = new kakao.maps.Marker({
                map : map,
                position : coords
            });

            // 인포윈도우로 장소에 대한 설명을 표시합니다
            var infowindow = new kakao.maps.InfoWindow(
                    {
                        content : '<div style="font-size: 13px;width:150px;text-align:center;padding:6px 0;">${hospital.hospNm }</div>'
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