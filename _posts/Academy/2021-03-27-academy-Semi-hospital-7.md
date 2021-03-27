---
title: "[동물병원] 목록 조회 화면"
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

- 출력 화면 <br>

![image](https://user-images.githubusercontent.com/73421820/112639770-fbb90a00-8e83-11eb-84af-97af1b319175.png)

<br><br>

- JSP코드

```html
<jsp:include page="/WEB-INF/views/common/otherHeader.jsp"></jsp:include>
<div class="wrapper">
    <!-- 검색창 -->
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

    <!-- 동물 병원 리스트 -->

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


    <!-- 등록하기 버튼  (관리자로 로그인 했을 때만 보인다.-->
    <c:if test="${!empty loginMember && loginMember.memberAdmin == 'A' }">
        <div class="row-item">
            <button type="button" class="btn_class" id="insertHospital"
                onclick="location.href = '${contextPath}/hospital/insertForm'">등록하기</button>
        </div>
    </c:if>


    <%---------------------- Pagination ----------------------%>
    <%-- 페이징 처리 주소를 쉽게 사용할 수 있도록 미리 변수에 저장 --%>
    <c:choose>
        <%-- 검색 내용이 파라미터에 존재할 때 == 검색을 통해 만들어진 페이지인가? --%>
        <c:when test="${!empty param.sk && !empty param.sv }">
            <c:url var="pageUrl" value="/hospital/search" />

            <%-- 쿼리스트링으로 사용할 내용을 변수에 저장함. --%>
            <c:set var="searchStr" value="&sk=${param.sk }&sv=${param.sv }" />
        </c:when>
        <c:otherwise>
            <c:url var="pageUrl" value="/hospital/list" />
        </c:otherwise>
    </c:choose>


    <!-- 화살표에 들어갈 주소를 변수로 생성 -->
    <%-- 검색을 안 했을 때 : /hospital/list?cp=1 
                검색을 했을 때 : /search?cp=1&location=서울&sv=49   --%>
    <c:set var="firstPage" value="${pageUrl}?cp=1${searchStr }" />
    <c:set var="lastPage"
        value="${pageUrl}?cp=${pInfo.maxPage}${searchStr }" />

    <%-- EL을 이용한 숫자 연산의 단점 : 연산이 자료형에 영향을 받지 않는다. --%>
    <%-- <fmt:parseNumber> : 숫자 형태를 지정하여 변수 선언 
        integerOnly="true" : 정수로만 숫자 표현 (소수점 버림)
        --%>


    <fmt:parseNumber var="c1" value="${(pInfo.currentPage - 1)/10}"
        integerOnly="true" />
    <fmt:parseNumber var="prev" value="${c1 * 10}" integerOnly="true" />
    <c:set var="prevPage" value="${pageUrl}?cp=${prev}${searchStr }" />
    <!-- /board/list/do?cp=10  -->

    <fmt:parseNumber var="c2" value="${(pInfo.currentPage + 9)/10}"
        integerOnly="true" />
    <fmt:parseNumber var="next" value="${c2 * 10 + 1}" integerOnly="true" />
    <c:set var="nextPage" value="${pageUrl}?cp=${next}${searchStr }" />





    <!-- 페이징 -->
    <div class="paging">
        <nav aria-label="Page navigation example">
            <ul id="pagingBtn"
                class="pagination pagination-sm justify-content-center">

                <%-- 현재 페이지가 10페이지 초과인 경우 --%>
                <c:if test="${pInfo.currentPage>10}">
                    <!-- 첫 페이지로 이동(<<) -->
                    <li class="page-item"><a class="page-link"
                        href="${firstPage}">&lt;&lt;</a></li>

                    <!-- 이전 페이지로 이동(<)  -->
                    <li class="page-item"><a class="page-link" href="${prevPage}">&lt;</a></li>
                </c:if>

                <!-- 페이지 목록  -->
                <c:forEach var="page" begin="${pInfo.startPage}"
                    end="${pInfo.endPage}">
                    <c:choose>
                        <c:when test="${pInfo.currentPage == page }">
                            <!-- 현재 보고 있는 페이지는 클릭이 안 되게 한다.  -->
                            <li class="page-item"><a class="page-link"
                                style="color: orange;">${page }</a></li>
                        </c:when>

                        <c:otherwise>
                            <li class="page-item"><a class="page-link"
                                href="${pageUrl }?cp=${page}${searchStr}">${page}</a></li>
                        </c:otherwise>
                    </c:choose>
                </c:forEach>

                <%-- 다음 페이지가 마지막 페이지 이하인 경우 --%>
                <c:if test="${next <= pInfo.maxPage }">
                    <!-- 다음 페이지로 이동  -->
                    <li class="page-item"><a class="page-link"
                        href="${nextPage }">&gt;</a></li>
                    <li class="page-item"><a class="page-link"
                        href="${lastPage }">&gt;&gt;</a></li>
                </c:if>
            </ul>
        </nav>
    </div>
</div>

<jsp:include page="/WEB-INF/views/common/footer.jsp"></jsp:include>
```


<br><br>

- CSS

```css
/* ---------------------동물병원 조회 페이지-----------------------------------------------  */
* {
	font-family: 'Noto Sans KR', sans-serif;
}

.wrapper {
	width: 1100px;
	box-sizing: border-box;
	margin: 0 auto;
	/*     border : 1px solid sienna */
}

.hospital_list {
	width: 60%;
	margin: auto;
	/*     border : 1px solid gold */
}

.row-item {
	width: 100%;
	box-sizing: border-box;
}

/* ---------------------------검색창------------------------- */

/* 배경화면  */
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

/* 버튼 */
.btn_class {
	border: 1px solid #8bd2d6;
	background-color: #8bd2d6;
	cursor: pointer;
	outline: none;
}

/* ----------------------지역 선택------------------------------- */
.locationSelect {
	margin: 0 0 30px 85%;
}

.locationNm {
	background-color: fff;
	font-size: 16px;
	width: 75px;
	border: 3px solid #8bd2d6;
	border-radius: 5px;
}

/* --------------------동물 병원 리스트--------------------------- */
.thumbnail {
	/*   background-color : burlywood; */
	width: 100%;
	height: 200px;
	margin-bottom: 15px;
	box-sizing: border-box;
	display: table;
}

.thumbnail_img {
	/*     border: 1px solid red; */
	height: 100%;
	width: 30%;
	display: inline-block;
	box-sizing: border-box;
	display: table-cell;
	table-layout: fixed;
	vertical-align: middle;
}

.hospital_img {
	width: 300px;
	height: 200px;
}

.thumbnail_info {
	/*    border: 1px solid green; */
	height: 100%;
	width: 60%;
	display: inline-block;
	box-sizing: border-box;
	display: table-cell;
	table-layout: fixed;
	vertical-align: middle;
}

.hospital_info {
	font-size: 13px;
	margin: 0;
}

#hospital_name {
	font-size: 25px;
	font-weight: bold;
}

#hospital_name:hover {
	color: #000;
}

.icon {
	width: 10px;
	height: 10px;
	margin-right: 10px;
}

/* ------------------------글쓰기버튼------------------------------- */
#insertHospital {
	width: 100px;
	height: 40px;
	margin: 40px 0 0 90%;
	line-height: 20px;
	border-radius: 5px;
	color: #fff;
	font-size: 17px;
}

.btn_class:hover {
	background-color: #17a2b8;
}

/*----------------------페이징(부트스트랩)------------------------  */
.paging {
	margin-top: 100px;
}

.page-item>a {
	color: black;
	text-decoration: none;
}

.page-item>a:hover {
	color: orange;
}

/* 상세페이지 이동 커서  */
.numberSelect {
	cursor: pointer;
}
```

<br><br>

