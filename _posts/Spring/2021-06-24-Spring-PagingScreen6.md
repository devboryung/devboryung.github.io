---
title: "[Spring] 페이징 화면 처리"
excerpt: "Spring Framework - MyBatis에서 전체 데이터의 개수 처리"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## MyBatis에서 전체 데이터의 개수 처리

페이지의 이동이 모든 작업에서 정상적으로 이루어지는 것을 확인했다면 최종적으로는 데이터베이스에 있는 실제 모든 게시물의 수(total)를 구해서 PageDTO를 구성할 때 전달해 주어야 된다.<br>
전체의 개수를 구하는 SQL은 어렵거나 복잡하지 않다.<br>

어노테이션으로 처리해도 무방하지만 BoardMapper 인터페이스에 getTotalCount(0 메서드를 정의하고 XML을 이용해서 SQL을 처리한다.<br>

<br>

> BoardMapper

```java
public int getTotalCount(Criteria cri);
```

<br>

getTotalCount()는 Criteria를 파라미터로 전달받도록 설계하지 않아도 문제가 생기지 않지만, 게시물의 목록과 전체 데이터 수를 구하는 작업은 일관성 있게 Criteria를 받는 것이 좋다.<br>

<br>

> BoardMapper.xml


```xml
<select id="getTotalCount" resultType="int">
    select count(*) from tbl_board where bno >0
</select>
```
<br>

BoardService와 BoardServiceImpl에서는 별도의 메서드를 작성해서  BoardMapper의 getTotalCount()를 호출한다.<br>

<br>

> BoardService 인터페이스


```java
public int getTotal(Criteria cri);
```

<br>

> BoardServiceImpl

```java
@Override
public int getTotal(Criteria cri) {
    
    log.info("get total count");
    
    return mapper.getTotalCount(cri);
}
```
<br>


BoardController에서는 BoardService 인터페이스를 통해서 getTotal()을 호출하도록 변경한다.
<br><br>

> BoardController

```java
@GetMapping("/list")
public void list(Criteria cri, Model model) {
    
    log.info("list:" + cri);
    model.addAttribute("list", service.getList(cri));
    //model.addAttribute("pageMaker", new PageDTO(cri,123));
    
    int total = service.getTotal(cri);
    
    log.info("total:" + total);
    
    model.addAttribute("pageMaker", new PageDTO(cri, total));
    
}
```
<br><br>


<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
