---
title: "[동물병원] 검색창"
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

## [동물병원] 검색

- 출력 화면

![image](https://user-images.githubusercontent.com/73421820/112640597-d082ea80-8e84-11eb-9797-79460e2ec195.png)

<br><br>

- CSS

```css
.bg-image-full {
	background: no-repeat center center scroll;
	-webkit-background-size: cover;
	-moz-background-size: cover;
	background-size: cover;
	-o-background-size: cover;
}

div.bg-image-full {
	height: 400px;
	text-align: center;
	border-radius: 15px;
	margin-bottom: 20px;
}

/* 검색 div */
.search {
	width: 38%;
	height: 50px;
	margin: auto;
	margin-top: 20px;
	border-right-width: 0px;
	position: relative;
	padding-top: 20px;
	box-sizing: border-box;
}

/* 검색 옵션창 */
#searchOption {
	display: inline-block;
	position: absolute;
	left: 2%;
	height: 100%;
	width: 25%;
	font-size: 15px;
	font-weight: 500;
	padding: 1em;
	background-color: rgba(255, 255, 255, 0.6);
	border-radius: 15px 0 0 15px;
	border: none;
}
/* 옵션 창 선택시 테두리 없애기 */
#searchOption:focus {
	outline: none;
}

/* 검색어 입력 창  */
.searchBar {
	display: inline-block;
	position: absolute;
	transform: translate(-31%);
	width: 75%;
	height: 98%;
	border: none;
	outline: none;
	font-size: 16px;
	background-color: rgba(255, 255, 255, 0.6);
	border-radius: 0 15px 15px 0;
	padding-left: 10px;
}

/* 검색 버튼 */
#searchBtn {
	height: 100%;
	position: absolute;
	left: 90%;
	width: 60px;
	height: 100%;
	background-color: rgba(255, 255, 255, 0.1);
	cursor: pointer;
	border: none;
	outline: none;
}

/* 돋보기 아이콘  */
#searchIcon {
	position: absolute;
	left: 20px;
	top: 12px;
	width: 25px;
	height: 25px;
	border: none;
}
```

- JSP 코드

```html
<div class="row-item" style="margin-bottom: 90px;">
    <form action="${contextPath }/hospital/search" method="GET"
        id="searchForm">
        <div class="bg-image-full"
            style="background-image: url('https://cdn.pixabay.com/photo/2016/01/19/17/41/friends-1149841_960_720.jpg');">

            <div class="search">
                <select name="sk" id="searchOption">
                    <option value="hospitalName">병원명</option>
                    <option value="location">주소</option>
                </select> <input type="text" name="sv" class="searchBar"
                    placeholder="검색어를 입력해 주세요." autocomplete="off" maxlength='15'>
                <button class="searchBar btn_class" id="searchBtn">
                    <img src="${contextPath}/resources/image/icon/searchIcon.png"
                        id="searchIcon" style="display: inline-block; margin: 0 auto;">
                </button>
            </div>

        </div>
    </form>
</div>
```
<br><br>

- 검색어 유지 script

```java
<script>
    // 검색 내용이 있을 경우 검색창에 해당 내용을 작성해두는 기능
    (function(){
        var searchKey = "${param.sk}";
        
        var searchValue ="${param.sv}";
        
        // select의 option을 반복 접근
        $("select[name=sk]>option").each(function(index,item){
            // index : 현재 접근중인 요소의 인덱스
            // item : 현재 접근중인 요소
            
            if($(item).val() == searchKey){
                $(item).prop("selected",true);
            }
        });
        
        // 검색어 입력창에 searchValue 값 출력
        $("input[name=sv]").val(searchValue);
    })();
</script>
```

<br><br>

> - Form태그로 묶어 버튼 클릭시 제출되게 함.
 - 병원명 / 주소를 선택하는 option 태그의 name을 sk로 지정.
 - 검색어를 입력하는 input 창의 name 을 sv로 지정.
 - 버튼 클릭시 action 주소로 제출 됨. (${contextPath }/hospital/search)
 - 검색된 옵션, 내용, 페이지를 파라미터로 받아 비즈니스 로직 수행.
 - 검색된 결과에 맞게 sql문을 조합 후, 일치하는 게시글의 수를 조회, 페이징 처리.
 - 결과에 맞는 숙소 목록과, 썸네일을 조회 후 화면에 뿌려준다.

<br><br>



- /hospital/search 요청을 전달받을 Controller

```java
package com.kh.semi.hospital.controller;

import java.io.IOException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.kh.semi.hospital.model.service.HSearchService;
import com.kh.semi.hospital.model.vo.Attachment;
import com.kh.semi.hospital.model.vo.Hospital;
import com.kh.semi.hospital.model.vo.PageInfo;


@WebServlet("/hospital/search")
public class HSearchController extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		
		String searchKey = request.getParameter("sk");
		String searchValue = request.getParameter("sv");
		String cp = request.getParameter("cp");
		
		
		try {
			HSearchService service = new HSearchService();
			
			Map<String,Object> map  = new HashMap<String,Object>();
			map.put("searchKey", searchKey);
			map.put("searchValue", searchValue);
			map.put("currentPage", cp);
			
			// 페이징 처리를 위한 데이터를 계산하고 저장하는객체 PageInfo 얻어오기
			PageInfo pInfo = service.getPageInfo(map);
			
			// 검색된 동물병원 목록 조회
			List<Hospital> hList = service.searchHospitalList(map,pInfo);
			
			
			// 동물병원 목록 조회 성공 시 썸네일 목록 조회 수행
			if(hList!=null) {
				List<Attachment> fList = service.searchThumbnailList(map,pInfo);
				
				if(!fList.isEmpty()) {
					request.setAttribute("fList", fList);
				}
			}
			
			// 조회된 내용과 PageInfo 객체를 request객체에 담아서 요청 위임
			String path ="/WEB-INF/views/hospital/hospitalList.jsp";
			
			request.setAttribute("hList", hList);
			request.setAttribute("pInfo", pInfo);
			
			RequestDispatcher view = request.getRequestDispatcher(path);
			view.forward(request, response);
			
		}catch(Exception e) {
			e.printStackTrace();
			String path = "/WEB-INF/views/common/errorPage.jsp";
			request.setAttribute("errorMsg", "검색 과정에서 오류 발생");
			RequestDispatcher view = request.getRequestDispatcher(path);
			view.forward(request, response);
		}
	}
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}

```

<br><br>


- 페이징 처리 정보 Service

```java
public PageInfo getPageInfo(Map<String, Object> map) throws Exception {
    Connection conn = getConnection();

    // 얻어온 파라미터 cp가 null이면 1, 아니면 int형으로 파싱
    map.put("currentPage",
            (map.get("currentPage") == null)? 1 : Integer.parseInt((String) map.get("currentPage")));


    // 검색 조건에 따른 SQL 조건문을 조합하는 메소드 
    String condition = createCondition(map);

    // SQL 조건문을 map에 추가
    map.put("condition", condition);

    // DB에서 조건을 만족하는 게시글의 수를 조회하기
    int listCount = dao.getListCount(conn, condition);

    close(conn);

    return new PageInfo((int) map.get("currentPage"), listCount);
}
```
<br><br>

- 조건에 만족하는 게시글 수 조회 DAO

```java
public int getListCount(Connection conn, String condition) throws Exception {
    int listCount =0;
    
    String query = "SELECT COUNT(*) FROM V_HOSP WHERE HOSP_DEL_FL = 'N' AND " + condition;
    
    try {
        stmt = conn.createStatement();
        rset = stmt.executeQuery(query);
        
        if(rset.next()) {
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


- 검색 조건에 따라 SQL 조합하는 Service

```java
private String createCondition(Map<String, Object> map) {
    String condition = null;

    String searchKey = (String) map.get("searchKey");
    String searchValue = (String) map.get("searchValue");
    

    // 검색 조건에 따라 SQL 조합
    switch (searchKey) {
    case "hospitalName": condition = " HOSP_NM LIKE '%' || '" + searchValue + "' || '%' "; break; 
            
    case "location":  condition = " (LOCATION1 LIKE '%' || '" + searchValue + "' || '%' "
                                    + "OR LOCATION2 LIKE '%' || '" + searchValue + "' || '%') "; break;
    }
    
    
    return condition;
}
```


<br><br>



- 검색 조건에 따라 조건에 만족하는 게시글 수를 조회하는 DAO

```java
public int getListCount(Connection conn, String condition) throws Exception {
    int listCount =0;
    
    String query = "SELECT COUNT(*) FROM V_HOSP WHERE HOSP_DEL_FL = 'N' AND " + condition;
    
    try {
        stmt = conn.createStatement();
        rset = stmt.executeQuery(query);
        
        if(rset.next()) {
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

- 검색된 동물병원 목록 조회 Service

```java
public List<Hospital> searchHospitalList(Map<String, Object> map, PageInfo pInfo) throws Exception {
    Connection conn = getConnection();
    
    String condition = createCondition(map);
    
    List<Hospital> hList = dao.searchHospitalList(conn, condition, pInfo);
    
    close(conn);
    
    return hList;
}
```
<br><br>

-  검색된 동물병원 목록 조회 DAO

```java
public List<Hospital> searchHospitalList(Connection conn, String condition, PageInfo pInfo) throws Exception {
    
    List<Hospital> hList=null;
    String query = "SELECT HOSP_NO, HOSP_NM, LOCATION2, PHONE, OPENING_TIME, CLOSING_TIME FROM" + 
            "    (SELECT ROWNUM RNUM , H.*" + 
            "    FROM" + 
            "        (SELECT * FROM HOSPITAL " + 
            "        WHERE " + condition +
            "        AND HOSP_DEL_FL = 'N' ORDER BY HOSP_NO DESC) H )" + 
            "WHERE RNUM BETWEEN ? AND ?";
    
    
    try {
        
        // SQL 구문 조건절에 대입할 변수 생성 (매 페이지 시작번호 1,7,13... , 매 페이지 끝 번호 6,12,19... )
        int startRow = (pInfo.getCurrentPage()-1) * pInfo.getLimit()+1;
        int endRow = startRow + pInfo.getLimit()-1;
        
        
        pstmt = conn.prepareStatement(query);
        pstmt.setInt(1, startRow);
        pstmt.setInt(2, endRow);
        
        rset = pstmt.executeQuery();
        
        hList = new ArrayList<Hospital>();
        
        while(rset.next()) {
            Hospital hospital = new Hospital(rset.getInt("HOSP_NO"), 
                    rset.getString("HOSP_NM"), rset.getString("LOCATION2"), 
                    rset.getString("PHONE"), rset.getString("OPENING_TIME"), 
                            rset.getString("CLOSING_TIME"));
            
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

- 동물병원 목록 조회 성공 시 썸네일 목록 조회 Service

```java
public List<Attachment> searchThumbnailList(Map<String, Object> map, PageInfo pInfo) throws Exception {
    Connection conn = getConnection();
    
    String condition = createCondition(map);
    
    List<Attachment> fList = dao.searchThumbnailList(conn,pInfo, condition);
    
    close(conn);
    return fList;
}
```

<br><br>

- 동물병원 목록 조회 성공 시 썸네일 목록 조회 DAO

```java
public List<Attachment> searchThumbnailList(Connection conn, PageInfo pInfo, String condition) throws Exception {
    List<Attachment> fList = null;
    
    String query = "SELECT IMG_NAME, HOSP_NO FROM HOSPITAL_IMG " + 
            "WHERE HOSP_NO IN (" + 
            "    SELECT HOSP_NO FROM " + 
            "    (SELECT ROWNUM RNUM, H.* FROM " + 
            "            (SELECT HOSP_NO  FROM HOSPITAL " + 
            "            WHERE HOSP_DEL_FL='N' " + 
            "            AND " + condition + 
            "            ORDER BY HOSP_NO DESC ) H) " + 
            "    WHERE RNUM BETWEEN ? AND ?" + 
            ") " + 
            "AND IMG_LEVEL = 0";
    try {
        // 위치 홀더에 들어갈 시작 행, 끝 행번호 계산
        int startRow = (pInfo.getCurrentPage() -1) * pInfo.getLimit() + 1;
        int endRow = startRow + pInfo.getLimit()-1;
        
        pstmt = conn.prepareStatement(query);
        pstmt.setInt(1, startRow);
        pstmt.setInt(2, endRow);
        
        rset = pstmt.executeQuery();
        
        fList = new ArrayList<Attachment>();
        while(rset.next()){
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