---
title: "[숙소] 페이징 처리"
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


## [숙소] 페이징 처리

- 출력 화면

![image](https://user-images.githubusercontent.com/73421820/112479582-892f2800-8db8-11eb-93c5-236f12f2ae6e.png)


<br><br>



- JSP

```html
<%---------------------- Pagination ----------------------%>
<%-- 페이징 처리 주소를 쉽게 사용할 수 있도록 미리 변수에 저장 --%>
<c:choose>
    <%-- 검색 내용이 파라미터에 존재할 때 == 검색을 통해 만들어진 페이지인가? --%>
    <c:when test="${!empty param.sk && !empty param.sv }">
        <c:url var="pageUrl" value="/room/search" />

        <%-- 쿼리스트링으로 사용할 내용을 변수에 저장함. --%>
        <c:set var="searchStr" value="&sk=${param.sk }&sv=${param.sv }" />
    </c:when>
    <c:otherwise>
        <c:url var="pageUrl" value="/room/list" />
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
                        <!-- 현재 보고 있는 페이지는 클릭이 안 되게 한다.  
                             현재 보고 있는 페이지는 주황색으로 표시한다.-->
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
```

<br>

> 페이징 관련 Service는 목록 조회/ 검색 시 수행함.

<br><br>