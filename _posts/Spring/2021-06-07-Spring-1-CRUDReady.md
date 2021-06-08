---
title: "[Spring] 영속/비즈니스 계층의 CRUD 구현 준비"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---

영속 계층의 작업은 항상 다음과 같은 순서로 진행된다.<br>

- 테이블의 칼럼 구조를 반영하는 VO(Value Object)클래스의 생성
- MyBatis의 Mapper 인터페이스의 작성/XML 처리
- 작성한 Mapper 인터페이스의 테스트

<br>

위의 과정 전에 먼저 JDBC 연결을 테스트하는 과정을 거치는 것이 좋지만, SQL Developer의 연결 자체가 이미 JDBC 연결을 이용하기 때문에 예제에서는 별도의 과정을 생략하고 진행함.<br> 


## 영속 계층의 구현 준비

거의 모든 웹 애플리케이션의 최종 목적은 데이터베이스에 데이터를 기록하거나, 원하는 데이터를 가져오는 것이 목적이기 때문에 개발할 때 어느 정도의 설계가 진행되면 데이터베이스 관련 작업을 한다.<br>

<br><br>

### VO 클래스의 작성

VO 클래스를 생성하는 작업은 테이블 설계를 기준으로 작성하면 된다.<br>


> tbl_board 테이블 구성

![image](https://user-images.githubusercontent.com/73421820/121014017-7a132000-c7d4-11eb-84d7-dcc64f826fbf.png)
<br>

> BoardVO 클래스 생성

```java
package org.zerock.domain;

import java.util.Date;

import lombok.Data;

@Data
public class BoardVO {
	private Long bno;
	private String title;
	private String content;
	private String writer;
	private Date regdate;
	private Date updateDate;
}
```

<br>

BoardVO 클래스는 Lombok을 이용해서 생성자와 getter/setter, toString() 등을 만들어 내는 방식을 사용한다.<br> 이를 위해 @Data 어노테이션을 적용한다.<br>

<br><br>

### Mapper 인터페이스와 Mapper XML

MyBatis는 SQL을 처리하는데 어노테이션이나 XML을 이용할 수 있다.<br>
간단한 SQL이라면 어노테이션을 이용해서 처리하는 것이 무난하지만, SQL이 점점 복잡해지고 검색과 같이 상황에 따라 다른 SQL문이 처리되는 경우에는 어노테이션은 그다지 유용하지 못하다는 단점이 있다.<br>
XML의 경우 단순 텍스트를 수정하는 과정만으로 처리가 끝나지만, 어노테이션의 경우 코드를 수정하고 다시 빌드 하는 등의 유지 보수성이 떨어지는 이유로 기피하는 경우도 종종 있다.<br>
<br>

> Mapper 인터페이스

root-context.xml에 'org.zerock.mapper'패키지를 스캔하도록 설정한다.

```xml
<mybatis-spring:scan base-package="org.zerock.mapper"/>
```

<br>

Mapeer 인터페이스를 작성할 때는 리스트(select)와 등록(insert) 작업을 우선해서 작성한다.<br>

> BoardMapper 인터페이스

```java
package org.zerock.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Select;
import org.zerock.domain.BoardVO;

public interface BoardMapper {
	
	@Select("select * from tbl_board where bno>0")
	public List<BoardVO> getList();
}
```
<br>

BoardMapper 인터페이스를 작성할 때는 이미 작성된 BoardVO 클래스를 적극적으로 활용해서 필요한 SQL을 어노테이션의 속성값으로 처리할 수 있다.<br>
**SQL을 작성할 때는 반드시 ';'이 없도록 작성해야 한다.**<br>
SQL 뒤에 'where bno>0'과 같은 조건은 테이블을 검색하는데 bno라는 카럼 조건을 주어서 Primary Key를 이용하도록 유도하는 조건이다.<br>

> SQL Developer에서 실행한 결과

![image](https://user-images.githubusercontent.com/73421820/121023079-e5152480-c7dd-11eb-88a5-92090b7ccb30.png)

<br>

SQL Developer에서 먼저 확인하는 이유는 `SQL이 문제가 없이 실행 가능한지`를 확인하기 위한 용도와, 데이터베이스의 `commit을 하지 않았다면 나중에 테스트 결과가 달라지기 때문에` 이를 먼저 비교할 수 있도록 하기 위함이다.<br>

작성된 BoardMapper 인터페이스를 테스트 할 수 있게 테스트 클래스를 추가한다.<br>

> BoardMapperTests 클래스

```java
package org.zerock.mapper;

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
public class BoardMapperTests {
	
	@Setter(onMethod_ = @Autowired) 
	private BoardMapper mapper;
	
	@Test
	public void testGetLisT() {
		mapper.getList().forEach(board->log.info(board));
	}
}
```
<br>

BoardMapperTests 클래스는 스프링을 이용해서 BoardMapper 인터페이스의 구현체를 주입받아서 동작하게 한다.<br>

testGetList()의 결과는 SQL Developer에서 실행된 것과 동일해야만 정상적으로 동작한 것이다.<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121191669-258ca500-c8a7-11eb-9713-4570a3b75b86.png)

<br>

BoardMapperTests를 이용해서 테스트가 완료되면  Mapper XML파일을 생성한다.<br>

> BoardMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE mapper PUBLIC	 "-//mybatis.org//DTD Mapper 3.0//EN" "thhp://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="org.zerock.mapper.BoardMapper">
	<select id="getList" resultType="org.zerock.domain.BoardVO">
		<![CDATA[
			select * from tbl_board where bno > 0	
		]]>
	</select>
</mapper>
```
<br>

XMl 작성 시 \<mapper\>의 namespace 속성값을 Mapper 인터페이스와 동일한 이름을 주는 것에 주의하고, \<select\> 태그의 id 속성값은 메서드의 이름과 일치하게 작성한다.<br>
resultType 속성의 값은 select 쿼리의 결과를 특정 클래스의 객체로 만들기 위해서 설정한다.<br>
XML에 사용한 CDATA 부분은 XML에서 부등호를 사용하기 위해서 사용한다.<br>
<br>
XML에 SQL문이 처리되었으니 BoardMapper 인터페이스에 SQL은 제거해도 된다.<br>

> BoardMapper

```java
//@Select("select * from tbl_board where bno>0")
public List<BoardVO> getList();
```
<br>

인터페이스 수정 후에는 반드시 기존의 테스트 코드를 통해서 기존과 동일하게 동작하는지 확인해야 한다.<br>

> 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121194961-296df680-c8aa-11eb-802f-3b441500565c.png)



<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
