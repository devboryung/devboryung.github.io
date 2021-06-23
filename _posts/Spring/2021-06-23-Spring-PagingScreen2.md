---
title: "[Spring] 페이징 화면 처리"
excerpt: "Spring Framework - 페이징 처리를 위한 클래스 설계"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 페이징 처리를 위한 클래스 설계

화면에 페이징 처리를 위해서 여러 정보가 필요하다면 클래스를 구성해서 처리하는 방식도 꽤 편한 방식이 될 수 있다.<br>

클래스를 구성하면 Controller 계층에서 JSP 화면에 전달할 때에도 객체를 생성해서 Model에 담아 보내는 과정이 단순해지는 장점도 있다.<br>

PageDTO 클래스를 설계한다.<br>

<br>

> PageDTO 클래스

```java
package org.zerock.domain;

import lombok.Getter;
import lombok.ToString;

@Getter
@ToString
public class PageDTO {
	
	private int startPage;
	private int endPage;
	private boolean prev, next;
	
	private int total;
	private Criteria cri;
	
	public PageDTO(Criteria cri, int total) {
		this.cri = cri;
		this.total = total;
		
		this.endPage = (int) (Math.ceil(cri.getPageNum()/10.0)) * 10;
		this.startPage = this.endPage - 9;
		
		int realEnd = (int)(Math.ceil((total*1.0) / cri.getAmount()));
		
		if(realEnd < this.endPage) {
			this.endPage = realEnd;
		}
		
		this.prev = this.startPage >1;
		this.next = this.endPage < realEnd;
	}
}
```
<br>

PageDTO는 생성자를 정의하고 Criteria와 전체 데이터 수(total)를 파라미터로 지정한다.<br>
Criteria 안에는 페이지에서 보여주는 데이터 수(amount)와 현재 페이지 번호(pageNum)를 가지고 있기 때문에 이를 이용해서 필요한 모든 내용을 계산할 수 있다.<br>

BoardController에서는 PageDTO를 사용할 수 있도록 Model에 담아서 화면에 전달해 줄 필요가 있다.<br>

<br>

> BoardController

```java
@GetMapping("/list")
public void list(Criteria cri, Model model) {
    
    log.info("list:" + cri);
    model.addAttribute("list", service.getList(cri));
    model.addAttribute("pageMaker", new PageDTO(cri,123));
}
```

<br>

list() 는 'pageMaker'라는 이름으로 PageDTO 클래스에서 객체를 만들어서 Model에 담아준다.<br>
PageDTO를 구성하기 위해서는 전체 데이터 수가 필요한데, 아직 그 처리가 이루어지지 ㅇ낳았으므로 임의의 값으로 123을 지정한다.<br>

<br>


<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
