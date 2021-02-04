---
title: "2020년 01월 28일"
excerpt: "MyBatis 설정 등"
search: true
categories: 
  - Academy
tags: 
  - Servlet
  - Java
  - MyBatis
toc: true
---

## MyBatis란?
> 개발자가 지정한 SQL, 저장 프로시저 그리고 몇가지 고급 매핑을 지원하는 퍼시스턴스 프레임워크이다. 마이바티스는 JDBC로 처리하는 상당부분의 코드와 파라미터 설정및 결과 매핑을 대신해준다. 마이바티스는 데이터베이스 레코드에 원시타입과 Map 인터페이스 그리고 자바 POJO 를 설정해서 매핑하기 위해 XML과 어노테이션을 사용할 수 있다.[^MyBatis]

<br><br><br>


## MyBatis 설정

[![MyBatis_logo](https://mybatis.org/images/mybatis-logo.png)](https://mybatis.org/mybatis-3/ko/index.html "MyBatis 공식홈페이지") <br>
👆🏼 이미지 클릭 시 공식 홈페이지로 이동

<br><br><br>

### 라이브러리 다운

홈페이지 사이드 메뉴에서 시작하기를 누른 후 하단에 보이는 `mybatis-x.x.x.jar`를 클릭한다. <br>
![20210128_01](https://user-images.githubusercontent.com/70805241/106011631-ff3b4980-60fd-11eb-9b61-eff3f1acfa2e.JPG)

<br><br>

mybatis-3.5.6 zip 파일을 다운한다. <br>
![20210128_02](https://user-images.githubusercontent.com/70805241/106012035-622ce080-60fe-11eb-8ce9-1946796496ba.JPG)

<br><br>


zip 파일 압축을 해제한 후 최상단에 있는 `mybatis-3.4.5.jar` 파일을 프로젝트 라이브러리 경로에 넣어준다. <br>
![20210128_03](https://user-images.githubusercontent.com/70805241/106012577-e4b5a000-60fe-11eb-880e-f82e1fc53778.JPG) 

<br><br><br>


### source folder 생성

Java Resources > src 내에 새로운 source folder를 만든다. <br>
![20210128_04](https://user-images.githubusercontent.com/70805241/106013720-13804600-6100-11eb-86f9-8f24ba733840.JPG) <br>
![20210128_05](https://user-images.githubusercontent.com/70805241/106013726-14b17300-6100-11eb-8084-33879952e91b.JPG)
<br>
![20210128_06](https://user-images.githubusercontent.com/70805241/106013723-1418dc80-6100-11eb-949e-62702cf5846f.JPG)

<br><br><br>


### mybatis-config.xml
>XML설정파일에서 지정하는 마이바티스의 핵심이 되는 설정은 트랜잭션을 제어하기 위한 TransactionManager과 함께 데이터베이스 Connection인스턴스를 가져오기 위한 DataSource 를 포함한다.[^mybatis2]
<br><br>

mybatis를 사용하기 위해 위에서 만들었던 resoure 폴더에 `mybatis-config.xml`을 생성한다. <br>
![20210128_07](https://user-images.githubusercontent.com/70805241/106015167-89d17800-6101-11eb-9044-c214afb3e785.JPG)
<br>
![20210128_08](https://user-images.githubusercontent.com/70805241/106015472-dddc5c80-6101-11eb-8a22-17d3b253b2aa.JPG) <br>

![20210128_08](https://user-images.githubusercontent.com/70805241/106015472-dddc5c80-6101-11eb-8a22-17d3b253b2aa.JPG)
<br>

하단의 내용을 config 파일 안에 붙여넣는다.
![20210128_10](https://user-images.githubusercontent.com/70805241/106016146-8094db00-6102-11eb-90ab-10995eca5bfa.JPG) <br>

- **configuration** : MyBatis 설정과 관련된 세팅 내용을 선언하는 태그(영역)으로 configuration 내부에 작성되는 태그들의 순서가 중요하다. 
- **environments** : DB 연결 설정 정보를 작성하는 태그
- **transactionManager** : 트랜잭션의 권한이 누구에게 있는지 선언하는 태그 JDBC라고 명시되어 있으면 JAVA에서 수행한다는 의미다.
- **DB 설정** <br> ![20210128_11](https://user-images.githubusercontent.com/70805241/106017023-5f80ba00-6103-11eb-846e-9bd8f10bb316.JPG) <br>
- **mappers** : DB에서 수행될 SQL 구문을 담은 mapper 파일을 등록하는 태그

<br><br>

- MyBatis 기본 세팅
configuration 바로 밑에 settings 태그를 추가하여 작성한다. <br>

```xml
<settings>
<!-- 마이바티스 null 처리 세팅 
    마이바티스는 매핑되는 컬럼에 null 값이 있을 경우 오류를 발생시킨다.
    그래서 값이 null일 때 처리방법을 지정한다.
    value 지정 시 NULL은 반드시 대문자로 작성한다. -> 소문자로 작성 시 오류 발생!
-->
<setting name="jdbcTypeForNull" value="NULL"/>
</settings>
```
<br><br><br>

### MyBatisTemplate class 생성

Java Resources > src > common 클릭 후 Ctrl+n <br>
![20210128_12](https://user-images.githubusercontent.com/70805241/106018165-960b0480-6104-11eb-9c70-5da40a974206.JPG)<br>
![20210128_13](https://user-images.githubusercontent.com/70805241/106101658-88e02b00-6181-11eb-9be0-105031e90251.JPG)

<br><br>

class 생성 후 하단 내용을 작성한다.
```java
import org.apache.ibatis.session.SqlSession;

public class MybatisTemplate {

	public static SqlSession getSqlSession() {
		// SqlSession : 확장된 Connection
		SqlSession session = null;
		try {
			
		} catch (Exception e) {
			e.printStackTrace();
		}
		return session;
	}
}
```
<br>
그리고 try 부분에 공식 홈페이지에 있는 세줄을 복사해서 붙여넣는다.<br> 

![20210128_14](https://user-images.githubusercontent.com/70805241/106101990-11f76200-6182-11eb-8fd3-24fff9d542c3.JPG)
<br>

에러가 나는 부분은 import가 되지 않아 발생하는 에러이므로 import를 해주면 없어진다. `ctrl + shift + s`를 눌러 import를 진행하자.<br>

![20210128_15](https://user-images.githubusercontent.com/70805241/106102564-ee80e700-6182-11eb-9591-afc6754d2efc.JPG)
<br>

resource의 경우는 제일 첫 줄을 import 하는게 아니므로 주의하자. <br>

![20210128_16](https://user-images.githubusercontent.com/70805241/106102569-ef197d80-6182-11eb-83d0-1bbdcacd52ba.JPG)
<br>

import를 끝낸 후 본인의 개발환경에 맞게 복사, 붙여넣기 한 세줄을 수정한다. <br>

```java
String resource = "/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
session = new SqlSessionFactoryBuilder().build(inputStream).openSession(false);
```
<br>

- **SqlSessionFactoryBuilder** : SqlSession을 만들어 내는 공장을 만드는 객체
- **SqlSessionFactory** : SqlSessionFactoryBuilder.build()의 결과물로 SqlSessionFactory 공장에서 SqlSession을 만든다.
- **openSession(false)** : SqlSession을 만들 때 AutoCommit을 false 상태로 만든다.


<br><br><br>

## MyBatis 실습
MyBatis 실습은 기존에 JDBC 형식으로 만들었던 wsp 프로젝트 내 `Board` 패키지에서 수정하는 방식으로 진행한다. <br>
먼저 기존의 파일들을 보존하기 위해 DAO, Service 파일을 새로 생성한다. <br> ![20210128_17](https://user-images.githubusercontent.com/70805241/106107444-4838df80-618a-11eb-8a5d-d0e2768e27f0.JPG) <br>
<br>

그리고 `BoardController`에서 전역변수로 선언한 service 관련 코드를 수정한다. `BoardService`에서도 똑같이 DAO 관련 코드도 수정해주자.
```java
// BoardController
//BoardService service = new BoardService();
BoardService2 service = new BoardService2();

// BoardService
BoardDAO2 dao = new BoardDAO2();
```
<br><br>

그리고 마지막으로 Service와 DAO 파일 상단에 하단 import 코드를 작성한다.<br>
```java
import static com.kh.wsp.common.MybatisTemplate.*;
```
<br>


### 게시글 목록 조회
- `BoardController` <br>

```java
if(command.equals("/list.do")) {
	errorMsg = "게시판 목록 조회 과정에서 오류 발생";

	PageInfo pInfo = service.getPageInfo(cp);

	List<Board> bList = service.selectBoardList(pInfo);
	if(bList != null) {
		List<Attachment> fList = service.selectThumbnailList(pInfo);
	
		if(!fList.isEmpty()) {
			request.setAttribute("fList", fList);
		}
	}
path = "/WEB-INF/views/board/boardList.jsp";
request.setAttribute("bList", bList);
request.setAttribute("pInfo", pInfo);

view = request.getRequestDispatcher(path);
view.forward(request, response);
} 
```
<br><br>

- `getPageInfo(cp)` <br>

기존 코드
```java
public PageInfo getPageInfo(String cp) throws Exception {
	Connection conn = getConnection();
	int currentPage = cp == null ? 1 : Integer.parseInt(cp);
	int listCount = dao.getListCount(conn);
	close(conn);
	return new PageInfo(currentPage, listCount);
}
```
<br><br>

마이바티스 형식으로 작성된 코드
```java
public PageInfo getPageInfo(String cp) throws Exception {
	SqlSession session = getSqlSession ();
	int currentPage = cp == null ? 1 : Integer.parseInt(cp);
	int listCount = dao.getListCount(session);
	session.close();
	return new PageInfo(currentPage, listCount);
}
```

<br><br>

- `BoardDAO` <br>

```java
public int getListCount(Connection conn) throws Exception {
	int listCount = 0;
	String query = prop.getProperty("getListCount");
	try {
		stmt = conn.createStatement();
		
		rset = stmt.executeQuery(query);
		
		if(rset.next()) {
			listCount = rset.getInt(1);
		}
	}finally {
		close(rset);
		close(stmt);
	}
	return listCount;
}
```
<br><br>

마이바티스 형식으로 작성된 코드

```java
public int getListCount(SqlSession session) throws Exception {
	return session.selectOne(arg0);
}
```
<br>
selectOne() 메소드의 경우는 조회 결과가 단일 행일때 사용한다. 첫 번째 매개변수에서는 sql이 작성된 mapper의 이름. 태그 아이디를 적고 두 번째 매개변수에는 sql 수행 시 필요한 전달값을 적는다. 두 번째 매개변수는 생략이 가능하다. <br>

아직 SQL을 처리할 수 있는 mapper 파일이 없으므로 새로 생성한다. mapper 파일은 mappers 패키지 안에 board-mapper.xml 이란 이름으로 생성한다.<br>
![20210128_22](https://user-images.githubusercontent.com/70805241/106126916-59411b00-61a1-11eb-8cf3-f9b0639f45f0.JPG)
<br><br>

생성 후 하단의 코드를 붙여 넣는다. <br>
![20210128_23](https://user-images.githubusercontent.com/70805241/106127475-ee441400-61a1-11eb-9c20-5944220466dd.JPG)
<br><br>

그리고 다음과 같이 수정한다.<br>
```xml
<mapper namespace="boardMapper">

<select id="getListCount" resultType="_int">
   SELECT COUNT(*) FROM V_BOARD
	WHERE BOARD_STATUS = 'Y'
</select>
  
</mapper>
```
- **namespace** : mapper의 이름(별칭)
- **resultType** : resultSet 결과를 매핑해서 반환되는 타입을 지정. 타입 지정 시 별칭 또는 전체 클래스명(패키지명 + 클래스명)을 작성해야한다.<br>

<br>

이제 위에서 작성해놨던 코드를 다음과 같이 수정한다. <br>

```java
public int getListCount(SqlSession session) throws Exception {
	return session.selectOne("boardMapper.getListCount");
}
```

<br><br>

여기까지 작성한 mapper 파일을 등록하기 위해 `mybatis-config.xml`로 이동해서 개발 환경에 맞에 mapper 경로를 설정한다.

```xml
<mappers>
	<mapper resource="/mappers/board-mapper.xml"/>
</mappers>
```
<br><br>


이제 Service에서 마이바티스로 변환한 코드가 잘 실행되는지 테스트한다.<br>
```java
int listCount = dao.getListCount(session);
System.out.println("listCount : " + listCount); 
```
<br><br>

성공!<br>
![20210128_24](https://user-images.githubusercontent.com/70805241/106146751-6c141980-61ba-11eb-8d92-637c1ae675de.JPG)


<br><br>

그럼 이제 목록 조회 나머지 메소들을 모두 마이바티스 형식으로 변경한다.<br><br><br>


- `(Service) selectBoardList` <br>

기존 코드 <br>
```java
public List<Board> selectBoardList(PageInfo pInfo) throws Exception {
	Connection conn = getConnection();
	List<Board> bList = dao.selectBoardList(conn, pInfo);
	close(conn);
	return bList;
}
```
<br><br>

마이바티스
```java
public List<Board> selectBoardList(PageInfo pInfo) throws Exception {
	SqlSession session = getSqlSession();
	List<Board> bList = dao.selectBoardList(session, pInfo);
	session.close();
	return bList;
}
```
<br><br><br>


- `(DAO) selectBoardList` <br>
기존 코드
```java
public List<Board> selectBoardList(Connection conn, PageInfo pInfo) throws Exception{
	List<Board> bList = null;
	String query = prop.getProperty("selectBoardList");
	try {
		// SQL 구문 조건절에 대입할 변수 생성
		int startRow = (pInfo.getCurrentPage() - 1) * pInfo.getLimit() + 1; 
		int endRow = startRow + pInfo.getLimit() - 1; 
		pstmt = conn.prepareStatement(query);
		pstmt.setInt(1, startRow);
		pstmt.setInt(2, endRow);
		rset = pstmt.executeQuery();
		bList = new ArrayList<Board>();
		while(rset.next()) {
			Board board = new Board(
							rset.getInt("BOARD_NO"), 
							rset.getString("BOARD_TITLE"), 
							rset.getString("MEMBER_ID"), 
							rset.getInt("READ_COUNT"), 
							rset.getString("CATEGORY_NM"), 
							rset.getTimestamp("BOARD_CREATE_DT"));
			bList.add(board);
		}
	}finally {
		close(rset);
		close(pstmt);
	}
	return bList;
}
```

<br><br>

마이바티스
```java
public List<Board> selectBoardList(SqlSession session, PageInfo pInfo) throws Exception{
	List<Board> bList = null;
	// 마이바티스에서는 ROWNUM이 포함된 복잡한 Select문을 사용할 필요가 없도록 RowsBounds 객체를 제공해준다.

	// offset : 시작점
	// limit : 시작점으로부터 몇개까지

	int offset = (pInfo.getCurrentPage() - 1) * pInfo.getLimit();

	RowBounds rowBounds = new RowBounds(offset, pInfo.getLimit());

	// selectList() : 다중 행 결과를 자동적으로 List에 담아서 반환한다.
	// 첫 번째 매개변수 : mapper이름.태그 아이디
	// 두 번째 매개변수 : SQL에서 사용할 전달값 (없으면 null)
	// 세 번째 매개변수 : RowBounds 객체를 참조하는 변수

	bList = session.selectList("boardMapper.selectBoardList", null, rowBounds);


	return bList;
}
```
<br><br><br>

- `board-mapper.xml` <br>

기존 SQL

```xml
<entry key="selectBoardList">
SELECT * FROM 
    (SELECT ROWNUM RNUM, V.*
    FROM 
        (SELECT * FROM V_BOARD WHERE BOARD_STATUS='Y' ORDER BY BOARD_NO DESC) V )
WHERE RNUM BETWEEN ? AND ?
</entry>
```

<br>

마이바티스

```xml
<!-- *RowBounds를 사용했기 때문에 SELECT문을 단순하게 작성해도
  		지정된 offset부터 limit 개수만큼의 행만 조회된다.
	** 조회 결과가 다중열인 경우 JAVA 기본 자료형에는 담을 수 없다 -> VO 필요
  		resultType = "VO클래스 풀네임(패키지명 + 클래스명)"
	-> 별칭은 mybatis-config.xml에서 작성한다.
		   -->
<select id="selectBoardList" resultMap="board_rm">
	SELECT * FROM V_BOARD WHERE BOARD_STATUS = 'Y' ORDER BY BOARD_NO DESC
</select>
```

`board_rm`을 사용하기 위한 VO 별칭 먼저 작성하기 위해 mybatis-config.xml로 이동한다.

<br><br><br><br>

- `mybatis-config.xml` <br>

별칭 부분은 `settings` 태그 밑에 작성한다.
```xml
<!-- VO 별칭 지정  -->
<typeAliases>
	<typeAlias type="com.kh.wsp.board.model.vo.Board" alias="Board"/>
</typeAliases>
```
<br>

별칭을 지정해주었으면 resultMap을 만들러 board-mapper.xml로 이동한다. <br><br><br>

- `board-mapper.xml` <br>

마이바티스에서 가장 강력한 기능 중 하나로 ResultSet에서 데이터를 가져올 때 작성되는 JDBC 코드를 줄여주는 역할을 한다. DB 컬럼명과 VO 필드명이 다를 때 이를 매핑시키는 역할을 한다. DB 컬럼명과 VO 필드명이 같을 경우엔 resultMap 없이 resultType으로 자동 매핑이 된다.<br>


```xml
<mapper namespace="boardMapper">
<!-- 이 밑으로 작성한다. -->
<resultMap type="Board" id="board_rm">
	<id property="boardNo" column="BOARD_NO" /> <!-- PK 연결 -->
	
	<!-- PK를 제외한 모든 컬럼들
		property : VO 필드명, column : DB 컬럼명
	-->
	<result property="boardTitle" column="BOARD_TITLE" />
	<result property="boardCount" column="BOARD_COUNT" />
	<result property="memberId" column="MEMBER_ID" />
	<result property="readCount" column="READ_COUNT" />
	<result property="categoryName" column="CATEGORY_NM" />
	<result property="boardCreateDate" column="BOARD_CREATE_DT" />
	<result property="boardModifyDate" column="BOARD_MODIFY_DT" />
	<result property="boardStatus" column="BOARD_STATUS" />
</resultMap>
```

<br><br><br>


- `selectThumbnailList()`  <br>

마지막으로 썸네일 조회를 위한 메소드를 마이바티스 형식으로 바꾼다. 기존 코드와 다른 부분이 많아 Contorller 먼저 수정한다. <br>
<br>

- `BoardContorller` <br>

```java
if(bList != null) {
	// 추가
	List boardNoList = new ArrayList();
	
	for(Board b : bList) {
		boardNoList.add(b.getBoardNo() + "");
	}
	
	String boardNoStr = String.join(", ", boardNoList);
	// 추가 끝
															// 수정
	List<Attachment> fList = service.selectThumbnailList(boardNoList);
	
	if(!fList.isEmpty()) {
		request.setAttribute("fList", fList);
	}
}
```



- `(service) selectThumbnailList()` <br>

기존 코드
```java
public List<Attachment> selectThumbnailList(PageInfo pInfo) throws Exception {
	Connection conn = getConnection();
	List<Attachment> fList = dao.selectThumbnailList(conn, pInfo);
	close(conn);
	return fList;
}
```
<br>

마이바티스
```java
public List<Attachment> selectThumbnailList(String boardNoStr) throws Exception {
	SqlSession session = getSqlSession();
	List<Attachment> fList = dao.selectThumbnailList(session, pInfo);
	session.close();
	return fList;
}
```
<br><br>

- `(DAO) selectThumbnailList()` <br>

기존 코드
```java
public List<Attachment> selectThumbnailList(Connection conn, PageInfo pInfo) throws Exception{
	List<Attachment> fList = null;
	String query = prop.getProperty("selectThumbnailList");
	try {
		int startRow = (pInfo.getCurrentPage() -1) * pInfo.getLimit() + 1;
		int endRow = startRow + pInfo.getLimit() - 1;
		pstmt = conn.prepareStatement(query);
		pstmt.setInt(1, startRow);
		pstmt.setInt(2, endRow);
		rset = pstmt.executeQuery();
		
		fList = new ArrayList<Attachment>();
		
		while(rset.next()) {
			Attachment at = new Attachment();
			at.setFileName(rset.getString("FILE_NAME"));
			at.setParentBoardNo(rset.getInt("PARENT_BOARD_NO"));
			fList.add(at);
		}
	}finally {
		close(rset);
		close(pstmt);
	}
	return fList;
}
```
<br>

마이바티스
```java
public List<Attachment> selectThumbnailList(SqlSession session, String boardNoStr) throws Exception{
	return session.selectList("boardMapper.selectThumbnailList", boardNoList);
}
```

<br><br>

- `mybatis-config.xml` <br>

사용하기 편하게 VO에 별칭을 지정해준다.
```xml
<!-- 추가 -->
<typeAlias type="com.kh.wsp.board.model.vo.Attachment" alias="Attachment" />
```
<br><br>

- `board-mapper.xml` <br>

SQL과 resultMap을 작성한다. <br>

```xml
<!-- 썸네일 목록 조회(select / 다중 행 다중 열) -->
<select id="selectThumbnailList" resultMap="attachment_rm">
	SELECT * FROM aTTACHMENT
	WHERE PARENT_BOARD_NO
	IN (${boardNoStr})
	AND FILE_LEVEL = 0
</select>
```
얻어온 매개변수가 작성될 위치와 형태에 따라 `#{매개변수명}`, `${매개변수명}` 으로 필요한 위치에 작성할 수 있다. <br>

- **#{매개변수명}**
	- Statement, 전달 받은 값을 모양 그대로 사용하고자 할때 사용한다.
- **${매개변수명}**
	- PreparedStatement, 위치홀더(?)에 매개변수를 값으로써 지정할 때 사용한다. 
	- 주의사항 : 전달 받은 값이 String 형태이면 값 앞, 뒤에 ''(작은따옴표)가 붙는다.


<br>

```xml
<resultMap type="Attachment" id="attachment_rm">
	<id property="fileNo" column="FILE_NO"/>
	
	<result property="filePath" column="FILE_PATH"/>
	<result property="fileName" column="FILE_NAME"/>
	<result property="fileLevel" column="FILE_LEVEL"/>
	<result property="parentBoardNo" column="PARENT_BOARD_NO"/>
</resultMap>
```

<br><br><br>

제대로 조회가 되면 성공적으로 바뀐것이다! <br>
![image](https://user-images.githubusercontent.com/70805241/106265878-016ee680-626b-11eb-9b38-1257b592a9a4.png)


<br><br><br>

마이바티스가 대략 하루 반 정도 진도를 더 나갔는데 이렇게 기존에 작성했던 JDBC 메소드를 바꾸는 형식이라 따로 포스팅,,하진,,,않,,,,




[^MyBatis]: 출처 : [MyBatis 공식홈페이지](https://mybatis.org/mybatis-3/ko/index.html)
[^mybatis2]: 출처 : [MyBatis 공식홈페이지](https://mybatis.org/mybatis-3/ko/getting-started.html)

