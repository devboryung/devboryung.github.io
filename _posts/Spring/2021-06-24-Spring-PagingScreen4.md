---
title: "[Spring] 페이징 화면 처리"
excerpt: "Spring Framework - 조회 페이지로 이동"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 조회 페이지로 이동

목록 화면에서 페이지 번호를 클릭하면 정상적으로 원하는 페이지로 이동하는 것을 볼 수 있지만, 몇 가지 문제가 있다.<br>

우선 사용자가 3페이지에 있는 게시글을 클릭한 후 다시 목록으로 이동해 보면 무조건 1페이지 목록 페이지로 이동하는 증상이 일어난다.<br>

<br><br>
페이징 처리를 하고 나면 특정 게시물의 조회 페이지로 이동한 후 다시 목록으로 돌아가는데 문제가 생긴다.<br>
조회 페이지에서 'List'를 선택하면 다시 1페이지의 상태로 돌아가는 문제가 발생하는 것을 볼 수 있다.<br>
이를 해결하기 위해서 조회 페이지로 갈 때 현재 목록 페이지의 pageNum과 amount를 같이 전달해야 한다.<br>
이런 경우 페이지 이동에 사용했던 <form\> 태그에 추가로 게시물의 번호를 같이 전송하고, action 값을 조정해서 처리할 수 있다.<br>
<br>

원래 게시물의 제목에는 '/board/get?bno=xxx'로 이동할 수 있는 링크가 직접 처리되어 있었다.<br>

<br>

> list.jsp

```html
<a href='/board/get?bno=<c:out value="${board.bno}"/>'>
<c:out value="${board.title}"/> </a>
```

<br>

페이지 번호는 조회 페이지에 전달되지 않기 때문에 조회 페이지에서 목록 페이지로 이동할 때는 아무런 정보가 없이 다시 '/board/list'를 호출하게 된다.<br>

간단하게는 각 게시물의 링크에 추가로 '&pageNum=xx'와 같이 처리할 수도 있지만 나중에 여러 조건들이 추가되는 상황에서는 복잡한 링크를 생성해야 한다.<br>
<br>

<a\>태그로 복잡한 링크를 생성하는 방식이 나쁘다고는 말할 수 없다.<br>
가장 대표적인 예가 검색엔진이다.<br>
검색엔진에서는 출력된 정보와 링크를 저장해서 사용하기 때문에 <a\>태그 내의 링크가 완전한 URL인 경우가 노출에 유리하다.<br>
만일 웹페이지가 검색엔진에 의해서 노출이 필요한 경우라면 직접 모든 문자열을 구성해 주는 방식이 더 좋다.<br>
<br>

직접 링크로 연결된 경로를 페이지 이동과 마찬가지로 <form\> 태그를 이용해서 처리할 것이므로 <a\> 태그에는 이동하려는 게시물의 번호만을 가지게 수정한다.<br>
(이벤트 처리를 수월하게 하기 위해서 <a\>태그에 class 속성을 하나 부여한다.)<br>

<br>

> list.jsp

```html
<td>
    <a class="move" href='<c:out value="${board.bno}" />'>
    <c:out value="${board.title}" /></a>
</td>
```

<br>

화면에서는 조회 페이지로 가는 링크 대신에 단순히 번호만이 출력된다.<br>
(마우스를 올려보면 아래쪽에서 확인이 가능하다.)<br>

실제 클릭은 JavaScript를 통해서 게시물의 제목을 클릭했을 때 이동하도록 이벤트 처리를 새로 작성한다.<br>

<br>

> list.jsp

```html
$(".move").on("click", function(e){
    
e.preventDefault();
actionForm.append("<input type='hidden' name='bno' value='"+$(this).attr("href")+"'>");
actionForm.attr("action","/board/get");
actionForm.submit();

});
```
<br>

게시물의 제목을 클릭하면 <form\> 태그에 추가로 bno값을 전송하기 위해서 <input\>태그를 만들어서 추가하고, <form\>태그의 action은 'board/get'으로 변경한다.<br>
위의 처리가 정상적으로 되었다면 게시물의 제목을 클릭했을 때 pageNum과 amount 파라미터가 추가로 전달되는 것을 볼 수 있다.<br>

![image](https://user-images.githubusercontent.com/73421820/123269151-ad2a1300-d539-11eb-88f5-83cb194c62de.png)<br>

<br>
<br>
<br>

### 조회 페이지에서 다시 목록 페이지로 이동 - 페이지 번호 유지

조회 페이지에 다시 목록 페이지로 이동하기 위한 파라미터들이 같이 전송되었다면 조회 페이지에서 목록으로 이동하기 위한 이벤트를 처리해야 한다.<br>

BoardController의 get()메서드는 원래는 게시물의 번호만 받도록 처리되어 있었지만, 추가적인 파라미터가 붙으면서 Criteria를 파라미터로 추가해서 받고 전달한다.<br><br>

> BoardController 클래스

```java
@GetMapping({"/get", "/modify"})
public void get(@RequestParam("bno") Long bno, @ModelAttribute("cri") Criteria cri, Model model) {
    
    log.info("/get of modify");
    model.addAttribute("board", service.get(bno));
}	
```
<br>


@ModelAttribute는 자동으로 Model에 데이터를 지정한 이름으로 담아준다.<br>
@ModelAttribute를 사용하지 않아도 Controller에서 화면으로 파라미터가 된 객체는 전달이 되지만, 좀 더 명시적으로 이름을 지정하기 위해서 사용한다.<br>

<br>

기존 get.jsp에서는 버튼을 클릭하면 <form\> 태그를 이용하는 방식이었으므로 필요한 데이터를 추가해서 이동하도록 수정한다.<br>
<br>

> get.jsp

```html
<form id="operForm" action="/board/modify" method="get">
    <input type='hidden' id='bno' name='bno' value='<c:out value="${board.bno }"/>'>
    <input type='hidden' name='pageNum' value='<c:out value="${cri.pageNum }"/>'>
    <input type='hidden' name='amount' value='<c:out value="${cri.amount }"/>'>
</form>
```

<br>

get.jsp는 operForm이라는 id를 가진 <form\>태그를 이미 이용했기 때문에 cri라는 이름으로 전달된 Criteria 객체를 이용해서 pageNum과 amount값을 태그로 구성하고, 버튼을 클릭했을 때 정상적으로 목록 페이지로 이동하게 처리한다.<br>

<br><br><br>


### 조회 페이지에서 수정/삭제 페이지로 이동

조회 페이지에서는 'Modify' 버튼을 통해서 수정/삭제 페이지로 이동하게 된다.<br>
수정/삭제 페이지에서는 다시 목록으로 가는 버튼이 존재하므로 동일하게 목록 페이지에 필요한 파라미터들을 처리해야 한다.<br>
BoardController에서는 get() 메서드에서 '/get'과 '/modify'를 같이 처리하므로 별도의 추가적인 처리 없이도 Criteria를 Model에 cri라는 이름으로 담아서 전달한다.<br>
<br>

조회 페이지에서 <form\>태그는 목록 페이지로의 이동뿐 아니라 수정/삭제 페이지 이동에도 사용되기 때문에 파라미터들은 잦동으로 같이 전송된다.<br>




<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
