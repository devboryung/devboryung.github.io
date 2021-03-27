---
title: "[동물병원] 목록 조회"
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


## [동물병원] 목록 조회

- 출력 화면

![image](https://user-images.githubusercontent.com/73421820/112645785-2f972e00-8e8a-11eb-8919-660e170158ca.png)


<br><br>

> 헤더에서 동물병원을 클릭하면 요청되는 주소 

```html
<li><a href="${contextPath}/hospital/list" class="nav-items" id="nav-animalHospital">동물병원</a></li>
```

<br><br>

- 전달받을 Controller<br>
 hospital으로 시작되는 모든 주소를 받은 후 contextPath+/hospital 을 잘라 command 에 저장해  일치하는 주소와 매핑시킨다.

 ```java
 package com.kh.semi.hospital.controller;

import java.io.IOException;
import java.util.ArrayList;
import java.util.Enumeration;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.kh.semi.common.MyFileRenamePolicy;
import com.kh.semi.hospital.model.service.HospitalService;
import com.kh.semi.hospital.model.vo.Attachment;
import com.kh.semi.hospital.model.vo.Hospital;
import com.kh.semi.hospital.model.vo.PageInfo;
import com.kh.semi.member.model.vo.Member;
import com.oreilly.servlet.MultipartRequest;

@WebServlet("/hospital/*")
public class HospitalController extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		String uri = request.getRequestURI(); //전체 요청 주소
		
		String contextPath = request.getContextPath();
		
		String command = uri.substring((contextPath+"/hospital").length());
		
		// 컨트롤러 내에서 공용으로 사용할 변수 미리 선언
		
		String path = null; // forward 또는 redirect 경로를 저장할 변수
		RequestDispatcher view = null; // 요청 위임 객체
		
		// sweet alert 메세지 전달하는 용도
		String swalIcon = null;
		String swalTitle = null;
		String swalText = null;
		String errorMsg = null;
		
		try {
			HospitalService service = new HospitalService();
			
			// 현재 페이지를 얻어와 커리스트링으로 사용할 것.
			// 쿼리스트링은 서버측에서 파라미터로 인식된다.
			String cp = request.getParameter("cp");
			// CurrentPage 사용 이유
			// 1) 페이징 처리 시 계산에 필요한 값이기 때문
			// 2) 효율적인 UI/UX를 제공하기 위해서 사용한다(상세조회/북마크...)
		    // 처음엔 얻어 올 값이 없어서 null 이 된다.
			
```


<br><br>


- 동물병원 목록 조회 Controller

```java
if(command.equals("/list")) {
    
    errorMsg = "동물병원 목록 조회 중 오류 발생";
                    
    
    // 1) 페이징 처리를 위한 값 계산 Service호출
    PageInfo pInfo = service.getPageInfo(cp);
    
    
    // 2) 동물병원 목록 조회 비즈니스 로직 수행
    List<Hospital> hList = service.selectHospitalList(pInfo);
    // pInfo를 가져가는 이유 = 
    // pInfo에 담겨져있는 currentPage와 limit를 이용해 현재 페이지에 맞는 게시글 목록을 조회하기 위해
    
    // 썸네일 추가
    if(hList!=null) {
        // 조회된 동물병원이 있다면 썸네일이 있는지 확인해보고 있으면 출력한다.
        List<Attachment> fList = service.selectThumbnailList(pInfo);
        
        if(!fList.isEmpty()) {
            request.setAttribute("fList", fList);
        }
    }
    
    path = "/WEB-INF/views/hospital/hospitalList.jsp";
    
    request.setAttribute("hList", hList);
    request.setAttribute("pInfo", pInfo);
    
    view = request.getRequestDispatcher(path);
    view.forward(request, response);
}
```

<br><br>


- 페이징 처리를 위한 값 계산 Service

```java
public PageInfo getPageInfo(String cp) throws Exception {
    Connection conn = getConnection();
    
    // cp가 null일 경우 1, 아니면 cp를 얻어옴.
    int currentPage = cp == null ? 1 : Integer.parseInt(cp);
    
    // System.out.println(currentPage); 동물병원 화면 들어가자마자 1 출력 됨
    
    // DB에서 전체 게시글 수를 조회하여 반환받기
    int listCount = dao.getListCount(conn);
    
    close(conn);
    
    return new PageInfo(currentPage, listCount);
}
```

<br><br>

- 전체 게시글 수 조회 DAO

```java
public int getListCount(Connection conn) throws Exception {
    int listCount =0;
    String query = prop.getProperty("getListCount");
    
    try {
        stmt = conn.createStatement();
        rset = stmt.executeQuery(query);
        
        while(rset.next()){
            listCount = rset.getInt(1);
        }
    }finally {
        close(rset);
        close(stmt);
    }
    
    return listCount;
}
```

<br><br>

- 전체 게시글 수 조회 sql문

```sql
<entry key="getListCount">
SELECT COUNT(*) FROM HOSPITAL
WHERE HOSP_DEL_FL = 'N'
</entry>
```
<br><br>

- 동물병원 목록 조회 Service

```java
public List<Hospital> selectHospitalList(PageInfo pInfo) throws Exception {
    Connection conn = getConnection();
    
    List<Hospital> hList = dao.selectHospitalList(conn, pInfo);
    
    close(conn);
    return hList;
}
```

<br><br>

- 동물병원 목록 조회 DAO

```java
public List<Hospital> selectHospitalList(Connection conn, PageInfo pInfo) throws Exception {
    List<Hospital> hList = null;
    String query = prop.getProperty("selectHospitalList");
    
    try {
        // SQL 구문 조건절에 대입할 변수 생성
        int startRow = (pInfo.getCurrentPage()-1) * pInfo.getLimit()+1;
        int endRow = startRow + pInfo.getLimit()-1;
        // 7개의 글 중에서 1페이지에 해당하는 글을 가져옴 : 7~2번째의 글만 가져오게 됨.
        
        pstmt = conn.prepareStatement(query);
        pstmt.setInt(1, startRow);
        pstmt.setInt(2,  endRow);
        
        rset = pstmt.executeQuery();
        
        hList = new ArrayList<Hospital>();
        
        while(rset.next()) {
            Hospital hospital = new Hospital(rset.getInt("HOSP_NO"), rset.getString("HOSP_NM"), 
                    rset.getString("LOCATION2"), rset.getString("PHONE"), 
                    rset.getString("OPENING_TIME"), rset.getString("CLOSING_TIME"));
                    
            hList.add(hospital);		
        }
        
    }finally {
        close(rset);
        close(pstmt);
        
    }
    return hList;
}
```
<br><br>

- 동물병원 목록 조회 sql문

```sql
<entry key="selectHospitalList">
SELECT * FROM 
	(SELECT ROWNUM RNUM, H.*
	 FROM (SELECT * FROM HOSPITAL WHERE HOSP_DEL_FL='N' ORDER BY HOSP_NO DESC)H)
WHERE  RNUM BETWEEN ? AND ?
</entry>
```

<br><br>

- 썸네일 목록 조회 Service

```java
public List<Attachment> selectThumbnailList(PageInfo pInfo) throws Exception {
    Connection conn = getConnection();
    
    List<Attachment> fList = dao.selectThumbnailList(conn,pInfo);
    
    close(conn);
    
    return fList;
}
```

<br><br>

- 썸네일 목록 조회 DAO

```java
public List<Attachment> selectThumbnailList(Connection conn, PageInfo pInfo) throws Exception {
    List<Attachment> fList = null;
    
    String query = prop.getProperty("selectThumbnailList");
    
    try {
        // 위치 홀더에 들어갈 시작 행, 끝 행번호 계산
        int startRow = (pInfo.getCurrentPage() -1) * pInfo.getLimit() +1;
        int endRow = startRow + pInfo.getLimit()-1;
        
        pstmt = conn.prepareStatement(query);
        pstmt.setInt(1, startRow);
        pstmt.setInt(2, endRow);
        
        rset = pstmt.executeQuery();
        
        fList = new ArrayList<Attachment>();
        
        while(rset.next()) {
            Attachment at = new Attachment();
            at.setFileName(rset.getString("IMG_NAME"));
            at.setHospNo(rset.getInt("HOSP_NO"));

            
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


- 썸네일 목록 조회 sql문

```sql
<!-- 썸네일 목록 조회  -->
<entry key="selectThumbnailList">
SELECT * FROM HOSPITAL_IMG
WHERE HOSP_NO
IN (SELECT HOSP_NO FROM  
	(SELECT ROWNUM RNUM, H.* FROM 
		(SELECT HOSP_NO FROM V_HOSP 
		WHERE HOSP_DEL_FL='N'
		 ORDER BY HOSP_NO DESC)H)
WHERE RNUM BETWEEN ? AND ? )
AND IMG_LEVEL =0
</entry>
```
<br><br>


> 전달받은 데이터를 화면에 뿌려준다.

```html
<c:choose>
    <c:when test="${empty hList }">
        <!-- hList가 비어있을 때 : 게시글 목록 조회에서 조회되지 않았을 때  -->
        <div class="row-item">
            <div style="text-align: center; font-size: 18px;">등록된 병원이
                없습니다.</div>
        </div>
    </c:when>

    <c:otherwise>
        <!-- 조회된 게시글 목록이 있을 때  -->
        <c:forEach var="hospital" items="${hList}">
            <!-- hList에서 하나씩 꺼내와 hospital에 담는다.  -->
            <div class="row-item " style="margin-bottom: 40px;">

                <div class="thumbnail">
                    <div class="thumbnail_img">

                        <!------------------------ 썸네일 출력  --------------------->
                        <c:set var="flag" value="true" />
                        <c:forEach var="thumbnail" items="${fList }">
                            <c:if test="${hospital.hospNo == thumbnail.hospNo }">
                                <%-- 현재 출력하려는 게시글 번호와 썸네일 목록 중 부모게시글번호가 일치하는 썸네일 정보가 있다면  --%>
                                <img class="hospital_img"
                                    src="${contextPath }/resources/image/uploadHospitalImages/${thumbnail.fileName}">
                                <c:set var="flag" value="false" />
                            </c:if>
                        </c:forEach>

                        <c:if test="${flag == 'true'}">
                            <img class="hospital_img"
                                src="${contextPath }/resources/image/icon/nonImage.png">
                        </c:if>

                        <!-------------------------------------------------------->

                    </div>
                    <div class="thumbnail_info numberSelect" style="cursor: pointer;">
                        <div class="hospital_info">
                            <span id="hospital_name"> ${hospital.hospNm }</span>
                        </div>
                        <div class="hospital_info">
                            <img class="icon"
                                src="${contextPath}/resources/image/icon/site.png">주소 :
                            ${hospital.location2 }
                        </div>
                        <div class="hospital_info">
                            <img class="icon"
                                src="${contextPath}/resources/image/icon/phone.png">연락처
                            : ${hospital.phone }
                        </div>
                        <div class="hospital_info">
                            <img class="icon"
                                src="${contextPath}/resources/image/icon/clock.png">영업시간
                            : ${hospital.openingTime } ~ ${hospital.closingTime }
                        </div>
                    </div>
                    <span style="visibility: hidden">${hospital.hospNo }</span>
                </div>
            </div>
        </c:forEach>
    </c:otherwise>
</c:choose>
<!-- 한 페이지 6개씩 보이기 -->
```
<br><br>