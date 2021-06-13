---
title: "[Spring] 화면 처리 - 목록 화면 처리"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 목록 화면 처리

list.jsp 페이지의 일부를 include 하는 방식으로 처리했음에도 많은 HTML의 내용들이 존재하므로 최소한의 태그들만 적용한다.<br>
<br>

list.jsp에는 JSTL의 출력과 포맷을 적용할 수 있는 태그 라이브러리를 추가한다. <br>
<br>

> list.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>


<%@include file="../includes/header.jsp" %>

    <div class="row">
        <div class="col-lg-12">
            <h1 class="page-header">Tables</h1>
        </div>
        <!-- /.col-lg-12 -->
    </div>
    <!-- /.row -->
    
    <div class="row">
        <div class="col-lg-12">
            <div class="panel panel-default">
                <div class="panel-heading">
                    Board List Page
                </div>
                <!-- /.panel-heading -->
                <div class="panel-body">
                    <table width="100%" class="table table-striped table-bordered table-hover" id="dataTables-example">
                        <thead>
                            <tr>
                                <th>#번호</th>
                                <th>제목</th>
                                <th>작성자</th>
                                <th>작성일</th>
                                <th>수정일</th>
                            </tr>
                        </thead>
                    </table>
                </div>
                <!-- /.panel-body -->
            </div>
            <!-- /.panel -->
        </div>
        <!-- /.col-lg-12 -->
    </div>

<%@include file="../includes/footer.jsp" %>
```
<br>

수정된 list.jsp를 저장하고 브라우저를 통해서 원하는 형태로 출력되는지 확인한다.<br>
<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121805168-f0a49780-cc84-11eb-8521-b0f9d748d889.png)
<br><br>

### Model에 담긴 데이터 출력

'board/list'를 샐행했을 때 이미 BoardController는 Model을 이용해서 게리물의 목록을 'list'라는 이름으로 담아서 전달했으므로 list.jsp에서는 이를 출력한다.<br>
출력은 JSTL을 이용해서 처리한다.<br>
<br>

> list.jsp

```java
<table width="100%" class="table table-striped table-bordered table-hover" id="dataTables-example">
    <thead>
        <tr>
            <th>#번호</th>
            <th>제목</th>
            <th>작성자</th>
            <th>작성일</th>
            <th>수정일</th>
        </tr>
    </thead>
    
    <c:forEach items="${list}" var="board">
        <tr>
            <td><c:out value="${board.bno }"/></td>
            <td><c:out value="${board.title }"/></td>
            <td><c:out value="${board.writer }"/></td>
            <td><fmt:formatDate pattern="yyyy-MM-dd" value="${board.regdate }"/></td>
            <td><fmt:formatDate pattern="yyyy-MM-dd" value="${board.updateDate }"/></td>
        </tr>
    </c:forEach>                                		
</table>
```
<br>

브라우저를 통해서 결과를 확인하면 데이터 베이스에 있는 전체 목록이 출력된다.<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121805311-b687c580-cc85-11eb-92ef-f8e6a0e9b831.png)





<br><br>
관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
