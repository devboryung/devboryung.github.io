---
title: "[Spring] 화면 처리"
excerpt: "Spring Framework - 등록 입력 페이지와 등록 처리"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 등록 입력 페이지와 등록 처리

게시물의 등록 작업은 POST 방식으로 처리하지만, 화면에서 입력을 받아야 하므로 GET 방식으로 입력 페이지를 볼 수 있도록 BoardController에 메서드를 추가한다.<br>

<br>

> BoardController 클래스

```java
@GetMapping("/register")
public void register() {
    
}
```

<br>

register()는 입력 페이지를 보여주는 역할만을 하기 때문에 별도의 처리가 필요하지 않다.<br>
views 폴더에는 includes를 적용한 입력 페이지를 작성한다.<br>
<br>

> register.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>    
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>


<%@include file="../includes/header.jsp" %>    


<div class="row">
	<div class="col-lg-12">
		<h1 class="page-header">Board Register</h1>
	</div>
</div>


<div class="row">
	<div class="col-lg-12">
		<div class="panel panel-default">
			<div class="panel-heading">Board Register</div>
			<div class="panel-body">
				<form role="form" action="/board/register" method='post'>
				
					<div class="form-group">
						<label>Title</label> <input class="form-control" name="title">
					</div>
					
					<div class="form-group">
						<label>Text</label> <textarea class="form-control" rows="3" name='content'></textarea>
					</div>
					
					<div class="form-group">
						<label>Writer</label> <input class="form-control" name='writer'>
					</div>
					
					<button type="submit" class="btn btn-default">Submit Button</button>
					<button type="reset" class="btn btn-default">Reset Button</button>
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

register.jsp 페이지에서는 \<form\> 태그를 이용해서 필요한 데이터를 전송한다.<br>
\<input\>이나 \<textarea\>태그의 name 속성은 BoardVO 클래스의 변수와 일치시켜 준다.<br>

브라우저를 통해 '/board/register'화면이 제대로 출력되는지 확인한다.<br>
<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121806049-1f247180-cc89-11eb-9ebd-ed39c93c93a7.png)
<br><br>

화면이 정상적으로 보인다면 입력 항목을 넣어서 새로운 게시물이 등록되는지 확인한다.<br>
BoardController의 POST방식으로 동작하는 register()는 redirect 시키는 방식을 이용하므로, 게시물의 등록 후에는 다시 '/board/list'로 이동하게 된다.<br>
<br>

> BoardController 클래스

```java
@PostMapping("/register")
public String register(BoardVO board, RedirectAttributes rttr) {
    log.info("register: " + board);
    
    service.register(board);
    
    rttr.addFlashAttribute("result", board.getBno());
    
    return "redirect:/board/list";
}
```
<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121806354-7d058900-cc8a-11eb-9c85-9d5637e5f9eb.png)

<br>

게시물의 등록은 정상적으로 이루어지지만 한글이 깨지는 문제가 발생한다.<br>


<br><br>

### 한글 문제와 UTF-8 필터 처리

새로운 게시물을 등록했을 때 한글 입력에 문제가 있다면 1) 브라우저에서 한글이 깨져서 전송되는지를 확인하고, 2) 문제가 없다면 스프링 MVC 족에서 한글을 처리하는 필터를 등록해야 된다.<br>

<br>

브라우저에서 전송되는 데이터는 개발자 도구를 이용해서 확인할 수 있다.<br>
개발자 도구에서 'Network'탭을 열어둔 상태에서 데이터를 보내면 해당 내용을 볼 수 있으므로 이때 POST 방식으로 제대로 전송되었는지, 한글이 깨진 상태로 전송된 것인지를 확인할 수 있다.<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121806608-86432580-cc8b-11eb-94ae-e2f339f4a8e7.png)

<br><br>

위의 화면을 보면 브라우저가 한글을 문제없이 보냇음을 알 수 있는데, 문제는 Controller 혹은 데이터베이스 쪽이라는 것을 알 수 있다.<br>

BoardController와 BoardServiceImpl을 개발할 때는 이미 Lombok의 로그를 이용해서 필요한 기능들을 기록해 두었으므로, 이를 확인해야 된다.<br>
<br>

> Test Log 결과

```java
INFO : jdbc.audit - 11. Connection.prepareStatement(insert into tbl_board (bno,title,content,writer)
		values (?, ?, ?, ?)) returned net.sf.log4jdbc.sql.jdbcapi.PreparedStatementSpy@17ca8b92
INFO : jdbc.audit - 11. PreparedStatement.setLong(1, 45) returned 
INFO : jdbc.audit - 11. PreparedStatement.setString(2, "�׽�Ʈ ���� ����") returned 
INFO : jdbc.audit - 11. PreparedStatement.setString(3, "�׽�Ʈ ���� ����") returned 
INFO : jdbc.audit - 11. PreparedStatement.setString(4, "user00") returned 
INFO : jdbc.sqlonly - insert into tbl_board (bno,title,content,writer) values (45, '�׽�Ʈ ���� ����', '�׽�Ʈ ���� ����', 
'user00') 

INFO : jdbc.sqltiming - insert into tbl_board (bno,title,content,writer) values (45, '�׽�Ʈ ���� ����', '�׽�Ʈ ���� ����', 
'user00') 
```
<br>

위의 로그를 살펴보면 BoardController에 전달될 때 이미 한글이 깨진 상태로 처리된 것을 볼 수 있다.<br>
이 문제를 해결하기 위해서 web.xml에 필터를 추가한다.<br>
<br>

> web.xml

```xml
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>

<filter-mapping>
    <filter-name>encoding</filter-name>
    <servlet-name>appServlet</servlet-name>
</filter-mapping>
```

<br>

한글에 대한 처리가 끝난 후 다시 게시물을 작성해 보면 한글에 문제가 없이 입력되는 것을 확인할 수 있다.<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121807782-990c2900-cc90-11eb-8eb6-c7fd1633c60c.png)

![image](https://user-images.githubusercontent.com/73421820/121807788-9dd0dd00-cc90-11eb-9522-a3573b963917.png)




<br><br>

### 재전송(redirect) 처리

등록 과정에서 POST방식으로 데이터가 처리되는 과정을 그림으로 표현하면 다음과 같다.<br>

![image](https://user-images.githubusercontent.com/73421820/121807960-54cd5880-cc91-11eb-9ef1-c3b8d6b1afd3.png)

<br>



BoardController에서 register() 메서드는 'redirect:/board/list'를 전송하는데 브라우저는 이를 통보받은 후 '/board/list'로 이동하게 된다.<br>
만일 위와 같이 재전송을 하지 않는다면 사용자는 브라우저의 '새로고침'을 통해서 동일한 내용을 계속 서버에 등록할 수 있기 때문에(흔히 도배라고 표현하는) 문제가 발생하게 된다.<br>
브라우저에서는 이런 경우 경고창을 보여주기는 하지만 근본적으로 차단하지는 않는다.<br>
따라서 등록, 수정, 삭제 작업은 처리가 완료된 후 다시 동일한 내용을 전송할 수 없도록 아예 URL을 이동하는 방식을 이용한다.<br>
이러한 과정에서 하나 더 신경 써야 하는 것은 브라우저에 등록, 수정, 삭제의 결과를 바로 알 수 있게 피드백을 줘야 한다는 점이다.<br>
경고창이나 \<div\>를 이용하는 모달창을 이용해서 이런 작업을 처리한다.<br>

BoardController에서 redirect 처리를 할 때 RedirectAttributes라는 특별한 타입의 객체를 이용했다.<br>
addFlashAttribute()의 경우 이러한 처리에 적합한데, 그 이유는 일회성으로만 데이터를 전달하기 때문이다.<br>
addFlashAttribute()로 보관된 데이터는 단 한 번만 사용할 수 있게 보관된다.(내부적으로는 HttpSession을 이용해서 처리)<br>
list.jsp 페이지의 아래쪽에 \<script\> 태그를 이용해서 상황에 따른 메시지를 확인할 수 있다.<br>

> list.jsp

```java
<script type="text/javascript">

$(document).ready(function(){
	var result = '<c:out value="${result}"/>';
});

</script>
```

<br>

만약 새로운 게시물이 등록된 직후에 위의 코드는 다음과 같이 처리된다.<br>
(아래 번호는 생성된 게시물의 번호이다.)<br>

```
<script type="text/javascript">

$(document).ready(function(){
	var result = '15';
});

</script>
```
<br>

새로운 게시물의 번호는 addFlashAttribute()로 저장되었기 때문에 한 번도 사용된 적이 없다면 위와 같이 값을 만들어 내지만 사용자가 '/board/list'를 호출하거나, '새로고침'을 통해서 호출하는 경우는 아래와 같이 아무런 내용이 없게 된다.<br>

```
<script type="text/javascript">

$(document).ready(function(){
	var result = '';
});

</script>
```
<br>

addFlashAttribute()를 이용해서 일회성으로만 데이터를 사용할 수 있으므로 이를 이용해서 경고창이나 모달창 등을 보여주는 방식으로 처리할 수 있다.<br>




<br><br>


### 모달(Modal)창 보여주기

최근에는 브라우저에서 경고창(alert)을 띄우는 방식보다 모달창(Modal)을 보여주는 방식을 많이 사용한다.<br>
BootStrap은 모달창을 간단하게 사용할 수 있으므로 목록 화면에서 필요한 메시지를 보여주는 방법을 사용한다.<br>
<br>

모달창은 기본적으로 \<div>를 화면에 특정 위치에 보여주고, 배경이 되는 <div\> 에 배경색을 입혀서 처리한다.<br>
모달창은 활성화된 \<div\>를 선택하지 않고는 다시 원래의 화면을 볼 수 없도록 막기 때문에 메시지를 보여주는데 효과적인 방식이다.<br>
모달창에 대한 코드는 다운로드한 SBAdmin2의 pages 폴더 내 notifications.html 파일을 참고한다.<br>
<br>

모달창을 처리하기 위해서 우선 \<div>를 이용해서 페이지의 코드에 추가한다.<br>
list.jsp 내에 \<table> 태그의 아래쪽에 모달창의 \<div>를 추가한다.<br>
<br>

> list.jsp

```java
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" 
    aria-labelledby="myModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
                <h4 class="modal-title" id="myModalLabel">Modal title</h4>
            </div>
            <div class="modal-body">처리가 완료되었습니다.</div>
            <div class="modal-footer">
                <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                <button type="button" class="btn btn-primary">Save changes</button>
            </div>
        </div>
        <!-- /.modal-content  -->
    </div>	
    <!-- /.modal-dialog  -->
</div> 
<!-- /.modal -->
```
<br>

모달창을 보여주는 작업은 jQuery를 이용해서 처리할 수 있다.<br>
<br>

> list.jsp 내 JQuery

```html
<script type="text/javascript">

$(document).ready(function(){
	var result = '<c:out value="${result}"/>';
	
	checkModal(result);
	
	function checkModal(result){
		if(result === ''){
			return;
		}
		if(parseInt(result)>0){
			$(".modal-body").html("게시글 "+ parseInt(result) + " 번이 등록되었습니다.");
		}
		
		$("#myModal").modal("show");
	}
});
</script>
```
<br>

checkModal() 함수는 파라미터에 따라서 모달창을 보여주거나 내용을 수정한 뒤 보이도록 작성한다.<br>
checkModal()에서는 새로운 게시글이 작성되는 경우 RedirectAttributes로 게시물의 번호가 전송되므로 이를 이용해서 모달창의 내용을 수정한다.<br>
$("#modal").modal('show')를 호출하면 모달창이 보이게 된다.<br>
<br>

이제 '/board/register'를 이용해서 새로운 게시물을 작성하고 나면 자동으로 '/board/list'로 이동하면서 모달창이 보이게 된다.<br>
<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121812131-b85f8200-cca1-11eb-8b99-3cd1307c3c1f.png)

<br><br>

![image](https://user-images.githubusercontent.com/73421820/121812295-45a2d680-cca2-11eb-968a-2540ec290aad.png)

<br>




<br><br>



### 목록에서 버튼으로 이동하기

게시물의 작성과 목록 페이지로 이동이 정상적으로 동작했다면, 마지막으로 목록 상단에 버튼을 추가해서 등록 작업을 시작할 수 있게 처리한다.<br><br>

> list.jsp

```java
<div class="panel-heading">
    Board List Page
    <button id='regBtn' type="button" class="btn btn-xs pull right">Register New Board</button>
</div>
```
<br>

list.jsp 하단의 jQuery를 이용하는 부분에서 해당 버튼을 클릭했을 때의 동작을 정의한다.<br>

```html
<script type="text/javascript">

$(document).ready(function(){
	var result = '<c:out value="${result}"/>';
	
	checkModal(result);
	

	function checkModal(result){
		if(result === ''){
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

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121812505-f7420780-cca2-11eb-85a9-8ceced7e231a.png)

<br>


화면에서 'Register New Board' 버튼을 클릭하면 게시물의 등록 페이지로 이동할 수 있다.<br>


<br><br>


관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
