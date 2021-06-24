---
title: "[Spring] 페이징 화면 처리"
excerpt: "Spring Framework - JSP에서 페이지 번호 출력"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## JSP에서 페이지 번호 출력

JSP에서 페이지 번호를 출력하는 부분은 JSTL을 이용해서 처리할 수 있다.<br>

부트 스트랩 SB Admin2를 이용해서 list.JSP에 페이지 번호를 출력하는 부분을 추가한다.<br>

> list.jsp

```html
<!-- 페이징  -->
<div class='pull-right'>
    <ul class="pagination">
        <c:if test="${pageMaker.prev }">
            <li class="paginate_button previous"><a href="#">Previous</a></li>
        </c:if>
    
        <c:forEach var="num" begin="${pageMaker.startPage }" end="${pageMaker.endPage }">
            <li class="paginate_button"><a href="#">${num}</a></li>
        </c:forEach>
        
        
        <c:if test="${pageMaker.next }">
            <li class="paginate_button next"><a href="#">NEXT</a></li>
        </c:if>		                            	
    </ul>
</div>
<!-- end Pagination  -->
```
<br>

PageMaker라는 이름으로 전달된 PageDTO를 이용해서 화면에 페이지 번호들을 출력한다.<br>

예를 들어, 현재 total은 123이라는 숫자로 지정되어 있으므로 5페이지를 조회하면 next값은 true가 되어야 한다.<br>
반면에 amount 값이 20인 경우에는 7페이지까지만 출력되어야 한다.<br>


<br><br>


### 페이지 번호 이벤트 처리

화면에서 페이지 번호가 보이기는 하지만 아직 페이지 번호를 클릭했을 때 이벤트 처리가 남아있다.<br>
일반적으로 <a\> 태그의 href 속성을 이용하는 방법을 사용할 수도 있지만, 직접 링크를 처리하는 방식의 경우 검색 조건이 붙고 난 후에 처리가 복잡하게 되므로 JavaScript를 통해서 처리하는 방식을 이용한다.<br>

우선 페이지와 관련된 <a\> 태그의 href 속성값으로 페이지 번호를 가지도록 수정한다.<br>
번호의 출력 부분은 <c:out\>을 이용해서 출력하는 것이 좋지만 예제에서는 가독성의 문제로 일반 EL을 이용한다.<br>
<br>

> list.jsp

```html
<c:if test="${pageMaker.prev }">
    <li class="paginate_button previous">
        <a href="${pageMaker.startPage-1 }">Previous</a>
    </li>
</c:if>

<c:forEach var="num" begin="${pageMaker.startPage }" end="${pageMaker.endPage }">
    <li class="paginate_button ${pageMaker.cri.pageNum == num ? 'active' : '' }">
        <a href="${num }">${num}</a>
    </li>
</c:forEach>


<c:if test="${pageMaker.next }">
    <li class="paginate_button next">
        <a href="${pageMaker.endPage +1 }">NEXT</a>
    </li>
</c:if>		
```
<br>

이제 화면에서는 <a\>태그는 href 속성값으로 단순히 번호만을 가지게 변경된다.<br>
브라우저에서 만들어진 결과를 보면 아래와 같은 형태가 된다.<br>

<br>

> 실행 화면

![image](https://user-images.githubusercontent.com/73421820/123247564-fcfce000-d521-11eb-8eaa-fd8d94ad1676.png)<br>

<br>

이 상태에서 페이지 번호를 클릭하게 되면 해당하는 URL이 존재하지 않기 때문에 문제가 생기게 된다.<br>
<a\>태그가 원래의 동작을 못하도록 JavaScript 처리를 한다.<br>

실제 페이지를 클릭하면 동작을 하는 부분은 별도의 <form\>태그를 이용해서 처리하도록 한다.<br>
(<c:out\>을 사용하는 것이 더 좋은 방법이지만 간단히 사용하기 위해서 EL로 처리한다.)<br>



<br>

> list.jsp

```html
<form id='actionForm' action="/board/list" method='get'>
    <input type="hidden" name='pageNum' value="${pageMaker.cri.pageNum }">
    <input type="hidden" name="amount" value="${pageMaker.cri.amout }">
</form>
```
<br>

기존에 동작하던 JavaScript 부분은 아래와 같이 기존의 코드에 페이지 번호를 클릭하면 처리하는 부분이 추가된다.<br>

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
	
	var actionForm = $("#actionForm");
	
	$(".paginate_button a").on("click", function(e){
		e.preventDefault();
		console.log('click');
		actionForm.find("input[name='pageNum']").val($(this).attr("href"));
	
	});
	
});
</script>
```

<br>

list.jsp에서는 <form\> 태그를 추가해서 URL의 이동을 처리하도록 변경했다.<br>
JavaScript에서는 <a\> 태그를 클릭해도 페이지 이동이 없도록 preventDefault()처리를 하고, <form\> 태그 내 pageNum 값은 href 속성값으로 변경한다.<br>
이 처리를 하고나면 화면에서 페이지 번호를 클릭했을 때 <form\>태그 내의 페이지 번호가 바뀌는 것을 브라우저에서 개발자 도구를 통해 확인할 수 있다.<br>

![image](https://user-images.githubusercontent.com/73421820/123255418-c5defc80-d52a-11eb-8b62-630574e8af1e.png)<br>




<br>

마지막 처리는 actionForm 자체를 submit() 시켜야 한다.<br>

<br>

> list.jsp

```html
$(".paginate_button a").on("click", function(e){
    e.preventDefault();
    console.log('click');
    actionForm.find("input[name='pageNum']").val($(this).attr("href"));
    actionForm.submit();
}); 
```

<br>

브라우저에서 페이지 번호를 클릭하면 화면에서 제대로 이동이 되는지 확인한다.<br>


![image](https://user-images.githubusercontent.com/73421820/123255475-db542680-d52a-11eb-91f7-95cbeb220736.png)




<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
