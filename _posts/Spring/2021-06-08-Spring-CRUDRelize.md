---
title: "[Spring] 영속 영역의 CRUD 구현"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 영속 영역의 CRUD 구현

웹 프로젝트 구조에서 마지막 영역이 영속 영역이지만, 실제로 구현을 가장 먼저 할 수 있는 영역도 영속 영역이다.<br>
영속 영역은 기본적으로 CRUD 작업을 하기 때문에 테이블과 VO(DTO) 등 약간의 준비만으로도 비즈니스 로직과 무관하게 CRUD 작업을 작성할 수 있다.<br>
MyBatis는 내부적으로 JDBC의 PreparedStatement를 활용하고 필요한 파라미터를 처리하는 `?`에 대한 치환은 `#{속성}`을 이용해서 처리한다.<br>

<br><br>


### create(insert) 처리

tbl_board 테이블은 PK 칼럼으로 bno를 이용하고, 시퀀스를 이용해서 자동으로 데이터가 추가될 때 번호가 만들어지는 방식을 사용한다.<br>
이처럼 자동으로 PK 값이 정해지는 경우에는 2가지 방식으로 처리할 수 있다.<br>
- insert만 처리되고 생성된 PK 값을 알 필요가 없는 경우
- insert문이 실행되고 생성된 PK 값을 알아야 하는 경우

<br>

BoardMapper 인터페이스에는 위의 상황들을 고려해서 메서드를 추가 선언한다.<br>

> BoardMapper 인터페이스

```java
package org.zerock.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Select;
import org.zerock.domain.BoardVO;

public interface BoardMapper {
	
	//@Select("select * from tbl_board where bno>0")
	public List<BoardVO> getList();
	
	public void insert(BoardVO baord);
	
	public void insertSelectKey(BoardVO board);
}
```

<br>

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
	
	<insert id="insert">
		insert into tbl_board (bno, title, content, writer)
		values (seq_board.nextval, #{title}, #{content}, #{writer})
	</insert>
	
	<insert id="insertSelectKey">
		<selectKey keyProperty="bno" order="BEFORE" resultType="long">
			select seq_board.nextval from dual
		</selectKey>

		insert into tbl_board (bno,title,content,writer)
		values (#{bno}, #{title}, #{content}, #{writer})	
	</insert>
</mapper>
```

<br>

BoardMapper의 insert()는 단순히 시퀀스의 다음 값을 구해서 insert 할 때 사용한다.<br>
insert문은 몇 건의 데이터가 변경되었는지만을 알려주기 때문에 추가된 데이터의 PK 값을 알 수 없지만, 1번의 SQL 처리만으로 작업이 완료되는 장점이 있다.<br>
<br>

insertSelectKey()는 @SelectKey라는 MyBatis의 어노테이션을 이용한다.<br>
@SelectKey는 주로 PK 값을 미리(before) SQL을 통해서 처리해 두고 특정한 이름으로 결과를 보관하는 방식이다.<br>
@Insert 할 때 SQL문을 보면 #{bno}와 같이 이미 처리된 결과를 이용하는 것을 볼 수 있다.<br>
<br>

우선 insert()에 대한 테스트 메서드를 작성한다.<br>

> BoardMapperTests 클래스

```java
@Test
public void testInsert() {
    
    BoardVO board = new BoardVO();
    board.setTitle("새로 작성하는 글");
    board.setContent("새로 작성하는 내용");
    board.setWriter("newbie");
    
    mapper.insert(board);
    
    log.info(board);
}
```

<br>

테스트 코드 마지막에 log.info(board)를 작성한 이유는 Lombok이 만들어주는 toString()을 이용해서 bno 멤버 변수(인스턴스 변수)의 값을 알아보기 위함이다.<br>

<br>

> 실행 결과

```java
INFO : org.zerock.mapper.BoardMapperTests - BoardVO(bno=null, title=새로 작성하는 글, content=새로 작성하는 내용, writer=newbie, regdate=null, updateDate=null)
```
<br>
테스트 결과를 살펴보면 BoardVO 클래스의 toString()의 결과가 출력되는 것을 볼 수 있는데, bno의 값이 null로 비어 있는 것을 확인할 수 있다.<br>
<br>

@SelectKey를 이용한 테스트에 대한 메서드를 작성한다.<br>

> BoardMapperTest 클래스

```java
	@Test
	public void testInsertSelectKey() {
		
		BoardVO board = new BoardVO();
		board.setTitle("새로 작성하는 글 select key");
		board.setContent("새로 작성하는 내용 select key");
		board.setWriter("newbie");
		
		mapper.insertSelectKey(board);
		log.info(board);
		
	}
```
<br>

> 실행 결과

```java
INFO : jdbc.sqlonly - select seq_board.nextval from dual 

...생략...

INFO : jdbc.audit - 1. Connection.prepareStatement(insert into tbl_board (bno,title,content,writer)
		values (?, ?, ?, ?)) returned net.sf.log4jdbc.sql.jdbcapi.PreparedStatementSpy@33e01298

...생략...

INFO : jdbc.sqltiming - insert into tbl_board (bno,title,content,writer) values (24, '새로 작성하는 글 select key', '새로 작성하는 
내용 select key', 'newbie') 
```
<br>

실행되는 결과를 살펴보면 'select seq_board.nextval from dual'과 같은 쿼리가 먼저 실행되고 여기서 생성된 결과를 이용해서 bno 값으로 처리되는 것을 볼 수 있다.<br>
BoardMapper의 insertSelectKey()의 @Insert 문의 SQL을 보면 'insert into tbl_board (bno, title,content, writer) value(#{bno},#{title}, #{content}, #{writer})'와 같이 파라미터로 전달되는 BoardVO의 bno 값을 사용하게 되어 있다.<br>
테스트 코드의 마지막 부분을 보면 BoardVO 객체의 bno값이 이전과 달리 지정된 겂을 볼 수 있다.<br>
@SelectKey를 이용하는 방식은 SQL을 한 번 더 실행하는 부담이 있기는 하지만 자동으로 추가되는 PK 값을 확인해야 하는 상황에서는 유용하게 사용될 수 있다.<br>



<br><br>

### read(select) 처리

insert가 된 데이터를 조회하는 작업은 PK를 이용해서 처리하므로 BoardMapper의 파라미터 역시 BoardVO 클래스의 bno 타입 정보를 이용해서 처리한다.<br>

> BoardMapper 인터페이스

```java
package org.zerock.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Select;
import org.zerock.domain.BoardVO;

public interface BoardMapper {
	
	//@Select("select * from tbl_board where bno>0")
	public List<BoardVO> getList();
	
	public void insert(BoardVO baord);
	
	public void insertSelectKey(BoardVO board);
	
	public BoardVO read(Long bno);
}
```
<br>

> BoardMapper.xml

```xml
<select id="read" resultType="org.zerock.domain.BoardVO">
    select * from tbl_board where bon = #{bno}
</select>
```

<br>

MyBatis는 Mapper 인터페이스의 리턴 타입에 맞게 select의 결과를 처리하기 때문에 tbl_board의 모든 칼럼은 BoardVO의 'bno,title,content,writer,regdate,updateDate' 속성값으로 처리된다.<br>

좀 더 엄밀하게 말하면 MyBatis는 bno라는 칼럼이 존재하면 인스턴스의 'setBno()'롤 호출하게 된다.<br>

MyBatis의 모든 파라미터와 리턴 타입의 처리는 get 파라미터명(), set 칼럼명()의 규칙으로 호출된다.<br>
다만 위와 같이 #{속성}이 1개만 존재하는 경우에는 별도의 get 파라미터명()을 사용하지 않고 처리된다.<br>
<br>

현재 테이블에 존재하는 데이터의 bno 칼럼의 값을 이용해서 테스트 코드를 통해 확인한다.<br>

> BoardMapperTests 클래스

```java
@Test
public void testRead() {
    BoardVO board = mapper.read(5L);
    
    log.info(board);
}
```
<br>

mapper.read()를 호출할 경우에는 현재 테이블에 있는 데이터의 bno 값이 존재하는지 여부를 반드시 확인해야 한다.<br>

> 실행 결과

```java
INFO : jdbc.audit - 1. PreparedStatement.close() returned 
INFO : jdbc.audit - 1. Connection.clearWarnings() returned 
INFO : org.zerock.mapper.BoardMapperTests - BoardVO(bno=5, title=테스트 제목, content=테스트 내용, writer=user00, regdate=Fri Jun 04 23:43:24 KST 2021, updateDate=Fri Jun 04 23:43:24 KST 2021)
```

<br>



<br><br>

### delect 처리

특정한 데이터를 삭제하는 작업 역시 PK 값을 이용해서 처리하므로 조회하는 작업과 유사하게 처리된다.<br>
등록, 삭제, 수정과 같은 DML 작업은 '몇 건의 데이터가 삭제(혹은 수정)되었는지'를 반환할 수 있다.<br>

> BoardMapper 인터페이스

```java
package org.zerock.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Select;
import org.zerock.domain.BoardVO;

public interface BoardMapper {
	
	//@Select("select * from tbl_board where bno>0")
	public List<BoardVO> getList();
	
	public void insert(BoardVO baord);
	
	public void insertSelectKey(BoardVO board);
	
	public BoardVO read(Long bno);
	
	public int delete(Long bno);
}
```

<br>

> BoardMapper.xml

```xml
<delete id="delete">
    delete from tbl_board where bno = #{bno}
</delete>
```
<br>

delete()의 메서드 리턴 타입은 int로 지정해서 만일 정상적으로 데이터가 삭제되면 1 이상의 값을 가지도록 작성한다.<br>
테스트 코드는 현재 테이블에 존재하는 번호의 데이터를 삭제해 보고 '1'이라는 값이 출력되는지 확인한다.<br>
만약 해당 번호의 게시물이 없다면 '0'이 출력된다.<br>

> BoardMapperTests 클래스

```java
@Test
public void testDelete() {
    log.info("DELETE COUNT : " + mapper.delete(3L));
}
```
<br>

testDelete()의 경우 3번 데이터가 존재했다면 다음과 같은 로그가 기록된다.<br>

> 실행 결과

```java
INFO : jdbc.sqltiming - delete from tbl_board where bno = 3 
 {executed in 8 msec}
INFO : jdbc.audit - 1. PreparedStatement.execute() returned false
INFO : jdbc.audit - 1. PreparedStatement.getUpdateCount() returned 1
INFO : jdbc.audit - 1. PreparedStatement.isClosed() returned false
INFO : jdbc.audit - 1. PreparedStatement.close() returned 
INFO : jdbc.audit - 1. Connection.clearWarnings() returned 
INFO : org.zerock.mapper.BoardMapperTests - DELETE COUNT : 1
```

<br>

<br><br>


### update 처리

마지막으로 update를 처리한다. 게시물의 업데이트는 제목, 내용, 작성자를 수정한다고 가정한다.<br>
업데이트할 때는 최종 수정시간을 데이터베이스 내 현재 시간으로 수정한다.<br>
Update는 delete와 마찬가지로 '몇 개의 데이터가 수정되었는가'를 처리할 수 있게 int 타입으로 메서드를 설계할 수 있다.<br>

> BoardMapper 인터페이스

```java
package org.zerock.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Select;
import org.zerock.domain.BoardVO;

public interface BoardMapper {
	
	//@Select("select * from tbl_board where bno>0")
	public List<BoardVO> getList();
	
	public void insert(BoardVO baord);
	
	public void insertSelectKey(BoardVO board);
	
	public BoardVO read(Long bno);
	
	public int delete(Long bno);
	
	public int update(BoardVO board);
}
```
<br>


> BoardMapper.xml

```xml
<update id="update">
    update tbl_board
    set title = #{title},
    content=#{content},
    writer = #{writer},
    updateDate = sysdate
    where bno = #{bno}
</update>
```
<br>

SQL에서 주의 깊게 봐야 하는 부분은 update 칼럼이 최종 수정시간을 의미하는 칼럼이기 때문에 현재 시간으로 변경해 주고 있다는 점과, regdate 칼럼은 최초 생성 시간이므로 건드리지 않는다는 점이다.<br>

#{title}과 같은 부분은 파라미터로 전달된 BoardVO 객체의 getTitle()과 같은 메서드들을 호출해서 파라미터들이 처리된다.<br>

<br>

테스트 코드는 read()를 이용해서 가져온 BoardVO 객체의 일부를 수정하는 방식이나 직접 BoardVO 객체를 생성해서 처리할 수 있다.<br>

> BoardMapperTests 클래스

```java
@Test
public void testUpdate() {
    
    BoardVO board = new BoardVO();
    
    //  실행 전 존재하는 번호인지 확인할 것
    board.setBno(5L);
    board.setTitle("수정된 제목");
    board.setContent("수정된 내용");
    board.setWriter("user00");
    
    int count = mapper.update(board);
    log.info("UPDATE COUNT : " + count);
    
}
```
<br>

> 실행 결과

```java
INFO : jdbc.sqlonly - update tbl_board set title = '수정된 제목', content='수정된 내용', writer = 'user00', updateDate = sysdate 
where bno = 5 

INFO : jdbc.sqltiming - update tbl_board set title = '수정된 제목', content='수정된 내용', writer = 'user00', updateDate = sysdate 
where bno = 5 
 {executed in 8 msec}
INFO : jdbc.audit - 1. PreparedStatement.execute() returned false
INFO : jdbc.audit - 1. PreparedStatement.getUpdateCount() returned 1
INFO : jdbc.audit - 1. PreparedStatement.isClosed() returned false
INFO : jdbc.audit - 1. PreparedStatement.close() returned 
```

<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
