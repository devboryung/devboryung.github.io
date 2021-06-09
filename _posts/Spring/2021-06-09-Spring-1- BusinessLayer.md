---
title: "[Spring] 비즈니스 계층"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 비즈니스 계층

비즈니스 계층은 고객의 요구사항을 반영하는 계층으로 프레젠테이션 계층과 영속 계층의 중간 다리 역할을 하게 된다.<br>
영속 계층은 데이터베이스를 기준으로 해서 설계를 나눠 구현하지만, 비즈니스 계층은 `로직을 기준`으로 해서 처리한다.<br>

<br>

'쇼핑몰에서 상품을 구매한다'고 가정할 때, 해당 쇼핑몰의 로직이 '물건을 구매한 회원에게는 포인트를 올려준다'고 하면 영속 계층의 설계는 '상품'과 '회원'으로 나누어서 설계하게 된다. 반면에 비즈니스 계층은 상품 영역과 회원 영역을 동시에 사용해서 하나의 로직을 처리하게 되므로 다음과 같은 구조를 만들게 된다.<br>
![image](https://user-images.githubusercontent.com/73421820/121373098-c481d180-c979-11eb-96a1-81c98c6fe810.png)<br>

현재 예제는 단일한 테이블을 이용하고 있기 때문에 위와 같은 구조는 아니다.<br>
하지만, 설계를 할 때는 원칙적으로 영역을 구분해서 작성해야 한다.<br>
일반적으로 비즈니스 영역에 있는 객체들은 '서비스(Service)'라는 용어를 많이 사용한다.<br>
<br>





<br><br>

### 비지니스 계층의 설정

설계를 할 때 각 계층 간의 연결은 인터페이스를 이용해서 느슨한(loose) 연결을 한다.<br>
게시물은 BoardService 인터페이스와 인터페이스를 구현한 BoardServiceImpl 클래스를 선언한다.<br>

> BoardService 인터페이스

```java
package org.zerock.service;

import java.util.List;

import org.zerock.domain.BoardVO;

public interface BoardService {
	
	public void register(BoardVO board);
	public BoardVO get(Long bno);
	public boolean modify(BoardVO board);
	public boolean remove(Long bno);
	public List<BoardVO> getList();

}
```

<br>

BoardService 메서드를 설계할 때 메서드 이름은 현실적인 로직의 이름을 붙이는 것이 관례이다.<br>
명백하게 반환해야 할 데이터가 있는 'select'를 해야하는 메서드는 리턴 타입을 지정할 수 있다.<br>
게시물은 특정한 게시물을 가져오는 get() 메서드와 전체 리스트를 구하는 getList()의 경우 처음부터 메서드의 리턴 타입을 결정해서 진행할 수 있다.<br>
<br>

BoardService 인터페이스를 구현하는 구현체는 BoardServiceImpl이라는 클래스로 작성한다.<br>
클래스의 상세 내용은 조금 미루고, 약간의 로그를 기록할 수 있는 정도의 코드를 작성한다.<br>

> BoardServiceImple 클래스

```java
package org.zerock.service;

import java.util.List;

import org.springframework.stereotype.Service;
import org.zerock.domain.BoardVO;
import org.zerock.mapper.BoardMapper;

import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@Service
@AllArgsConstructor
public class BoardServiceImpl implements BoardService {
	
	// spring 4.3 이상에서 자동 처리
	private BoardMapper mapper;
	
	@Override
	public void register(BoardVO board) {}

	@Override
	public BoardVO get(Long bno) {
		return null;
	}

	@Override
	public boolean modify(BoardVO board) {
		return false;
	}

	@Override
	public boolean remove(Long bno) {
		// TODO Auto-generated method stub
		return false;
	}

	@Override
	public List<BoardVO> getList() {
		return null;
	}
}
```
<br>

BoardServiceImpl 클래스에 가장 중요한 부분은 @Service라는 어노테이션이다.<br>
@Service는 계층 구조상 주로 비즈니스 영역을 담당하는 객체임을 표시하기 위해 사용한다.<br>
작성된 어노테이션은 패키지를 읽어 들이는 동안 처리된다.<br>
BoardServiceImpl 가 정상적으로 동작하기 위해서는 BoardMapper 객체가 필요하다.<br>

이는 @Autowired와 같이 직접 설정해 줄 수 있고, Setter를 이용해서 처리할 수도 있다.<br>
Lombok을 이용한다면 아래와 같은 방식으로 만들수도 있다.<br>

```java
@Log4j
@Service
public class BoardServiceImpl implements BoardServie{

    @Setter(onMethod_ = @Autowired)
    private BoardMapper mapper;
}
```
<br>

스프링 4.3부터는 단일 파라미터를 받는 생성자의 경우에는 필요한 파라미터를 자동으로 주입할 수 있다.<br>
@AllArgsContstructor는 모든 파라미터를 이용하는 생성자를 만들기 때문에 실제 코드는 아래와 같이 BoardMapper를 주입받는 생성자가 만들어지게 된다.<br>
![image](https://user-images.githubusercontent.com/73421820/121378612-525fbb80-c97e-11eb-88da-7e1199658c74.png)<br>
프로젝트 구조에서 클래스를 조사해 보면 스프링 4.3의 자동주입 기능으로 인해 앞의 그림과 같은 형태가 된다.<br>



<br><br>

**스프링의 서비스 객체 설정(root-context.xml)**

비즈니스 계층의 인터페이스와 구현 클래스가 작성되었다면, 이를 스프링의 빈으로 인식하기 위해서 root-context.xml에 @Service 어노테이션이 있는 org.zerock.service 패키지를 스캔하도록 추가해야 한다.<br>

프로젝트 생성 시 만들어진 root-context.xml의 네임스페이스 탭에서 context 항목을 추가한다.<br>

![image](https://user-images.githubusercontent.com/73421820/121379287-d5811180-c97e-11eb-8f92-cb7fe64fee2c.png)<br>

네임스페이스를 추가하면 해당 이름을 시작하는 태그들을 활용할 수 있다.<br>

> root-context.xml

```xml
<context:component-scan base-package="org.zerock.service">
</context:component-scan>
```

<br>


<br><br>

### 비즈니스 계층의 구현과 테스트

BoardMapper와 BoardService, BoardServiceImpl에 대한 구조 설정이 완료되었으므로 클래스를 작성해 테스트 작업을 진행한다.<br>

> BoardServiceTests 클래스

```java
package org.zerock.service;

import static org.junit.Assert.assertNotNull;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import lombok.Setter;
import lombok.extern.log4j.Log4j;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml")
@Log4j
public class BoardServiceTests {
	
	@Setter(onMethod_ = {@Autowired})
	private BoardService service;
	
	
	@Test
	public void testExist() {
		log.info(service);
		assertNotNull(service);
	}
}
```
<br>

BoardServiceTests의 첫 테스트는 BoardService 객체가 제대로 주입이 가능한지 확인하는 작업으로 시작한다.<br>
정상적으로 BoardService 객체가 생성되고 BoardMapper가 주입되었다면 BoardService 객체와 데이터베이스 관련 로그가 출력된다.<br>

```xml
INFO : org.zerock.service.BoardServiceTests - org.zerock.service.BoardServiceImpl@66fdec9
INFO : org.springframework.context.support.GenericApplicationContext - Closing org.springframework.context.support.GenericApplicationContext@18eed359: startup date [Thu Jun 10 00:13:07 KST 2021]; root of context hierarchy
INFO : com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Shutdown initiated...
```

<br>


<br><br>


**등록 작업의 구현과 테스트**

등록 작업은 BoardServiceImpl에서 파라미터로 전달되는 BoardVO 타입의 객체를 BoardMapper를 통해서 처리한다.<br>


> ServiceImple 클래스

```java
@Override
public void register(BoardVO board) {
    
    log.info("register...." + board);
    mapper.insertSelectKey(board);
    
}
```

<br>

BoardService는 void 타입으로 설계되었으므로 mapper.insertSelectKey()의 반환값인 int를 사용하지 않고 있지만, 필요하다면 예외 처리나 void 대신에 int 타입을 이용해서 사용할 수도 있다.<br>

mapper의 insertSelectKey()를 이용해서 나중에 생성된 게시물의 번호를 확인할 수 있게 작성했다.<br>

> BoardServiceTests 클래스

```java
@Test
public void testRegister() {
    
    BoardVO board = new BoardVO();
    
    board.setTitle("새로 작성하는 글");
    board.setContent("새로 작성하는 내용");
    board.setWriter("newbie");
    
    service.register(board);
    
    log.info("생성된 게시물의 번호 : " + board.getBno());
}
```

<br>

testRegister()의 테스트 결과는 다음과 같이 생성된 게시물의 번호를 확인할 수 있다.<br>

> 실행 결과

```java
INFO : jdbc.sqltiming - insert into tbl_board (bno,title,content,writer) values (25, '새로 작성하는 글', '새로 작성하는 내용', 'newbie') 
 {executed in 5 msec}
INFO : jdbc.audit - 1. PreparedStatement.execute() returned false
INFO : jdbc.audit - 1. PreparedStatement.getUpdateCount() returned 1
INFO : jdbc.audit - 1. PreparedStatement.isClosed() returned false
INFO : jdbc.audit - 1. PreparedStatement.close() returned 
INFO : jdbc.audit - 1. Connection.clearWarnings() returned 
INFO : org.zerock.service.BoardServiceTests - 생성된 게시물의 번호 : 25
```
<br>

<br><br>

**목록(리스트) 작업의 구현과 테스트**

BoardServiceImpl 클래스에서 현재 테이블에 저장된 모든 데이터를 가져오는 getList()는 아래와 같이 구현된다.<br>

> BoardServiceImple 클래스

```java
@Override
public List<BoardVO> getList() {
    log.info("getList....................");
    return mapper.getList();
}
```
<br>

> BoardServiceTests 클래스

```java
@Test
public void testGetList() {
    service.getList().forEach(board -> log.info(board));
}
```
<br>

테스트의 실행 결과로 등록 작업을 테스트할 때 추가된 데이터가 정상적으로 나오는지 확인한다.<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121388885-d0c05b80-c986-11eb-8d8b-7010e20f5760.png)<br>


<br><br>

**조회 작업의 구현과 테스트**

조회는 게시물의 번호가 파라미터이고 BoardVO의 인스턴스가 리턴이 된다.<br><br>

> BoardServiceImpl 클래스

```java
@Override
public BoardVO get(Long bno) {
    log.info("get............." + bno);
    
    return mapper.read(bno);
}
```
<br>

> BoardServiceTests 클래스

```java
@Test
public void testGet() {
    log.info(service.get(1L));
}
```
<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121389273-3280c580-c987-11eb-97d5-1bcc27e014ae.png)<br>



<br><br>

**삭제/수정 구현과 테스트**

삭제/수정은 메서드의 리턴 타입을 void로 설계할 수도 있지만 엄격하게 처리하기 위해서 Boolean 타입으로 처리한다.<br>

> BoardServiceImpl 클래스

```java
	@Override
	public boolean modify(BoardVO board) {

		log.info("modify..." + board);
		
		return mapper.update(board) == 1;
	}

	@Override
	public boolean remove(Long bno) {

		log.info("remove..." + bno);
		
		return mapper.delete(bno) == 1;
	}
```

<br>

정상적으로 수정과 삭제가 이루어지면 1이라는 값이 반환되기 때문에 '=='연산자를 이용해서 true/false를 처리할 수 있다.<br>

> BoardServiceTests 클래스

```java
@Test
public void testDelete() {
    // 게시물 번호의 존재 여부를 확인하고 테스트해야 된다.
    log.info("REMOVE RESULT: " + service.remove(2L));
}


public void testUpdate() {
    BoardVO board = service.get(1L);
    
    if(board == null) {
        return;
    }
    
    board.setTitle("제목 수정합니다.");
    log.info("MODIFY RESULT : " + service.modify(board));
}
```
<br>

testDelete()의 경우에는 해당 게시물이 존재할 때 true를 반환하는 것을 확인할 수 있고, testUpdate()의 경우에는 특정한 게시물을 먼저 조회하고, title 값을 수장한 후 이를 업데이트한다.<br>

<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
