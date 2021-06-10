---
title: "[Spring] 프레젠테이션(웹)계층의 CRUD 구현"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 프레젠테이션(웹)계층의 CRUD 구현

비즈니스 계층의 구현까지 모든 테스트가 진행되었다면 이제 프레젠테이션 계층인 웹의 구현이다.<br>
웹은 이전에 실습한 내용을 기준으로 현재 프로젝트에 반영해야 한다.<br>



<br><br>

### Controller의 작성

스프링 MVC의 Controller는 하나의 클래스 내에서 여러 메서드를 작성하고, @RequestMapping 등을 이용해서 URL을 분기하는 구조로 작성할 수 있기 때문에 하나의 클래스에서 필요한 만큼 메서드의 분기를 이용하는 구조로 작성한다.<br><br>

과거에는 이 단계에서 Tomcat(WAS)을 실행하고 웹 화면을 만들어서 결과를 확인하는 방식으로 코드를 작성했다.<br>
이 방식은 시간도 오래 걸리고 테스트를 자동화 하기에 어려움이 있다.<br>
따라서 이 단계에서는 WAS를 실행하지 않고 Controller를 테스트할 수 있는 방법을 학습해야 한다.<br>



<br><br>

**BoardController의 분석**

작성하기 전에는 반드시 현재 원하는 기능을 호출하는 방식에 대해 테이블로 정리한 후 코드를 작성하는 것이 좋다.<br>

![image](https://user-images.githubusercontent.com/73421820/121560402-6e349180-ca52-11eb-8445-4d6eea6df295.png)


<br>

테이블에서 From 항목은 해당 URL을 호출하기 위해서 별도의 입력화면이 필요하다는 것을 의미한다.<br>




<br><br>

### BoardController의 작성

BoardController 클래스를 작성한다.<br>

> BoardController 클래스

```java
package org.zerock.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import lombok.extern.log4j.Log4j;

@Controller
@Log4j
@RequestMapping("/board/*")
public class BoardController {
}
```

BoardController는 @Controller 어노테이션을 추가해서 스프링의 빈으로 인식할 수 있게 하고, @RequestMapping을 통해서 '/board'로 시작하는 모든 처리를 BoardController가 하도록 지정한다.<br>
BoardController가 속한 패키지는 servlet-context.xml에 기본으로 설정되어 있어서 별도의 설정이 필요하지 않다.<br>




<br><br>

**목록에 대한 처리와 테스트**

BoardController에서 전체 목록을 가져오는 처리를 먼저 작성한다.<br>
BoardController는 BoardService 타입의 객체와 같이 연동해야 하므로 의존성에 대한 처리도 같이 진행한다.<br><br>

> BoardController 클래스

```java
package org.zerock.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.zerock.service.BoardService;

import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Controller
@Log4j
@RequestMapping("/board/*")
@AllArgsConstructor
public class BoardController {
	
	private BoardService service;
	
	@GetMapping("/list")
	public void list(Model model) {
		log.info("list");
		
		model.addAttribute("list", service.getList());
	}

}
```
<br>

BoardController는 BoardService에 대해서 의존적이므로 @AllArgsConstructor를 이용해서 생성자를 만들고 자동으로 주입하도록 한다.(만약 생성자를 만들지 않을 경우에는 @Setter(onMethod_ = {@Autowired})를 이용해서 처리한다.)<br>

list()는 나중에 게시물의 목록을 전달해야 하므로 Model을 파라미터로 지정하고, 이를 통해서 BoardServiceImpl 객체의 getList() 결과를 담아 전달한다.<br>
BoardController 테스트는 스프링의 테스트 기능을 통해서 확인해 볼 수 있다.<br>
<br>

테스트 코드는 기존과 다르게 진행된다. 그 이유는 웹을 개발할 때 매번 URL을 테스트하리 위해서  Tomcat과 같은 WAS를 실행하는 불편한 단계를 생략하기 위해서이다.<br>
스프링의 테스트 기능을 활용하면 개발 당시에 Tomcat(WAS)을 실행하지 않고도 스프링과 웹 URL을 테스트 할 수 있다.<br>
<br>

WAS를 실행하지 않기 위해서는 약간의 추가적인 코드가 필요하지만 반복적으로 서버를 실행하고 화면에 입력하고, 오류를 수정하는 단계를 줄여줄 수 있기 때문에 Controller를 테스트할 때는 한 번쯤 고려해 볼 만한 방식이다.<br><br>


> BoardControllerTests 클래스

```java
package org.zerock.controller;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.web.WebAppConfiguration;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;

import lombok.Setter;
import lombok.extern.log4j.Log4j;


@RunWith(SpringJUnit4ClassRunner.class)
@WebAppConfiguration
@ContextConfiguration({"file:src/main/webapp/WEB-INF/spring/root-context.xml", "file:src/main/webapp/WEB-INF/spring/appServlet/servlet-context.xml"})
@Log4j
public class BoardControllerTests {
	
	@Setter(onMethod_ = @Autowired)
	private WebApplicationContext ctx;
	
	private MockMvc mockMvc;
	
	
	
	@Before
	public void setup() {
		this.mockMvc = MockMvcBuilders.webAppContextSetup(ctx).build();
	}
	
	
	@Test 
	public void testList() throws Exception{
		log.info(mockMvc.perform(MockMvcRequestBuilders.get("/board/list"))
				.andReturn()
				.getModelAndView()
				.getModelMap());
	}
}
```
<br>

테스트 클래스의 선언부에는 @WebAppConfiguration 어노테이션을 적용한다.<br>
`@WebAppConfiguration`은 Servlet의 ServletContext를 이용하기 위해서인데, 스프링에서는 WebApplicationContext라는 존재를 이용하기 위해서이다.<br>
`@Before` 어노테이션이 적용된 setUp()에서는 import 할 때 JUnit을 이용해야 한다.<br>
@Before가 적용된 메서드는 모든 테스트 전에 매번 실행되는 메서드가 된다.<br>
<br>

MockMvc는 말 그대로 `가짜 mvc`라고 생각하면 된다.<br> 
가짜로 URL과 파라미터 등을 브라우저에서 사용하는 것처럼 만들어서 Controller를 실행해 볼 수 있다.<br>
testList()는 MockMvcRequestBuilders라는 존재를 이용해서 GET 방식의 호출을 한다.<br>
이후에는 BoardController의 getList()에서 반환된 결과를 이용해서 Model에 어떤 데이터들이 담겨있는지 확인한다.<br>
<br>

> 실행 결과

데이터베이스에 저장된 게시물들을 볼 수 있다.<br>

```java
INFO : org.zerock.controller.BoardControllerTests - {list=[BoardVO(bno=22, title=새로 작성하는 글 select key, content=새로 작성하는 내용 select key, writer=newbie, regdate=Wed Jun 09 00:05:28 KST 2021, updateDate=Wed Jun 09 00:05:28 KST 2021), BoardVO(bno=23, title=새로 작성하는 글, content=새로 작성하는 내용, writer=newbie, regdate=Wed Jun 09 00:05:28 KST 2021, updateDate=Wed Jun 09 00:05:28 KST 2021), BoardVO(bno=1, title=테스트 제목, content=테스트 내용, writer=user00, regdate=Fri Jun 04 23:41:20 KST 2021, updateDate=Fri Jun 04 23:41:20 KST 2021), BoardVO(bno=2, title=테스트 제목, content=테스트 내용, writer=user00, regdate=Fri Jun 04 23:43:24 KST 2021, updateDate=Fri Jun 04 23:43:24 KST 2021), BoardVO(bno=25, title=새로 작성하는 글, content=새로 작성하는 내용, writer=newbie, regdate=Thu Jun 10 00:24:47 KST 2021, updateDate=Thu Jun 10 00:24:47 KST 2021), BoardVO(bno=4, title=테스트 제목, content=테스트 내용, writer=user00, regdate=Fri Jun 04 23:43:24 KST 2021, updateDate=Fri Jun 04 23:43:24 KST 2021), BoardVO(bno=5, title=수정된 제목, content=수정된 내용, writer=user00, regdate=Fri Jun 04 23:43:24 KST 2021, updateDate=Wed Jun 09 00:32:39 KST 2021), BoardVO(bno=21, title=새로 작성하는 글, content=새로 작성하는 내용, writer=newbie, regdate=Wed Jun 09 00:01:38 KST 2021, updateDate=Wed Jun 09 00:01:38 KST 2021), BoardVO(bno=24, title=새로 작성하는 글 select key, content=새로 작성하는 내용 select key, writer=newbie, regdate=Wed Jun 09 00:07:00 KST 2021, updateDate=Wed Jun 09 00:07:00 KST 2021)]}
INFO : org.springframework.web.context.support.GenericWebApplicationContext - Closing org.springframework.web.context.support.GenericWebApplicationContext@7791a895: startup date [Fri Jun 11 00:07:44 KST 2021]; root of context hierarchy
INFO : com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Shutdown initiated...
```
<br>




<br><br>


**등록 처리와 테스트**

BoardController에 POST방식으로 처리되는  register()를 작성하기<br><br>

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

register() 메서드는 조금 다르게 String을 리턴 타입으로 지정하고, RedirectAttributes를 파라미터로 지정한다.<br>
이는 등록 작업이 끝난 후 다시 목록 화면으로 이동하기 위함인데, 추가적으로 새롭게 등록된 게시물의 번호를 같이 전달하기 위해서 RedirectAttributes를 이용한다.<br>

리턴 시에는 'redirect:' 접두어를 사용하는데 이를 이용하면 스프링 MVC가 내부적으로  response.sendRedirect()를 처리해 주기 때문에 편리하다.<br>
<br>


> BoardControllerTests 클래스

```java
@Test
public void testRegister() throws Exception{
    
    String resultPage = mockMvc.perform(MockMvcRequestBuilders.post("/board/register")
            .param("title", "테스트 새글 제목")
            .param("content","테스트 새글 내용")
            .param("writer","user00")
            ).andReturn().getModelAndView().getViewName();
    
    log.info(resultPage);
}
```

<br>


테스트할 때 MockMvcRequestBuilder의 post()를 이용하면 POST방식으로 데이터를 전달할 수 있고, param()을 이용해서 전달해야 하는 파라미터들을 지정할 수 있다.(\<input\>태그와 유사한 역할)<br>

이러한 방식으로 코드를 작성하면 최초 작성 시에는 일이 많다고 느껴지지만 매번 입력할 필요가 없기 때문에 오류가 발생하거나 수정하는 경우 반복적인 테스트가 수월해 진다.<br>

<br>

> 실행 결과 

```java
INFO : jdbc.audit - 1. Connection.prepareStatement(insert into tbl_board (bno,title,content,writer)
		values (?, ?, ?, ?)) returned net.sf.log4jdbc.sql.jdbcapi.PreparedStatementSpy@36453307
INFO : jdbc.audit - 1. PreparedStatement.setLong(1, 41) returned 
INFO : jdbc.audit - 1. PreparedStatement.setString(2, "테스트 새글 제목") returned 
INFO : jdbc.audit - 1. PreparedStatement.setString(3, "테스트 새글 내용") returned 
INFO : jdbc.audit - 1. PreparedStatement.setString(4, "user00") returned 
INFO : jdbc.sqlonly - insert into tbl_board (bno,title,content,writer) values (41, '테스트 새글 제목', '테스트 새글 내용', 'user00') 

INFO : jdbc.sqltiming - insert into tbl_board (bno,title,content,writer) values (41, '테스트 새글 제목', '테스트 새글 내용', 'user00') 
 {executed in 6 msec}
INFO : jdbc.audit - 1. PreparedStatement.execute() returned false
INFO : jdbc.audit - 1. PreparedStatement.getUpdateCount() returned 1
INFO : jdbc.audit - 1. PreparedStatement.isClosed() returned false
INFO : jdbc.audit - 1. PreparedStatement.close() returned 
INFO : jdbc.audit - 1. Connection.clearWarnings() returned 
INFO : org.zerock.controller.BoardControllerTests - redirect:/board/list
```

<br>

실행되는 로그를 살펴보면 상단에 BoardVO 객체를 올바르게 데이터가 바인딩된 결과를 볼 수 있고, 중간에는 SQL의 실행 결과가 보인다.<br>
마지막에는 최종 반환 문자열을 확인할 수 있다.<br>

<br><br>


**조회 처리와 테스트**

등록 처리와 유사하게 조회 처리도 BoardController를 이용해서 처리할 수 있다.<br>
특별한 경우가 아니라면 조회는 GET 방식으로 처리하므로, @GetMapping을 이용한다.<br>
<br>

> BoardController 클래스

```java
@GetMapping("/get")
public void get(@RequestParam("bno") Long bno, Model model) {
    
    log.info("/get");
    model.addAttribute("board",service.get(bno));
}
```
<br>

BoardController의 get() 메서드에는 bno 값을 좀 더 명시적으로 처리하는 @RequestParam을 이용해서 지정한다.(파라미터 이름과 변수 이름을 기준으로 동작하기 때문에 생략해도 무방)<br>

또한 화면 쪽으로 해당 번호의 게시물을 전달해야 하므로 Model을 파라미터로 지정한다.<br>
<br>

> BoardControllerTests 클래스

```java
@Test
public void testGet() throws Exception{
    
    log.info(mockMvc.perform(MockMvcRequestBuilders.get("/board/get")
            .param("bno", "2"))
            .andReturn()
            .getModelAndView().getModelMap());
    
}
```
<br>

특정 게시물을 조회할 때 반드시 'bno'라는 파라미터가 필요하므로 param()을 통해서 추가하고 실행한다.<br>

<br>

> 실행 결과

```java
INFO : jdbc.audit - 1. PreparedStatement.close() returned 
INFO : jdbc.audit - 1. Connection.clearWarnings() returned 
INFO : org.zerock.controller.BoardControllerTests - {board=BoardVO(bno=2, title=테스트 제목, content=테스트 내용, writer=user00, regdate=Fri Jun 04 23:43:24 KST 2021, updateDate=Fri Jun 04 23:43:24 KST 2021), org.springframework.validation.BindingResult.board=org.springframework.validation.BeanPropertyBindingResult: 0 errors}
INFO : org.springframework.web.context.support.GenericWebApplicationContext - Closing org.springframework.web.context.support.GenericWebApplicationContext@7791a895: startup date [Fri Jun 11 00:46:54 KST 2021]; root of context hierarchy
INFO : com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Shutdown initiated...
```
<br>

파라미터가 제대로 수집되었는지 확인하고 SQL의 처리결과를 확인할 수 있다.<br>
마지막에는 Model에 담겨 있는 BoardVO 인스턴스의 내용을 살펴볼 수 있다.<br>

<br><br>

**수정 처리와 테스트**

수정 작업은 등록과 유사하다.<br>
변경된 내용을 수집해서 BoardVO 파라미터로 처리하고, BoardService를 호출한다.<br>
수정 작업을 시작하는 화면의 경우에는 GET 방식으로 접근하지만 실제 작업은 POST 방식으로 동작하므로 @PostMapping을 이용해서 처리한다.<br>

<br>

> BoardController 클래스

```java
@PostMapping("/modify")
public String modify(BoardVO board, RedirectAttributes rttr) {
    log.info("modify:" + board);
    
    if(service.modify(board)) {
        rttr.addFlashAttribute("result", "success");
    }
    
    return "redirect:/board/list";
}
```
<br>

service.modify()는 수정 여부를 boolean으로 처리하므로 이를 이용해서 성공한 경우에만 RedirectAttributes 에 추가한다.<br>

<br>

> BoardControllerTests 클래스

```java
@Test
public void testModify() throws Exception{
    
    String resultPage = mockMvc
            .perform(MockMvcRequestBuilders.post("/board/modify")
            .param("bno","1")
            .param("title", "수정된 테스트 새글 제목")
            .param("content", "수정된 테스트 새글 내용")
            .param("writer", "user00"))
            .andReturn().getModelAndView().getViewName();
    
    log.info(resultPage);
}
```
<br>

> 실행 결과

```java
INFO : jdbc.sqlonly - update tbl_board set title = '수정된 테스트 새글 제목', content='수정된 테스트 새글 내용', writer = 'user00', updateDate 
= sysdate where bno = 1 

INFO : jdbc.sqltiming - update tbl_board set title = '수정된 테스트 새글 제목', content='수정된 테스트 새글 내용', writer = 'user00', updateDate 
= sysdate where bno = 1 
 {executed in 9 msec}
INFO : jdbc.audit - 1. PreparedStatement.execute() returned false
INFO : jdbc.audit - 1. PreparedStatement.getUpdateCount() returned 1
INFO : jdbc.audit - 1. PreparedStatement.isClosed() returned false
INFO : jdbc.audit - 1. PreparedStatement.close() returned 
INFO : jdbc.audit - 1. Connection.clearWarnings() returned 
INFO : org.zerock.controller.BoardControllerTests - redirect:/board/list
```
<br>




<br><br>

**삭제 처리와 테스트**

삭제 처리도 조회와 유사하게 BoardController와 테스트 코드를 작성한다.<br>
삭제는 반드시 POST방식으로만 처리한다.<br>

<br>

> BoardController 클래스

```java
@PostMapping("/remove")
public String remove(@RequestParam("bno") Long bno, RedirectAttributes rttr) {
    
    log.info("remove...." + bno);
    
    if(service.remove(bno)) {
        rttr.addFlashAttribute("result", "success");
    }
    
    return "redirect:/board/list";
}
```
<br>

BoardController의 remove()는 삭제 후 페이지의 이동이 필요하므로 RedirectAttributes를 파라미터로 사용하고 'redirect'를 이용해서 삭제 처리 후에 다시 목록 페이지로 이동한다.<br>
<br>

> BoardControllerTests 클래스

```java	
@Test
public void testRemove() throws Exception{
    // 삭제 전 데이터베이스에 게시물 번호 확인할 것
    String resultPage = mockMvc.perform(MockMvcRequestBuilders.post("/board/remove")
            .param("bno","25")
            ).andReturn().getModelAndView().getViewName();
    log.info(resultPage);
}
```
<br>

MockMvc를 이용해서 파라미터를 전달할 때에는 문자열로만 처리해야 한다.<br>
테스트 전에 게시물의 번호가 존재하는지 확인하고 테스트를 실행한다.<br>
<br>

> 실행 결과

```java
INFO : jdbc.sqltiming - delete from tbl_board where bno = 25 
 {executed in 8 msec}
INFO : jdbc.audit - 1. PreparedStatement.execute() returned false
INFO : jdbc.audit - 1. PreparedStatement.getUpdateCount() returned 1
INFO : jdbc.audit - 1. PreparedStatement.isClosed() returned false
INFO : jdbc.audit - 1. PreparedStatement.close() returned 
```
<br>

<br><br>

경우에 따라서는 Controller에 대한 테스트 코드를 작성하는 것에 대해서 거부감을 가지는 경우도 많다.<br>
대부분은 일정에 여유가 없다는 이유로 테스트를 작성하지 않는 경우가 많은데 프로젝트를 진행하는 멤버들의 경험치가 낮을수록 테스트를 먼저 진행하는 습관을 가지는 것이 좋다.<br>
반복적으로 입력과 수정, WAS의 재시작 시간을 고려해보면 Controller에 대한 테스트를 진행하는 선택이 더 빠른 개발의 결과를 낳는 경우가 많다.<br>

<br><br>






관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
