---
title: "[Spring] MyBatis와 스프링에서 페이징 처리"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## MyBatis와 스프링에서 페이징 처리

MyBatis는 SQL을 그대로 사용할 수 있기 때문에 인라인뷰를 이용하는 SQL을 작성하고, 필요한 파라미터를 지정하는 방식으로 페이징 처리를 하게 된다.<br>
여기서 신경 써야하는 점은 페이징 처리를 위해서는 SQL을 실행할 때 몇 가지 파라미터가 필요하다는 점이다.<br>

페이징 처리를 위해서 필요한 파라미터는 `1)`페이지 번호(pageNum), `2)`한 페이지당 몇 개의 데이터(amount)를 보여줄 것인지가 결정되어야만 한다.<br>
<br>

페이지 번호와 몇 개의 데이터가 필요한지를 별도의 파라미터로 전달하는 방식도 나쁘지는 않지만, 아예 이 데이터들을 하나의 객체로 묶어서 전달하는 방식이 나중을 생각하면 좀 더 확장성이 좋다.<br>
<br>

>  Criteria 클래스 생성

```java
package org.zerock.domain;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
public class Criteria {
	
	private int pageNum;
	private int amount;

	public Criteria() {
		this(1,10);
	}
	
	public Criteria(int pageNum, int amount) {
		this.pageNum = pageNum;
		this.amount = amount;
	}

}
```
<br>

Criteria 클래스의 용도는 pageNum과 amount 값을 같이 전달하는 용도지만 생성자를 통해서 기본값을 1페이지, 10개로 지정해서 처리한다.<br>
Lombok을 이용해서 getter/setter을 생성해준다.<br>


<br><br>


### MyBatis 처리와 테스트

BoardMapper는 인터페이스와 어노테이션을 이용하기 때문에 페이징 처리와 같이 경우에 따라 SQL 구문 처리가 필요한 상황에서는 복잡하게 작성된다.(SQL문이 길어지고 복잡해지면 XML로 처리하는 것이 더 알아보기 쉽고 관리하기 쉬운 이유)<br>

BoardMapper에는 위에서 작성한 Criteria 타입을 파라미터로 사용하는 getListWithPaging()메서드를 작성한다.<br><br>


> BoardMapper 인터페이스

```java
public List<BoardVO> getListWithPaging(Criteria cri);
```

<br>

기존에 만들어둔 BoardMapper.xml에 getListWithPaging에 해당하는 태그를 추가한다. <br>
<br>


> BoardMapper.xml

```xml
<select id="getListWithPaging" resultType="org.zerock.domai.BoardVO">
    <![CDATA[
        select
            bno, title, content, writer, regdate, updatedate
        from
            (
                select /*+ INDEX_DESC(tbl_board pk_board) */
                rownum rn, bno, title, content, writer, regdate, updatedate
                from
                tbl_board
                where rownum <=20
            )
        where rn>10	
    ]]>
</select>
```
<br>

작성된 BoardMapper.xml에서는 XML의 CDATA 처리가 들어간다.<br>
CDATA 섹션은 XML에서 사용할 수 없는 부등호를 사용하기 위함이다.<br>
XML을 사용할 경우에는 '<, >'는 태그로 인식하는데, 이로 인해 생기는 문제를 막기 위함이다.<br>

인라인뷰에서는 BoardVO를 구성하는데 필요한 모든 칼럼과 ROWNUM을 RN이라는 가명을 이용해서 만들어 주고 바깥쪽 SQL에서는 RN컬럼을 조건으로 처리한다.<br>

<br><br>


**페이징 테스트와 수정**

MyBatis의 '#{}'를 적용하기 전에 XML 설정이 제대로 동작하는지 테스트를 먼저 진행하는 것이 좋다.<br>
테스트 환경은 이미 준비되어 있으므로 간단히 테스트 코드만을 추가할 수 있다.<br>
<br>

> BoardMapperTests 클래스

```java
@Test
public void testPaging() {
    
    Criteria cri = new Criteria();
    List<BoardVO> list = mapper.getListWithPaging(cri);
    list.forEach(board -> log.info(board));
}
```
<br>

Criteria 클래스에서 생성된 객체는 pageNum은 1, amount는 10이라는 기본값을 가지므로 별도의 파라미터 없이 생성한다.<br>

현재는 파라미터의 값이 반영되지 않았으므로 2페이지의 내용이 정상적으로 나오는지 확인한다.<br>

<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/122565686-b9b5f380-d081-11eb-922b-925f92b90b78.png)<br>


<br>

SQL에 문제가 없다는 것을 확인했다면 이제 Criteria 객체 내부의 값을 애용해서 SQL이 동작하도록 수정한다.<br>
20과 10이라는 값은 결국 pageNum과 amount를 이용해서 조절되는 값이다.<br>

<br>

> BoardMapper.xml

페이지 번호와 데이터 수를 변경할 수 있게 수정한다

```xml
<select id="getListWithPaging" resultType="org.zerock.domain.BoardVO">
    <![CDATA[
        select
            bno, title, content, writer, regdate, updatedate
        from
            
                (select /*+ INDEX_DESC(tbl_board pk_board) */
                rownum rn, bno, title, content, writer, regdate, updatedate
                from
                tbl_board
                where rownum <=#{pageNum} * #{amount})
            
        where rn> (#{pageNum}-1) * #{amount}	
    ]]>
</select>
```

<br>

SQL의 동작에 문제가 없는지 확인하기 위해 testPaging()을 수정한다.<br>

<br>

> BoardMapperTests 클래스

```java
@Test
public void testPaging() {
    
    Criteria cri = new Criteria();
    
    // 10개씩 3페이지
    cri.setPageNum(3);
    cri.setAmount(10);
    
    List<BoardVO> list = mapper.getListWithPaging(cri);
    list.forEach(board -> log.info(board.getBno()));
}
```

<br>

확인을 위해서 Criteria 객체를 생성할 때 파라미터를 추가해보거나, setter를 이용해서 내용을 수정한다.<br>
위의 경우는 한 페이지당 10개씩 출력하는 3페이지에 해당하는 데이터를 구한 것이다.<br>
테스트 코드가 동작한 후에는 SQL Developer에서 실행된 결과와 동일한지 체크하고 페이지 번호를 변경해서 정상적으로 번호가 처리되는지 확인한다.<br>

<br>

> 실행 결과


1페이지<br>

![image](https://user-images.githubusercontent.com/73421820/122566448-845dd580-d082-11eb-8835-c6c3ce10f10f.png)<br>


2페이지<br>

![image](https://user-images.githubusercontent.com/73421820/122566392-77d97d00-d082-11eb-9d48-4e27c5ff2719.png)<br>


3페이지<br>
![image](https://user-images.githubusercontent.com/73421820/122566342-698b6100-d082-11eb-9ceb-7fa95105c60c.png)<br>


<br><br>

### BoardController와 BoardService 수정

페이징 처리는 브라우저에서 들어오는 정보들을 기준으로 동작하기 때문에 Board Controller와 BoardService 역시 전달되는 파라미터들을 받는 형태로 수정해야 한다.

<br><br>

**BoardService 수정**

BoardService는 criteria를 파라미터로 처리하도록 BoardService 인터페이스와 BoardServiceImple 클래스를 수정해야 한다.<br>

> BoardService 인터페이스 

```java
//public List<BoardVO> getList();
public List<BoardVO> getList(Criteria cri);
```


<br>

> BoardServiceImple 클래스

```java
@Override
public List<BoardVO> getList(Criteria cri) {
    
    log.info("get List with criteria: " + cri);

    return mapper.getListWithPaging(cri);
}
```


<br>

원칙적으로는 BoardService 쪽에 대한 수정이 이루어 졌으니 이에 대한 테스트를 진행한다.<br>

메서드를 수정하면 이미 테스트 코드 역시 에러가 발생하므로 다음과 같이 수정해서 테스트를 진행한다.<br>
<br>


> BoardServiceTests 클래스


```java
@Test
public void testGetList() {
    //service.getList().forEach(board -> log.info(board));
    
    service.getList(new Criteria(2,10)).forEach(board->log.info(board));
}
```

<br>
<br><br>

**BoardController 수정**

기존 BoardController의 list()는 아무런 파라미터가 없이 처리되었기 때문에 pageNum과 amount를 처리하기 위해서 아래와 같이 처리한다.<br><br>

> BoardController 클래스

```java
//	@GetMapping("/list")
//	public void list(Model model) {
//		log.info("list");
//		
//		model.addAttribute("list", service.getList());
//	}
	
	@GetMapping("/list")
	public void list(Criteria cri, Model model) {
		
		log.info("list: " + cri);
		
		model.addAttribute("list", service.getList(cri));
	}
```
<br>

Criteria 클래스를 하나 만들어 두면 위와 같이 편하게 하나의 타입만으로 파라미터나 리턴 타입을 사용할 수 있기 때문에 여러모로 편리하다.<br>

BoardController 역시 이전에 테스트를 진행했으므로, pageNum과 amount를 파라미터로 테스트 한다.<br><br>

> BoardControllerTests 클래스

```java
@Test
public void testListPaging() throws Exception{
    
    log.info(mockMvc.perform(
            MockMvcRequestBuilders.get("/board/list")
            .param("pageNum", "2")
            .param("amount", "50"))
            .andReturn().getModelAndView().getModelMap());
}
```
<br>


<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
