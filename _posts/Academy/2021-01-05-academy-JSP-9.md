---
title: "2020년 01월 05일"
excerpt: "WSP-7"
search: true
categories: 
  - Academy
tags: 
  - Servlet
  - Java
  - HTML
  - CSS
toc: true
---

## 필터
<br>

*필터 생성*
![필터 생성](https://user-images.githubusercontent.com/73421820/103594053-e2668880-4f3a-11eb-8052-f24993612a4e.png)
<br><br><br>

### 필터란?
요청이나 응답을 해줄 때 
클라이언트와 서버(서블릿) 사이에서 데이터를 가공해
응답을 받을 때 가공된 데이터를 받게해주는 역할을 한다. <br>

>@WebFilter 어노테이션 :  어떤 요청이 왔을 때 필터를 동작하게 할건지? <br>
여러 주소를 적고싶다면  주소창안에 urlPatterns = {} 를 작성해 주소를 ,로 구분해서 작성해준다.

<br>

### 로그인 필터
>로그인에 성공해야 마이페이지의 기능을 사용할 수 있다. <br>
>필터처리를 안 해주면 로그아웃 상태에서도 주소창을 이용해 마이페이지에 접근할 수 있다.
>>--> 로그인 필터 생성
>요청이 들어왔을 때 로그인이 되어있는지 검사해주는 역할

<br><br>

LoginFilter.java

```java
package com.kh.wsp.filter;

import java.io.IOException;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import com.kh.wsp.member.model.vo.Member;

// 이 요청들은 로그인을 해야지만 수행할 수 있는 주소들이다.
@WebFilter(urlPatterns = 
				{"/member/myPage.do", "/member/changePwd.do", "/member/secession.do", 
				 "/member/updateMember.do", "/member/updatePwd.do","/member/updateStatus.do"})
public class LoginFilter implements Filter {

    public LoginFilter() {} // 기본 생성자
    public void init(FilterConfig fConfig) throws ServletException {}
	public void destroy() {}

	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		// 필터링 동작을 작성하는 부분
		// ServletRequest, ServletResponse -> HttpRequest,HttpResponse의 부모
		// ServletRequest 매개변수를 HttpServletRequest로 다운캐스팅 진행
		HttpServletRequest req = (HttpServletRequest)request;
		HttpServletResponse res = (HttpServletResponse)response;
		
		// Session 얻어오기
		HttpSession session = req.getSession();
		// Session을 얻어오는 이유? 로그인한 회원정보가 저장되어 있음.
		// 로그인이 되었는지 알기 위해서 sesion에 있는 로그인된 회원 정보를 얻어와야 된다.
		Member loginMember = (Member)session.getAttribute("loginMember");
		
		if(loginMember == null) { // 로그인이 되어있지 않음
			// 메인페이지로 강제 이동
			res.sendRedirect(req.getContextPath());
		}else {
			chain.doFilter(request, response);
			// 다음 필터를 호출하는 역할
			// 다음 필터가 없다면 Servlet 또는 JSP로 이동
		}
	}
}
```
<br>

### 관리자 필터
관리자로  로그인 됐을 때만 수행할 수 있는 기능들

AdminFilter.java

```java
package com.kh.wsp.filter;

import java.io.IOException;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import com.kh.wsp.member.model.vo.Member;


@WebFilter(urlPatterns= {"/notice/insertForm.do", "/notice/insert.do", "/notice/updateForm.do", 
						 "/notice/update.do", "/notice/delete.do"})
public class AdminFilter implements Filter {

  
    public AdminFilter() {}

	public void destroy() {}
	public void init(FilterConfig fConfig) throws ServletException {}

	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		
		HttpServletRequest req = (HttpServletRequest)request;
		HttpServletResponse res = (HttpServletResponse)response;
		
		HttpSession session = req.getSession();
		
		Member loginMember = (Member)session.getAttribute("loginMember");
		
		if(loginMember == null || !loginMember.getMemberGrade().equals("A") ) { 
			// 로그인이 안 되어 있거나 회원 등급이 'A'가 아닌 경우
			
			// 메인페이지로 강제 이동
			res.sendRedirect(req.getContextPath());
		}else {
			chain.doFilter(request, response);
		}
	}
}
```
<br><br><br>

## 게시판
<br>

### 게시판 목록 조회
<br>

header.jsp

```html
<li class="nav-item"><a class="nav-link" href="${contextPath }/board/list.do">Board</a></li>
```
<br>

*게시판 목록 페이징 처리*

PageInfo.java <br>
페이징 처리에 필요한 vo

```java
package com.kh.wsp.board.model.vo;

public class PageInfo { // 페이징 처리를 위한 값을 저장할  객체
	
	// 얻어 올 값
	private int currentPage; // 현재 페이지 번호를 저장할 변수
	private int listCount; // 전체 게시글 수를 저장할 변수
	
	// 설정할 값
	private int limit = 10; // 한 페이지에 보여질 게시글 목록 수 
	private int pageSize = 10; // 페이징바에 표시될 페이지 수
	
	
	// 계산할 값
	private int maxPage;    // 전체 목록 페이지의 수 == 마지막 페이지
	private int startPage;  // 페이징바 시작 페이지 번호
	private int endPage;    // 페이징바 끝 페이지 번호
	
	// 기본 생성자 사용 X
	
	
	public PageInfo(int currentPage, int listCount) {
		super();
		this.currentPage = currentPage;
		this.listCount = listCount;
		
		// 전달받은 값과 명시적으로 선언된 값을 이용하여 
		// makePageInfo()수행
		// vo가 생성되면 계산이 완료 된다.
		makePageInfo();
	}

	public PageInfo(int currentPage, int listCount, int limit, int pageSize) {
		super();
		this.currentPage = currentPage;
		this.listCount = listCount;
		this.limit = limit;
		this.pageSize = pageSize;
		
		makePageInfo();
	}
	
	

	public int getCurrentPage() {
		return currentPage;
	}

	public void setCurrentPage(int currentPage) {
		this.currentPage = currentPage;
	}

	public int getListCount() {
		return listCount;
	}

	public void setListCount(int listCount) {
		this.listCount = listCount;
		makePageInfo();
	}

	public int getLimit() {
		return limit;
	}

	public void setLimit(int limit) {
		this.limit = limit;
		makePageInfo();
	}

	public int getPageSize() {
		return pageSize;
	}

	public void setPageSize(int pageSize) {
		this.pageSize = pageSize;
		makePageInfo();
	}

	public int getMaxPage() {
		return maxPage;
	}

	public void setMaxPage(int maxPage) {
		this.maxPage = maxPage;
	}

	public int getStartPage() {
		return startPage;
	}

	public void setStartPage(int startPage) {
		this.startPage = startPage;
	}

	public int getEndPage() {
		return endPage;
	}

	public void setEndPage(int endPage) {
		this.endPage = endPage;
	}

	@Override
	public String toString() {
		return "PageInfo [currentPage=" + currentPage + ", listCount=" + listCount + ", limit=" + limit + ", pageSize="
				+ pageSize + ", maxPage=" + maxPage + ", startPage=" + startPage + ", endPage=" + endPage + "]";
	}
	
	
	// 페이징 처리에 필요한 값을 계산하는 메소드
	// private: 내부적으로만 사용 가능
	private void makePageInfo() {
		// maxPage : 총 페이지 수 == 마지막 페이지
		// 총 게시글 수가 100개, 한 페이지에 보여지는 게시글 수 10개 ==>  총 페이지 수 = 10, 마지막 페이지 = 10
		// 총 게 시글 수 101개, 한 페이지에 보여지는 게시글 수 10개 ==> 총 페이지 수 == (10.1 올림처리)11, 마지막 페이지 = 11
		maxPage = (int)Math.ceil((double)listCount / limit);
		
		
		// startPage: 페이징바 시작 번호
		// 페이징바에 페이지를 10개씩 보여줄 경우  1,11,21,31...
		startPage = (currentPage-1) / pageSize * pageSize + 1;
					// (11-1) / 10 * 10 + 1;
		
		// endPage : 페이징바의 끝 번호
		// 페이징바에 페이지를 10개씩 보여줄 경우 10,20,30,40 ...
		endPage = startPage + pageSize - 1;
		
		// 총 페이지의 수가 end페이지보다 작을 경우
		if(maxPage<=endPage) {
			endPage = maxPage;
		}
		
	}
}
```
<br>

BoardController.java

```java
package com.kh.wsp.board.controller;

import java.io.IOException;
import java.util.List;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.kh.wsp.board.model.service.BoardService;
import com.kh.wsp.board.model.vo.Board;
import com.kh.wsp.board.model.vo.PageInfo;

@WebServlet("/board/*")
public class BoardController extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		
		// 요청이 들어오는 주소  --> /wsp/board/list.do
		String uri = request.getRequestURI();
		// /wsp
		String contextPath = request.getContextPath();
		String command = uri.substring((contextPath + "/board").length());
		//  /wsp/board/list.do 에서  /wsp/board 를 잘라낸다 --> /list.do 만 남음
		
		String path = null;
		RequestDispatcher view = null;
		
		String swalIcon = null;
		String swalTitle = null;
		String swalText = null;
		
		String errorMsg = null;
		
		try {
			BoardService service = new BoardService();
			
			// 현재 페이지를 얻어옴
			String cp = request.getParameter("cp");
			
			// 게시글 목록 조회 Controller **********************************************************
			if(command.equals("/list.do")) {
				errorMsg = "게시판 목록 조회 과정에서 오류 발생";
				
				// 1) 페이징 처리를 위한 값 계산 Service 호출
				PageInfo pInfo = service.getPageInfo(cp);
```
<br>

BoardService.java

```java
/** 페이징 처리를 위한 값 계산 service
* @param cp
* @return PageInfo
* @throws Exception
*/
public PageInfo getPageInfo(String cp) throws Exception {
Connection conn = getConnection();

// cp가 null일 경우
int currentPage = cp == null ? 1 : Integer.parseInt(cp);

// DB에서 전체 게시글 수를 조회하여 반환받기
int listCount = dao.getListCount(conn);

close(conn);

return new PageInfo(currentPage, listCount);
}
```
<br>

BoardDao.java

```java
/** 전체 게시글 수 반환 DAO
* @param conn
* @return listCount
* @throws Exception
*/
public int getListCount(Connection conn) throws Exception {
int listCount =0;
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
<br>

SQL

```sql
<!-- 전체 게시글 수 조회  -->
<entry key="getListCount">
SELECT COUNT(*) FROM V_BOARD
WHERE BOARD_STATUS = 'Y'	
</entry>
```
<br>

<br>


<br><br>

*게시글 목록 조회*


BoardController.java

```java
// 2) 게시글 목록 조회 비즈니스 로직 수행
List<Board> bList = service.selectBoardList(pInfo);
// pInfo에 있는 currentPage, limit를 사용해야지만
// 현재 페이지에 맞는 게시글 목록만  조회할 수 있음.

/*for(Board b : bList) {
    System.out.println(b);
}*/

// 썸네일 추가 부분
path = "/WEB-INF/views/board/boardList.jsp";

request.setAttribute("bList", bList);
request.setAttribute("pInfo", pInfo);

view = request.getRequestDispatcher(path);
view.forward(request, response);
}

```
<br>

Board.java

```java

package com.kh.wsp.board.model.vo;

import java.sql.Timestamp;

public class Board {
	private int boardNo;
	private String boardTitle;
	private String boardContent;
	private String memberId;
	private int readCount;
	private String categoryName;
	private Timestamp boardCreateDate;
	private Timestamp boardModifyDate;
	private String boardStatus;
	
	public Board() {}

	public Board(int boardNo, String boardTitle, String memberId, int readCount, String categoryName,
			Timestamp boardCreateDate) {
		super();
		this.boardNo = boardNo;
		this.boardTitle = boardTitle;
		this.memberId = memberId;
		this.readCount = readCount;
		this.categoryName = categoryName;
		this.boardCreateDate = boardCreateDate;
	}


	public Board(int boardNo, String boardTitle, String boardContent, String memberId, int readCount,
			String categoryName, Timestamp boardCreateDate, Timestamp boardModifyDate, String boardStatus) {
		super();
		this.boardNo = boardNo;
		this.boardTitle = boardTitle;
		this.boardContent = boardContent;
		this.memberId = memberId;
		this.readCount = readCount;
		this.categoryName = categoryName;
		this.boardCreateDate = boardCreateDate;
		this.boardModifyDate = boardModifyDate;
		this.boardStatus = boardStatus;
	}

	public int getBoardNo() {
		return boardNo;
	}

	public void setBoardNo(int boardNo) {
		this.boardNo = boardNo;
	}

	public String getBoardTitle() {
		return boardTitle;
	}

	public void setBoardTitle(String boardTitle) {
		this.boardTitle = boardTitle;
	}

	public String getBoardContent() {
		return boardContent;
	}

	public void setBoardContent(String boardContent) {
		this.boardContent = boardContent;
	}

	public String getMemberId() {
		return memberId;
	}

	public void setMemberId(String memberId) {
		this.memberId = memberId;
	}

	public int getReadCount() {
		return readCount;
	}

	public void setReadCount(int readCount) {
		this.readCount = readCount;
	}

	public String getCategoryName() {
		return categoryName;
	}

	public void setCategoryName(String categoryName) {
		this.categoryName = categoryName;
	}

	public Timestamp getBoardCreateDate() {
		return boardCreateDate;
	}

	public void setBoardCreateDate(Timestamp boardCreateDate) {
		this.boardCreateDate = boardCreateDate;
	}

	public Timestamp getBoardModifyDate() {
		return boardModifyDate;
	}

	public void setBoardModifyDate(Timestamp boardModifyDate) {
		this.boardModifyDate = boardModifyDate;
	}

	public String getBoardStatus() {
		return boardStatus;
	}

	public void setBoardStatus(String boardStatus) {
		this.boardStatus = boardStatus;
	}

	@Override
	public String toString() {
		return "Board [boardNo=" + boardNo + ", boardTitle=" + boardTitle + ", boardContent=" + boardContent
				+ ", memberId=" + memberId + ", readCount=" + readCount + ", categoryName=" + categoryName
				+ ", boardCreateDate=" + boardCreateDate + ", boardModifyDate=" + boardModifyDate + ", boardStatus="
				+ boardStatus + "]";
	}
}
```
<br>

BoardService.java

```java
/** 게시글 목록 조회
* @param pInfo
* @return bList
* @throws Exception
*/
public List<Board> selectBoardList(PageInfo pInfo) throws Exception{
Connection conn = getConnection();

List<Board> bList = dao.selectBoardList(conn, pInfo);

close(conn);
return bList;
}
```
<br>

BoardDao.java

```java
/** 게시글 목록 조회 DAO
* @param conn
* @param pInfo
* @return 
* @throws Exception
*/
public List<Board> selectBoardList(Connection conn, PageInfo pInfo) throws Exception {
List<Board> bList = null;

String query = prop.getProperty("selectBoardList");

try {
    // SQL 구문 조건절에 대입할 변수 생성
    int startRow = (pInfo.getCurrentPage()-1) * pInfo.getLimit() +1;
    int endRow = startRow + pInfo.getLimit()-1;
    
    pstmt = conn.prepareStatement(query);
    pstmt.setInt(1, startRow);
    pstmt.setInt(2, endRow);
    
    rset = pstmt.executeQuery();
    
    bList = new ArrayList<Board>();
    
    while(rset.next()) {
        Board board = new Board(rset.getInt("BOARD_NO"), 
                rset.getString("BOARD_TITLE"), rset.getString("MEMBER_ID"), 
                rset.getInt("READ_COUNT"), 
                rset.getString("CATEGORY_NM"), rset.getTimestamp("BOARD_CREATE_DT"));
        bList.add(board);
    }
    
}finally {
    close(rset);
    close(pstmt);
}

return bList;
}

```
<br>

SQL

```sql
<!-- 지정된 페이지 게시글 목록 조회  -->
<entry key="selectBoardList">
SELECT * FROM
    (SELECT ROWNUM RNUM, V.*
    FROM
        (SELECT * FROM V_BOARD WHERE BOARD_STATUS ='Y' ORDER BY BOARD_NO DESC)V)
WHERE RNUM BETWEEN ? AND ?
</entry>
```


<br><br><br>

목록 조회 후 boardList.jsp로 포워드
<br>

### 게시판 초기 화면

![게시판 초기 화면](https://user-images.githubusercontent.com/73421820/103616948-aea65580-4f70-11eb-8a7f-968a56ef7d2b.png)

<br><br>


### 화면 구성 완료 후 화면
![화면](https://user-images.githubusercontent.com/73421820/103617055-dac1d680-4f70-11eb-9865-c8708f8eaf6f.png)


<br><br>

boardList.jsp

```html
<%-- 게시글 목록 출력 --%>
<tbody>
    <c:choose>
        <c:when test="${empty bList }">
        <!-- bList가 비어있을 때 : 게시글 목록 조회에서 조회되지 않았을 때  -->
                <tr>
                        <td colspan="6">존재하는 게시글이 없습니다.</td>
                </tr> 
        </c:when>
        
        <c:otherwise> <!-- 조회된 게시글 목록이 있을 때  -->
                <c:forEach var="board" items="${bList }">
                <!-- for문으로 반복접근해서 모든 데이터를 출력 해준다. 
                    한 바퀴마다 bList에서 하나씩 꺼내와 board에 담는다 -->
                        <tr>
                                <td>${board.boardNo }</td> <!--글번호 -->
                                <td>${board.categoryName }</td> <!--카테고리명 -->
                                <td class="boardTitle">
                                        ${board.boardTitle }
                                </td> <!--제목 -->
                                <td>${board.memberId }</td> <!--작성자 -->
                                <td>${board.readCount }</td> <!-- 조회수 -->
                                
                                <td>
                                        <%-- 날짜 출력 모양 지정 --%>
                <fmt:formatDate var="createDate" value="${board.boardCreateDate}" pattern="yyyy-MM-dd"/>
                <fmt:formatDate var="today" value="<%= new java.util.Date() %>" pattern="yyyy-MM-dd"/>
                
                <c:choose>
                <%-- 글 작성일이 오늘이 아닐 경우 --%>
                <c:when test="${createDate != today}">
                    ${createDate }
                </c:when>
                
                <%-- 글 작성일이 오늘일 경우 --%>
                <c:otherwise>
                    <fmt:formatDate value="${board.boardCreateDate}" pattern="HH:mm"/>
                                                </c:otherwise>
                                            </c:choose>
                                </td> <!-- 날짜 출력 -->
                        
                        </tr>	 
                </c:forEach>
        </c:otherwise>
    </c:choose>
</tbody>
</table>
</div>
```

<br>
<br>

### 게시판 페이징 처리
<br>

![페이징처리 48](https://user-images.githubusercontent.com/73421820/103616950-afd78280-4f70-11eb-97be-57e538177880.png)

<br><br>

boardList.jsp

```html
<%---------------------- Pagination ----------------------%>
<%-- 페이징 처리 주소를 쉽게 사용할 수 있도록 미리 변수에 저장 --%>
<c:url var="pageUrl" value="/board/list.do"/>

<!-- 화살표에 들어갈 주소를 변수로 생성 -->
<c:set var="firstPage" value="${pageUrl}?cp=1"/>
<c:set var="lastPage" value="${pageUrl}?cp=${pInfo.maxPage}"/>

<%-- EL을 이용한 숫자 연산의 단점 : 연산이 자료형에 영향을 받지 않는다. --%>
<%-- <fmt:parseNumber> : 숫자 형태를 지정하여 변수 선언 
integerOnly="true" : 정수로만 숫자 표현 (소수점 버림)
--%>

    
    <fmt:parseNumber var="c1" value="${(pInfo.currentPage - 1)/10}" integerOnly="true"/>
<fmt:parseNumber var="prev" value="${c1 * 10}" integerOnly="true"/>
<c:set var="prevPage" value="${pageUrl}?cp=${prev}"/> <!-- /board/list/do?cp=10  -->

<fmt:parseNumber var="c2" value="${(pInfo.currentPage + 9)/10}" integerOnly="true"/>
<fmt:parseNumber var="next" value="${c2 * 10 + 1}" integerOnly="true"/>
<c:set var="nextPage" value="${pageUrl}?cp=${next}"/>

            
    <div class="my-5">
<ul class="pagination">

    <%-- 현재 페이지가 10페이지 초과인 경우 --%>
    <c:if test="${pInfo.currentPage>10}">
        <li><!-- 첫 페이지로 이동(<<) -->
            <a class="page-link" href="${firstPage}">&lt;&lt;</a>
        </li>
        
        <li> <!-- 이전 페이지로 이동(<) -->
            <a class="page-link" href="${prevPage}">&lt;</a>
        </li>
    </c:if>

            
            <!-- 페이지 목록 -->
    <c:forEach var="page" begin="${pInfo.startPage}" end="${pInfo.endPage}">
        <c:choose>
            <c:when test="${pInfo.currentPage == page}">
            <li>
                <a class="page-link">${page}</a>
            </li>
            </c:when>
            
            <c:otherwise>
            <li>
                <a class="page-link" href="${pageUrl}?cp=${page}">${page}</a>
            </li>
            </c:otherwise>
        </c:choose>
    </c:forEach>
            
            
            <%-- 현재 페이지가 마지막 페이지 미만인 경우 --%>
    <c:if test="${next < pInfo.maxPage}">
        <li> <!-- 다음 페이지로 이동(>) -->
            <a class="page-link" href="${nextPage}">&gt;</a>
        </li>
        <li><!-- 마지막 페이지로 이동(>>) -->
            <a class="page-link" href="${lastPage}">&gt;&gt;</a>
        </li>
        
    </c:if>

</ul>
</div>
```

<br><br>
<br>


## 전체 BoardController

```java
package com.kh.wsp.board.controller;

import java.io.IOException;
import java.util.List;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.kh.wsp.board.model.service.BoardService;
import com.kh.wsp.board.model.vo.Board;
import com.kh.wsp.board.model.vo.PageInfo;

@WebServlet("/board/*")
public class BoardController extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		
		// 요청이 들어오는 주소  --> /wsp/board/list.do
		String uri = request.getRequestURI();
		// /wsp
		String contextPath = request.getContextPath();
		String command = uri.substring((contextPath + "/board").length());
		//  /wsp/board/list.do 에서  /wsp/board 를 잘라낸다 --> /list.do 만 남음
		
		String path = null;
		RequestDispatcher view = null;
		
		String swalIcon = null;
		String swalTitle = null;
		String swalText = null;
		
		String errorMsg = null;
		
		try {
			BoardService service = new BoardService();
			
			// 현재 페이지를 얻어옴
			String cp = request.getParameter("cp");
			
			
			
    // 게시글 목록 조회 Controller **********************************************************
        if(command.equals("/list.do")) {
            errorMsg = "게시판 목록 조회 과정에서 오류 발생";
            
            // 1) 페이징 처리를 위한 값 계산 Service 호출
            PageInfo pInfo = service.getPageInfo(cp);
            
//				System.out.println(pInfo);
            
            // 2) 게시글 목록 조회 비즈니스 로직 수행
            List<Board> bList = service.selectBoardList(pInfo);
            // pInfo에 있는 currentPage, limit를 사용해야지만
            // 현재 페이지에 맞는 게시글 목록만  조회할 수 있음.
            
            /*for(Board b : bList) {
                System.out.println(b);
            }*/
            
            // 썸네일 추가 부분
            path = "/WEB-INF/views/board/boardList.jsp";
            
            request.setAttribute("bList", bList);
            request.setAttribute("pInfo", pInfo);
            
            view = request.getRequestDispatcher(path);
            view.forward(request, response);
			}
		}catch(Exception e) {
			e.printStackTrace();
			path = "/WEB-INF/views/common/errorPage.jsp";
			
			// 에러 메세지를 request객체에 담는다
			request.setAttribute("errorMsg", errorMsg);
			view = request.getRequestDispatcher(path);
			view.forward(request, response);
		}
	}
	protected void doPost(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		
		doGet(request, response);
	}
}
```

<br>

## 전체 boardList

```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>게시판</title>
<style>
.pagination {
	justify-content: center;
}

#searchForm {
	position: relative;
}

#searchForm>* {
	top: 0;
}

.boardTitle>img {
	width: 50px;
	height: 50px;
}

#list-table th {
	text-align: center;
}

#list-table td:not(:nth-of-type(3)) {
	text-align: center;
}

.list-wrapper{
	height: 540px;
}

</style>

</head>
<body>
<jsp:include page="../common/header.jsp"></jsp:include>
<div class="container my-5">

<h1>게시판</h1>

<div class="list-wrapper">
    <table class="table table-hover table-striped my-5" id="list-table">
        <thead>
            <tr>
                <th>글번호</th>
                <th>카테고리</th>
                <th>제목</th>
                <th>작성자</th>
                <th>조회수</th>
                <th>작성일</th>
            </tr>
        </thead>
        
        <%-- 게시글 목록 출력 --%>
<tbody>
<c:choose>
    <c:when test="${empty bList }">
    <!-- bList가 비어있을 때 : 게시글 목록 조회에서 조회되지 않았을 때  -->
            <tr>
                    <td colspan="6">존재하는 게시글이 없습니다.</td>
            </tr> 
    </c:when>
    
    <c:otherwise> <!-- 조회된 게시글 목록이 있을 때  -->
            <c:forEach var="board" items="${bList }">
            <!-- for문으로 반복접근해서 모든 데이터를 출력 해준다. 
                한 바퀴마다 bList에서 하나씩 꺼내와 board에 담는다 -->
                    <tr>
                            <td>${board.boardNo }</td> <!--글번호 -->
                            <td>${board.categoryName }</td> <!--카테고리명 -->
                            <td class="boardTitle">
                                    ${board.boardTitle }
                            </td> <!--제목 -->
                            <td>${board.memberId }</td> <!--작성자 -->
                            <td>${board.readCount }</td> <!-- 조회수 -->
                            
                            <td>
                                    <%-- 날짜 출력 모양 지정 --%>
            <fmt:formatDate var="createDate" value="${board.boardCreateDate}" pattern="yyyy-MM-dd"/>
            <fmt:formatDate var="today" value="<%= new java.util.Date() %>" pattern="yyyy-MM-dd"/>
            
            <c:choose>
            <%-- 글 작성일이 오늘이 아닐 경우 --%>
            <c:when test="${createDate != today}">
                ${createDate }
            </c:when>
            
            <%-- 글 작성일이 오늘일 경우 --%>
            <c:otherwise>
                <fmt:formatDate value="${board.boardCreateDate}" pattern="HH:mm"/>
                                            </c:otherwise>
                                        </c:choose>
                            </td> <!-- 날짜 출력 -->
                    
                    </tr>	 
            </c:forEach>
    </c:otherwise>
</c:choose>
</tbody>
</table>
</div>


<%-- 로그인이 되어있는 경우 --%>
<button type="button" class="btn btn-primary float-right" id="insertBtn" 
                onclick="location.href = 'insertForm.do'">글쓰기</button>


    <%---------------------- Pagination ----------------------%>
<%-- 페이징 처리 주소를 쉽게 사용할 수 있도록 미리 변수에 저장 --%>
<c:url var="pageUrl" value="/board/list.do"/>

<!-- 화살표에 들어갈 주소를 변수로 생성 -->
<c:set var="firstPage" value="${pageUrl}?cp=1"/>
<c:set var="lastPage" value="${pageUrl}?cp=${pInfo.maxPage}"/>

<%-- EL을 이용한 숫자 연산의 단점 : 연산이 자료형에 영향을 받지 않는다. --%>
<%-- <fmt:parseNumber> : 숫자 형태를 지정하여 변수 선언 
integerOnly="true" : 정수로만 숫자 표현 (소수점 버림)
--%>

    
    <fmt:parseNumber var="c1" value="${(pInfo.currentPage - 1)/10}" integerOnly="true"/>
<fmt:parseNumber var="prev" value="${c1 * 10}" integerOnly="true"/>
<c:set var="prevPage" value="${pageUrl}?cp=${prev}"/> <!-- /board/list/do?cp=10  -->

<fmt:parseNumber var="c2" value="${(pInfo.currentPage + 9)/10}" integerOnly="true"/>
<fmt:parseNumber var="next" value="${c2 * 10 + 1}" integerOnly="true"/>
<c:set var="nextPage" value="${pageUrl}?cp=${next}"/>

            
    <div class="my-5">
<ul class="pagination">

    <%-- 현재 페이지가 10페이지 초과인 경우 --%>
    <c:if test="${pInfo.currentPage>10}">
        <li><!-- 첫 페이지로 이동(<<) -->
            <a class="page-link" href="${firstPage}">&lt;&lt;</a>
        </li>
        
        <li> <!-- 이전 페이지로 이동(<) -->
            <a class="page-link" href="${prevPage}">&lt;</a>
        </li>
    </c:if>

            
            <!-- 페이지 목록 -->
    <c:forEach var="page" begin="${pInfo.startPage}" end="${pInfo.endPage}">
        <c:choose>
            <c:when test="${pInfo.currentPage == page}">
            <li>
                <a class="page-link">${page}</a>
            </li>
            </c:when>
            
            <c:otherwise>
            <li>
                <a class="page-link" href="${pageUrl}?cp=${page}">${page}</a>
            </li>
            </c:otherwise>
        </c:choose>
    </c:forEach>
            
            
            <%-- 현재 페이지가 마지막 페이지 미만인 경우 --%>
    <c:if test="${next < pInfo.maxPage}">
        <li> <!-- 다음 페이지로 이동(>) -->
            <a class="page-link" href="${nextPage}">&gt;</a>
        </li>
        <li><!-- 마지막 페이지로 이동(>>) -->
            <a class="page-link" href="${lastPage}">&gt;&gt;</a>
        </li>
        
    </c:if>

</ul>
</div>



    <!-- 검색창 -->
<div class="my-5">
    <form action="${contextPath }/search.do" method="GET" class="text-center" id="searchForm">
        <select name="sk" class="form-control" style="width: 100px; display: inline-block;">
            <option value="title">글제목</option>
            <option value="content">내용</option>
            <option value="titcont">제목+내용</option>
            <option value="writer">작성자</option>
        </select>
        <input type="text" name="sv" class="form-control" style="width: 25%; display: inline-block;">
        <button class="form-control btn btn-primary" style="width: 100px; display: inline-block;">검색</button>
    </form>


</div>
</div>
<jsp:include page="../common/footer.jsp"></jsp:include>


<script>
// 게시글 상세보기 기능 (jquery를 통해 작업)
</script>
</body>
</html>
```