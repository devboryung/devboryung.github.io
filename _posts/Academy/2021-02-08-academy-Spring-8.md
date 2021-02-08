---
title: "2020년 02월 08일"
excerpt: "Spring 6 (자유게시판ㆍ정보게시판 목록 조회, 자유게시판 상세 조회)"
search: true
categories: 
  - Academy
tags: 
  - FrameWork
  - Java
  - Spring
toc: true
---

## DB Model

![image](https://user-images.githubusercontent.com/70805241/107212732-bc0c9f00-6a4a-11eb-9fdc-9fe89ce3d4c1.png) <br><br>

## 파일, 패키지 생성

게시판과 관련된 파일들을 생성한다. <br> ![image](https://user-images.githubusercontent.com/70805241/107211162-81a20280-6a48-11eb-8c01-af6b9f6568b6.png) <br> ![image](https://user-images.githubusercontent.com/70805241/107211231-99798680-6a48-11eb-9ad5-f5623d2fd22a.png) <br> ![image](https://user-images.githubusercontent.com/70805241/107211349-c4fc7100-6a48-11eb-8c38-66123e40e19f.png) <br><br><br>


## mybatis-config.xml

```xml
<!-- 별칭 지정 -->
<typeAlias type="com.kh.spring.board.model.vo.Board" alias="Board"/>

<!-- mapper 지정 -->
<mapper resource="/mappers/board-mapper.xml"></mapper>
```

<br><br><br>

## 페이지 목록 조회

### header.jsp

`FREE BOARD`와 `INFORMATION BOARD` a 태그를 누르면 각각 자유게시판, 정보게시판으로 이동한다. <br><br> ![image](https://user-images.githubusercontent.com/70805241/107212189-eb6edc00-6a49-11eb-82a8-d11a0def06c1.png) <br><br><br>

```html
<li class="nav-item">
    <a class="nav-link" href="${contextPath}/board/list/1">Free Board</a>
</li>

<li class="nav-item">
    <a class="nav-link" href="${contextPath}/board/list/2">Information Board</a>
</li>
```

<br><br><br>

### PageInfo.java
페이징 처리에 필요한 값을 계산하는 메소드를 작성한다. <br>

```java

// 위 생략

private void makePageInfo() {
    maxPage = (int)Math.ceil(( (double)listCount / limit));
    
    startPage = (currentPage-1)/pageSize * pageSize + 1;
    
    endPage = startPage + pageSize - 1;
    
    if(maxPage > endPage) {
        endPage = startPage + pageSize - 1;
    }
    else {
        endPage = maxPage;
    }
}
```

<br><br><br>

### BoardController.java

페이징 처리를 위해 getPageInfo 메소드와 selectList 메소드를 작성한다. <br>

```java
// 게시글 목록 조회 Controller
@RequestMapping("list/{type}")
public String boardList(@PathVariable("type") int type,
                        @RequestParam(value="cp", required = false, defaultValue="1") int cp,
                        Model model) {
    
    PageInfo pInfo = service.getPageInfo(type, cp);

    List<Board> bList = service.selectList(pInfo);
    
    model.addAttribute("bList", bList);
    model.addAttribute("pInfo", pInfo);
    
    return "board/boardList";
}
```

<br>

- **@PathVariable** : @RequestMapping에 작성된 URL 경로에 있는 특정 값을 변수로 사용할 수 있게하는 어노테이션.

<br><br><br>

### BoardService.java (interface)

```java
/** 게시글 목록 조회 Service
    * @param pInfo
    * @return 
    */
public abstract List<Board> selectList(PageInfo pInfo);


/** 게시글 상세 조회 Service
    * @param boardNo
    * @param type 
    * @return
    */
public abstract Board selectBoard(int boardNo, int type);
```
<br><br><br>

### BoardServiceImpl.java

```java
@Override
public PageInfo getPageInfo(int type, int cp) {
    // 전체 게시글 수 조회 
    int listCount = dao.getListCount(type);
    
    return new PageInfo(cp, listCount, type);
}


// 게시글 목록 조회 Service 구현
@Override
public List<Board> selectList(PageInfo pInfo) {
    return dao.selectList(pInfo);
}
```
<br><br><br>


### board-mapper.xml

```xml
<!-- 특정 게시판 전체 게시글 수 조회 -->
<select id="getListCount" parameterType="_int" resultType="_int">
        SELECT COUNT(*) FROM V_BOARD
        WHERE BOARD_STATUS = 'Y'
        AND BOARD_CD = #{type}
</select>

<!-- 게시글 목록 조회  -->
<select id="selectList" parameterType="_int" resultMap="board_rm">
    SELECT BOARD_NO, BOARD_TITLE, MEMBER_ID, READ_COUNT,
                    CATEGORY_NM, BOARD_CREATE_DT, BOARD_NM
    FROM V_BOARD
    WHERE BOARD_STATUS = 'Y'
    AND BOARD_CD = #{type}
    ORDER BY BOARD_NO DESC
</select>
```

<br><br><br>


### boardList.jsp

`boardType` 1이 자유게시판, 2가 정보게시판이기 때문에 title 게시판 이름이 보여야할 곳에 choose 구문을 작성한다. <br>

```html
<title>			
    <c:choose>
		<c:when test="${pInfo.boardType == 1}">
			자유게시판
		</c:when>
		<c:otherwise>
			정보게시판
		</c:otherwise>
    </c:choose>
</title>

<!-- 생략 -->

<h1>
    <c:choose>
        <c:when test="${pInfo.boardType == 1}">
            자유게시판
        </c:when>
        <c:otherwise>
            정보게시판
        </c:otherwise>
    </c:choose>
</h1>
```
<br><br> 

- 자유게시판 <br> ![image](https://user-images.githubusercontent.com/70805241/107213552-edd23580-6a4b-11eb-9391-c8547c3592a9.png)  <br><br>
- 정보게시판 <br> ![image](https://user-images.githubusercontent.com/70805241/107213598-06425000-6a4c-11eb-8284-f95abd5b31e4.png)  <br><br><br>



## 게시글 상세 조회

### boardList.jsp

게시글 한 행을 클릭했을 때 해당 게시글 상세조회로 이동하도록 스크립트를 작성한다. <br> 

```html
<script>
    $("#list-table td").on("click", function(){
        var boardNo = $(this).parent().children().eq(0).text();
        var boardViewURL = "../${pInfo.boardType}/" + boardNo;
        location.href = boardViewURL;
    });
</script>
```

<br><br><br>

### BoardController.java

```java
//게시글 상세조회 Controller
@RequestMapping("{type}/{boardNo}")
public String boardView(@PathVariable("type") int type,
                        @PathVariable("boardNo") int boardNo,
                        Model model,
                        @RequestHeader(value="referer", required = false) String referer,
                        RedirectAttributes ra) {

    Board board = service.selectBoard(boardNo, type);
    String url = null;
    if(board != null) { 
        model.addAttribute("board", board);
        url = "board/boardView";
    } else {
        if(referer == null) {
            url = "redirect:../list/" + type;
        } else {
            url = "redirect:" + referer;
        }
        ra.addFlashAttribute("swalIcon", "error");
        ra.addFlashAttribute("swalTitle", "존재하지 않는 게시글입니다.");
    }
    return url;
}
```
<br>

- **@RequestHeader(name="referer") String referer** : HTTP 요청 헤더에 존재하는 "referer" 값을 얻어와 매개변수 String referer에 저장한다.


<br><br><br>


### BoardService.java (interface)

```java
/** 게시글 상세 조회 Service
    * @param boardNo
    * @param type 
    * @return
    */
public abstract Board selectBoard(int boardNo, int type);
```

<br><br><br>


### BoardServiceImpl.java

게시글 상세조회는 해당 게시글의 조회수 기능도 같이 구현한다. <br>

```java
// 게시글 상세 조회 Service 구현
@Transactional(rollbackFor = Exception.class)
@Override
public Board selectBoard(int boardNo, int type) {
    Board temp = new Board();
    temp.setBoardNo(boardNo);
    temp.setBoardCode(type);
    
    Board board = dao.selectBoard(temp);
    
    if(board != null) {
        int result = dao.increaseReadCount(boardNo);
        
        if(result > 0) { 
            board.setReadCount(board.getReadCount() + 1);
        }
    }
    return board;
}
```
<br><br><br>


### BoardDAO.java

```java
/** 게시글 상세 조회 DAO
    * @param temp
    * @return board
    */
public Board selectBoard(Board temp) {
    return sqlSession.selectOne("boardMapper.selectBoard", temp);
}


/** 게시글 조회수 증가 DAO
    * @param boardNo
    * @return result
    */
public int increaseReadCount(int boardNo) {
    return sqlSession.update("boardMapper.increaseReadCount", boardNo);
}
```

<br><br><br>


### board-mapper.xml

```xml
<!-- 게시글 상세 조회 -->
<select id="selectBoard" parameterType="Board" resultMap="board_rm">
    SELECT BOARD_NO, BOARD_TITLE, BOARD_CONTENT, 
                    MEMBER_ID, READ_COUNT, CATEGORY_NM,
                    BOARD_CREATE_DT, BOARD_MODIFY_DT,
                    BOARD_CD, BOARD_NM
    FROM V_BOARD 
    WHERE BOARD_STATUS = 'Y'
    AND BOARD_NO = #{boardNo}
    AND BOARD_CD = #{boardCode}
</select>


<!-- 게시글 조회수 증가 -->
<update id="increaseReadCount" parameterType="_int">
UPDATE BOARD SET
READ_COUNT = READ_COUNT + 1
WHERE BOARD_NO = #{boardNo}
</update>
```
<br><br>

![image](https://user-images.githubusercontent.com/70805241/107214893-ee6bcb80-6a4d-11eb-91ea-4ab02a43d766.png)

