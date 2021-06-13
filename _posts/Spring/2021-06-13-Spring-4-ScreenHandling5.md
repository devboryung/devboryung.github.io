---
title: "[Spring] 화면 처리 - 게시물의 수정/삭제 처리"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 게시물의 수정/삭제 처리


게시물의 수정 작업은 일반적으로 `1)`조회 페이지에서 직접 처리하는 방식이나 `2)`별도의 수정/삭제 페이지를 만들어서 해당 페이지에서 수정과 삭제를 처리하는 방식을 많이 사용한다.<br>
최근에는 게시물의 조회 페이지에서 댓글 등에 대한 처리가 많아지면서 수정과 삭제는 별개의 페이지에서 하는 것이 일반적이다.<br>
<br>
조회 페이지에서는 GET방식으로 처리되는 URL을 통해서 수정/삭제 버튼이 존재하는 화면을 볼 수 있게 제작해야 한다.<br>
수정 혹은 삭제 작업은 POST 방식으로 처리되고, 결과는 다시 목록 화면에서 확인할 수 있는 형태로 제작한다.<br>
<br>

<br><br>

### 수정/삭제 페이지로 이동

BoardController에서 수정/삭제가 가능한 화면으로 이동하는 것은 조회 페이지와 같다.<br>
따라서 기존의 get() 메서드를 조금 수정해서 화면을 구성한다.<br>
<br>


> BoardController 클래스

```java
@GetMapping({"/get", "/modify"})
public void get(@RequestParam("bno") Long bno, Model model) {
    
    log.info("/get of modify");
    model.addAttribute("board", service.get(bno));
}	
```
<br>

@GetMapping이나 @PostMapping 등에는 URL을 배열로 처리할 수 있으므로, 하나의 메서드로 여러 URL을 처리할 수 있다.<br>

브라우저에서는 '/board/modify?bno=30'과 같은 방식으로 처리하므로, modify.jsp를 작성한다.<br>

modify.jsp는 get.jsp와 같지만 수정이 가능한 '제목'이나 '내용'등이 readonly 속성이 없도록 작성한다.<br>
POST 방식으로 처리하는 부분을 위해서는 <form\> 태그로 내용들을 감싸게 한다.<br>

<br>

> modify.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>    
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>


<%@include file="../includes/header.jsp" %>    


<div class="row">
	<div class="col-lg-12">
		<h1 class="page-header">Board Modify</h1>
	</div>
</div>


<div class="row">
	<div class="col-lg-12">
		<div class="panel panel-default">
			<div class="panel-heading">Board Modify Page</div>
			<div class="panel-body">
			
			<form role="form" action="/board/modify" method="post">

				<div class="form-group">
					<label>Bno</label> 
					<input class="form-control" name="bno" 
					value='<c:out value="${board.bno }"/>' readonly="readonly">
				</div>
				
				<div class="form-group">
					<label>Title</label>
					<input class="form-control" name="title" 
					value='<c:out value="${board.title }"/>'>
				</div>
				
				<div class="form-group">
					<label>Text area</label> 
					<textarea class="form-control" rows="3" name='content'>
					<c:out value="${board.content }"/></textarea>
				</div>
				
				
				<div class="form-group">
					<label>Writer</label> <input class="form-control" name='writer'
					value="<c:out value="${board.writer }"/>" readonly="readonly">
				</div>
				
				
				
				<div class="form-group">
					<label>RegDate</label> 
					<input class="form-control" name='regDate'
					value='<fmt:formatDate pattern="yyy/MM/dd" value="${board.regdate }"/>' readonly="readonly">
				</div>
				
				
				
				<button type="submit" data-oper='modify' class="btn btn-default">Modify</button>
				<button type="submit" data-oper='remove' class="btn btn-danger">Remove</button>
				<button type="submit" data-oper='list' class="btn btn-info">List</button>
				
			</form>
			
			</div>
			<!-- end panel-body  -->
		</div>
		<!-- end panel-body  -->
	</div>
	<!-- end panel -->
</div>
<!-- /.row  -->    
    
<%@include file="../includes/footer.jsp" %>
```

<br>

<form\> 태그는 action 속성을 '/board/modify'로 지정했지만, 삭제를 하면 '/board/remove'와 같이 action 속성의 내용을 수정해서 사용하게 된다.<br>
게시물의 '제목', '내용'은 수정이 가능한 형태로 사용해서 사용자가 편집할 수 있게 한다.<br>
등록일과 수정일은 나중에 BoardVO로 수집되어야 하므로 날짜 포맷을 'yyyy/MM/dd'의 포맷으로 해야한다.(만일 포맷이 맞지 않으면 파라미터 수집 부분에 문제가 생기므로 주의가 필요하다)<br>
마지막에는 '수정/삭제/목록'등의 버튼을 추가한다.<br>
<br>

브라우저에서는 'http://localhost:8080/board/modify?bno=(존재하는 게시물 번호)'와 같이 게시물 번호를 이용해서 수정 페이지가 정상적으로 출력되는지 확인한다.<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121814801-e6e35a00-ccad-11eb-97a9-4ae0a4537dcf.png)
<br>

JavaScript에서는 위의 버튼에 따라서 다른 동작을 할 수 있도록 수정해야 한다.<br>
<br>

> modify.jsp

```html
<script type="text/javascript">

$(document).ready(function(){

	var formObj = $("form");
	
	$('button').on("click",function(e){
		
		e.preventDefault();
		
		var operation = $(this).data("oper");
		
		console.log(operation);
		
		if(operation === 'remove'){
			// 삭제화면
			formObj.attr("action", "/board/remove");
		}else if(operation === 'list'){
			// move to list
			self.location = "/board/list";
			return;
		}
		formObj.submit();
	});
});
</script>
```
<br>

JavaScript에서는 <button\> 태그의 'data-oper'속성을 이용해서 원하는 기능을 동작하도록 처리한다.<br>
<form\>태그의 모든 버튼은 기본적으로 submit으로 처리하기 때문에 e.preventDefault()로 기본 동작을 막고 마지막에 직접 submit()을 수행한다.<br>


<br><br>


### 게시물 수정/삭제 확인

화면에서 게시물을 수정한 후에 'modify' 버튼을 통해서 BoardController에 수정을 요청한다.<br><br>

> 수정 화면

![image](https://user-images.githubusercontent.com/73421820/121815072-6160a980-ccaf-11eb-9870-0d01fbe24dbc.png)
<br>


<br>

Modify 버튼을 클릭하면 BoardController에서는 주어진 파라미터들을 BoardVO로 처리하게 되고, 다음과 같이 수정된 값이 제대로 수집된 것을 확인할 수 있다.<br>


```java
INFO : org.zerock.controller.BoardController - modify:BoardVO(bno=46, title=(수정테스트)테스트테스트, content=					(수정테스트)테스트테스트테스트테스트, writer=user00, regdate=null, updateDate=null)
```

<br>

게시물이 수정된 후에는 다시 '/board/list' 화면으로 이동하게 된다.<br>
이 경우에 대한 처리는 이미 완료되었으므로 모달창을 통해서 메시지를 확인할 수 있다.<br>

![image](https://user-images.githubusercontent.com/73421820/121815196-20b56000-ccb0-11eb-8416-a84183abdb91.png)
<br>

화면에서 'Remove'버튼을 클릭하게 되면 <form\>태그의 action 값이 '/board/remove'가 되고 데이터들이 전송된다.<br>
물론 BoardController에서는 bno 값 하나만 필요하지만 처리에는 문제가 없다.<br>
삭제 시 BoardController에는 아래와 같은 로그가 기록되게 된다.<br>

```java
INFO : org.zerock.controller.BoardController - remove....1
INFO : org.zerock.controller.BoardController - list
```
<br>

삭제할 때 목록 페이지로의 이동은 위의 그림과 동일하게 이동한다.<br>



<br><br>


### 조회 페이지에서 <form> 처리

게시물의 조회 페이지에서는 수정과 삭제가 필요한 페이지로 링크를 처리해야 한다.<br>
직접 버튼에 링크를 처리하는 방식을 사용하여 처리했지만, 나중에 다양한 상황을 처리하기 위해서 <form\> 태그를 이용해서 수정한다.<br>
<br>


> get.jsp

```java
<button data-oper='modify' class="btn btn-default">Modify</button>
<button data-oper='list' class="btn btn-info">List</button>

<form id="operForm" action="/board/modify" method="get">
    <input type='hidden' id='bno' name='bno' value='<c:out value="${board.bno }"/>'>
</form>
```

<br>

브라우저에서는 <form\> 태그의 내용은 보이지 않고 버튼만 보이게 된다.<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121815339-f44e1380-ccb0-11eb-8488-d4afde043ac4.png)
<br>

사용자가 버튼을 클릭하면 operForm이라는 id를 가진 <form> 태그를 전송해야 하므로 추가적인 JavaScript 처리가 필요하다.<br>
<br>

> get.jsp

```html
<script type="text/javascript">
$(document).ready(function(){
	
	var operForm = $("#operForm");
	
	$("button[data-oper='modify']").on("click",function(e){
		operForm.attr("action","/board/modify").submit();
	});
	
	$("button[data-oper='list']").on("click",function(e){
		operForm.find("#bno").remove();
		operForm.attr("action","/board/list");
		operForm.submit();
	});
});
</script>
```

<br>

사용자가 수정 버튼을 누르는 경우에는 bno값을 같이 전달하고 <form\>태그를 submit 시켜서 처리한다.<br>

만일 사용자가 list로 이동하는 경우에는 아직 아무런 데이터도 필요하지 않으므로 <form\> 태그 내의 bno 태그를 지우고 submit을 통해서 리스트 페이지로 이동한다.<br>

<br><br>

### 수정 페이지에서 링크 처리

수정 페이지에서는 사용자가 다시 목록 페이지로 이동할 수 있도록 하기 위해서 JavaScript의 내용을 조금 수정한다.<br>

<br>

> modify.jsp

```html
<script type="text/javascript">

$(document).ready(function(){

	var formObj = $("form");
	
	$('button').on("click",function(e){
		
		e.preventDefault();
		
		var operation = $(this).data("oper");
		
		console.log(operation);
		
		if(operation === 'remove'){
			// 삭제화면
			formObj.attr("action", "/board/remove");
		}else if(operation === 'list'){
			// move to list
			formObj.attr("action","/board/list").attr("method","get");
			formObj.empty();
		}
		formObj.submit();
	});
});
</script>
```
<br>

수정된 내용은 클릭한 버튼이 List인 경우 action 속성과 method 속성을 변경한다.<br>

'/board/list'로의 이동은 아무런 파라미터가 없기 때문에 <form\> 태그의 모든 내용은 삭제한 상태에서 submit()을 진행한다.<br> 이후에 코드는 실행되지 않도록 return을 통해서 제어한다.<br>
<br>

이 장에서 진행된 모든 내용을 도식화 시키면 아래와 같은 구조가 된다.<br>

![image](https://user-images.githubusercontent.com/73421820/121815847-5871d700-ccb3-11eb-9eea-1553c41e1742.png)


<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
