---
title: "[Spring] 페이징 화면 처리"
excerpt: "Spring Framework - 수정과 삭제 처리"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 수정과 삭제 처리

modify.jsp에서는 <form\> 태그를 이용해서 데이터를 처리한다.<br>

거의 입력과 비슷한 방식으로 구현되는데, 이제 pageNum과 amount라는 값이 존재하므로 <form\>태그 내에서 전송할 수 있게 수정해야 한다.<br>
<br>

> modify.jsp

```html
<form role="form" action="/board/modify" method="post">

    <input type='hidden' name='pageNum' value='<c:out value="${cri.pageNum }"/>'>
    <input type='hidden' name='amount' value='<c:out value="${cri.amount }"/>'>
```
<br>

modify.jsp 역시 Criteria를 Model에서 사용하기 때문에 태그를 만들어서 <form\> 태그 전송에 포함한다.<br>

<br>

<br><br>

### 수정/삭제 처리 후 이동

POST 방식으로 진행하는 수정과 삭제 처리는 BoardController에서 각각의 메서드 형태로 구현되어 있으므로 페이지 관련 파라미터들을 처리하기 위해서는 변경해 줄 필요가 있다.<br>

<br>

> BoardController 

```java
@PostMapping("/modify")
public String modify(BoardVO board, @ModelAttribute("cri") Criteria cri, RedirectAttributes rttr) {
    log.info("modify:" + board);
    
    if(service.modify(board)) {
        rttr.addFlashAttribute("result", "success");
    }
    
    rttr.addAttribute("pageNum", cri.getPageNum());
    rttr.addAttribute("amount", cri.getAmount());
    
    
    return "redirect:/board/list";
}
```
<br>

메서드의 파라미터에는 Criteria가 추가된 형태로 변경되고, RedirectAttributes 역시 URL 뒤에 원래의 페이지로 이동하기 위해서 pageNum과 amount 값을 가지고 이동하게 수정한다.<br>

<br>

삭제 처리 역시 동일하게 Criteria를 받아들이는 방식으로 수정한다.<br>

<br>

> BoardController

```java
@PostMapping("/remove")
public String remove(@RequestParam("bno") Long bno,@ModelAttribute("cri") Criteria cri, RedirectAttributes rttr) {
    
    log.info("remove...." + bno);
    
    if(service.remove(bno)) {
        rttr.addFlashAttribute("result", "success");
    }
    
    rttr.addAttribute("pageNum", cri.getPageNum());
    rttr.addAttribute("amount", cri.getAmount());
    
    return "redirect:/board/list";
}	
```
<br>

위와 같은 방식을 이용하면 수정/삭제 후 기존 사용자가 보던 페이지로 이동하는 것을 볼 수 있다.<br>

수정과 달리 삭제는 처리 후 1페이지로 이동해도 무관하지만, 이왕이면 사용자들에게 자신이 보던 정보를 이어서 볼 수 있게 조치해 주는 방식 역시 어렵지 않다.<br>

<br><br><br>

### 수정/삭제 페이지에서 목록 페이지로 이동

페이지 이동의 마지막은 수정/삭제를 취소하고 다시 목록 페이지로 이동하는 것이다.<br>
목록 페이지는 오직 pageNum과 amount만을 사용하므로 <form\> 태그의 다른 내용들은 삭제하고 필요한 내용만을 다시 추가하는 형태가 편리하다.<br>

<br>

> modify.jsp

```java
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
			
			var pageNumTag = $("input[name='pageNum']").clone();
			var amountTag = $("input[name='amount']").clone();
			
			formObj.empty();
			formObj.append(pageNumTag);
			formObj.append(amountTag);
		}
		formObj.submit();
	});
});
</script>
```

<br>

만일 사용자가 'List' 버튼을 클릭한다면 <form\> 태그에서 필요한 부분만 잠시 복사(clone)해서 보관해 두고, <form\>태그 내의 모든 내용은 지워버린다(empty).<br>
이후에 다시 필요한 태그들만 추가해서 '/board/list'를 호출하는 형태를 이용한다.<br>



<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
