---
title: "[Spring] 화면 처리 - 조회 페이지와 이동"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 조회 페이지와 이동

게시물의 등록과 리스트 처리가 끝났다면 가장 중요한 틀은 완성되었다고 볼 수 있다.<br>

다음으로는 목록 페이지에서 링크를 통해서 GET 방식으로 특정한 번호의 게시물을 조회할 수 있는 기능을 작성한다.<br>


<br><br>

### 조회 페이지 작성

조회 페이지는 입력 페이지와 거의 유사하지만 게시물 번호(bno)가 출력된다는 점과 모든 데이터가 읽기 전용으로 처리된다는 점이 가장 큰 차이이다.<br>

게시물의 조회는 BoardController에서 get() 메서드로 구성되어 있다.<br>

> BoardController 클래스

```java
@GetMapping("/get")
public void get(@RequestParam("bno") Long bno, Model model) {
    
    log.info("/get");
    model.addAttribute("board",service.get(bno));
}
```
<br>

그 후 register.jsp를 복사해서 get.jsp 파일을 생성한다.<br>
get.jsp는 게시물 번호를 보여줄 수 있는 필드를 추가하고, 모든 데이터는 readonly를 지정해서 작성한다.<br>
register.jsp에 있던 <form\> 태그는 조회 페이지에서는 그다지 필요하지 않으므로 제거하는 대신 마지막에는 수정/삭제 페이지로 이동하거나 원래의 목록 페이지로 이동할 수 있는 버튼을 추가한다.<br>

<br>


> get.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>    
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>


<%@include file="../includes/header.jsp" %>    


<div class="row">
	<div class="col-lg-12">
		<h1 class="page-header">Board Read</h1>
	</div>
</div>


<div class="row">
	<div class="col-lg-12">
		<div class="panel panel-default">
			<div class="panel-heading">Board Read Page</div>
			<div class="panel-body">
			
				<div class="form-group">
					<label>Bno</label> 
					<input class="form-control" name="bno" 
					value='<c:out value="${board.bno }"/>' readonly="readonly">
				</div>
				
				<div class="form-group">
					<label>Title</label>
					<input class="form-control" name="title" 
					value='<c:out value="${board.title }"/>' readonly="readonly">
				</div>
				
				<div class="form-group">
					<label>Text area</label> 
					<textarea class="form-control" rows="3" name='content' readonly="readonly">
					<c:out value="${board.content }"/></textarea>
				</div>
				
				<div class="form-group">
					<label>Writer</label> <input class="form-control" name='writer'
					readonly="readonly"><c:out value="${board.writer }"/>
				</div>
				
				<button data-oper='modify' class="btn btn-default">Modify</button>
				<button data-oper='list' class="btn btn-info">List</button>
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

브라우저에서는 '/board/get?bno=1'과 같이 게시물의 번호를 반드시 파라미터로 전달해야 한다.<br>
파라미터로 전달하는 bno 값이 존재한다면 아래와 같은 페이지를 보게 된다.<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121812908-c6fb6880-cca4-11eb-9ec1-bae99d0a022b.png)
<br>


화면 하단의 버튼은 '/board/list'와 '/board/modify?bno=xx'와 같이 이동하는 링크를 추가한다<br>

> get.jsp

```java
<button data-oper='modify' class="btn btn-default"
onclick="location.href='/board/modify?bno=<c:out value="${board.bno }"/>'">Modify</button>
<button data-oper='list' class="btn btn-info"
onclick="location.href='/board/list'">List</button>
```
<br>

<br><br>


### 목록 페이지와 뒤로 가기 문제

목록 페이지에서 각 게시물 제목에 <a\> 태그를 적용해서 조회 페이지로 이동하게 처리한다.<br>
최근에 웹페이지들은 사용자들의 트래픽을 고려해 목록 페이지에서 새창을 띄워서 조회 페이지로 이동하는 방식을 선호하지만 전통적인 방식에서는 현재창 내에서 이동하는 방식을 사용한다.<br>

조금 관심을 가지고 웹페이지들을 이용하다 보면 의외로 이러한 처리가 제대로 되지 않는 경우를 많이 보게 된다.<br>

- 목록에서 조회 페이지로 이동

> list.jsp

```java
<tr>
    <td><c:out value="${board.bno }"/></td>
    <td><a href='/board/get?bno<c:out value="${board.bno }"/>'>
    <c:out value="${board.title }"/></a></td>
    <td><c:out value="${board.writer }"/></td>
    <td><fmt:formatDate pattern="yyyy-MM-dd" value="${board.regdate }"/></td>
    <td><fmt:formatDate pattern="yyyy-MM-dd" value="${board.updateDate }"/></td>
</tr>
```
<br>

브라우저를 통해서 화면을 확인해 보면 각 게시물의 제목에 링크가 걸리는 것을 확인할 수 있고, 제목을 클릭하면 정상적으로 조회 페이지로 이동하는 것을 볼 수 있다.<br>
조회 페이지로의 이동은 JavaScript를 이용해서 처리할 수도 있고, 위와 같이 직접 <a\> 태그를 이용해서도 처리가 가능하다.<br>
만일 조회 페이지를 이동하는 방식이 아니라 '새창'을 통해서 보고 싶다면, <a\>태그의 속성으로 target='_blank'를 지정하면 된다.<br>
\<a>태그와 <form\> 태그에는 target 속성을 지정할 수 있는데 '_blank'는 새로운 창에서 처리된다.<br>

<br>

- 뒤로 가기의 문제

동일한 페이지 내에서 목록 페이지와 조회 페이지의 이동은 정상적으로 처리된 것 같아 보이지만 한 가지 문제가 남아있다.<br>
'등록 -> 목록 -> 조회' 까지는 순조롭지만 브라우저의 '뒤로 가기'를 선택하는 순간 다시 게시물의 등록 결과를 확인하는 방식으로 동작한다는 것이다.<br>

이런 문제가 생기는 원인은 브러우저에서 '뒤로 가기'나 '앞으로 가기'를 하면 서버를 다시 호출하는 게 아니라 과거에 자신이 가진 모든 데이터를 활용하기 때문이다.<br>
브라우저에서 조회 페이지와 목록 페이지를 여러 번 앞으로 혹은 뒤로 이동해도 서버에서는 처음에 호출을 제외하고 별다른 변화가 없는 것을 확인할 수 있다.<br>

이 문제를 해결하려면 window의 history 객체를 이용해서 현재 페이지는 모달창읠 띄울 필요가 없다고 표시를 해 두는 방식을 이용해야 한다.<br>
window의 history 객체는 스택 구조로 동작하기 때문에 아래 그림으로 원리를 살펴보겠다.<br>

![image](https://user-images.githubusercontent.com/73421820/121813774-8c93ca80-cca8-11eb-967a-8c1495754d04.png)
<br>

`그림 1`은 사용자가 브라우저를 열고 '/board/list'를 최초로 호출한 것이다.<br>
history에 쌓으면서 현재 페이지는 모달창을 보여줄 필요가 없다는 표시(그림에서 작은 상자)를 둔다.<br>
<br>

`그림 2`는 사용자가 '/board/register'를 호출한 경우다.
스택의 상단데 '/board/register'가 쌓이게 된다.<br>
만일 이 상태에서 '뒤로 가기'를 실행하면 아래쪽의 '/board/list'가 보여지는데 이때 심어둔 표시를 이용해서 모달창을 띄울 필요가 없다는 것을 확인 할 수 있다.<br>
<br>

`그림 3`은 사용자가 등록을 완료하고 '/board/list'가 호출되는 상황이다.<br>
브라우저에서 '앞으로 가기'나 '뒤로 가기'로 이동한 것이 아니므로 스택의 상단에 추가된다.<br>
등록 직후에 '/board/list'로 이동한 경우에는 모달창이 동작한다.<br>
<br>

그림 3에서 모달창을 보여준 후에는 스택의 상단에 모달창이 필요하지 않다는 표시를 해주어야 한다.<br>
이후에 '/board/register'를 호출하면 `그림 4`와 같이 된다.<br>
<br>

window.history 객체를 조작하는 것은 이론으로는 복잡해 보이지만, 사실 코드는 아래와 같이 약간의 변화만 있다.<br>

<br>

> list.jsp

```html
<script type="text/javascript">

$(document).ready(function(){
	var result = '<c:out value="${result}"/>';
	
	checkModal(result);
	
	history.replaceState({}, null, null);
	

	function checkModal(result){
		if(result === '' || history.state ){
			return;
		}
		if(parseInt(result) > 0){
			$(".modal-body").html("게시글 "+ parseInt(result) + " 번이 등록되었습니다.");
		}
		
		$("#myModal").modal("show");
	}
	
	$("#regBtn").on("click",function(){
		self.location = "/board/register";
	});
});
</script>
```
<br>

기존과 달라진 점은 추가된 history.replaceState() 부분과 checkModal()에서 history.state를 체크하는 부분이다.<br>
JavaScript의 처리는 우선 checkModal()을 실행하는데, 만일 등록된 후에 이동한 것이라면 위의 `그림 3`처럼 되기 때문에 모달창이 보이게 된다.<br>

모달창이 보이는 여부와 관계없이 JavaScript의 모든 처리가 끝나게 되면 history에 쌓이는 상태는 모달창을 보여줄 필요가 없는 상태가 된다.<br>
최종적인 결과는 아래와 같이 처리된다.<br>


<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
