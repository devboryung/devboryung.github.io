---
title: "2020년 01월 04일"
excerpt: "WSP-6"
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

## 공지사항

### 공지사항 상세 조회
행 어디를 클릭해도 해당 글의 상세페이지가 조회된다.<br>
한 행은 tr로 이루어져있고, td가 형제관계로 존재하고 있다.<br>
td중 한개를 선택했을 때, 맨 첫번째 형제인 글 번호(PK)를 선택하게 하기.<br>

<br>

*목록에 마우스를 올릴 경우 손가락 모양으로 커서 변경*

![커서모양 0](https://user-images.githubusercontent.com/73421820/103516409-d71c4a00-4eb3-11eb-86d1-194827753cbb.png)
<br><br><br>

```css
/* 목록에 마우스를 올릴 경우 손가락 모양으로 변경  */
#list-table td:hover{
	cursor : pointer;
}
```
<br>
<br><br>

*공지사항 상세조회 초기 화면*

![공지사항 상세조회 초기 화면](https://user-images.githubusercontent.com/73421820/103516356-bbb13f00-4eb3-11eb-9789-74065a68d129.png)

<br><br>

noticeList.jsp
```html
<h1>공지사항</h1>

<div class="list-wrapper">
    <table class="table table-hover table-striped my-5" id="list-table">
        <thead>
            <tr>
                <th>글번호</th>
                <th>제목</th>
                <th>작성자</th>
                <th>조회수</th>
                <th>작성일</th>
            </tr>
        </thead>

        <tbody>
            <!-- 공지사항 목록 -->
            <%-- 공지사항이 존재할 때와 존재하지 않을 때에 맞는 출력 형식을 지정해야 함. --%>
            
            <c:choose>
            <%-- 공지사항이 없을 때 --%>
            <c:when test="${empty list}"> 
                <tr>
                <td colspan="5" align="center">존재하는 공지사항이 없습니다.</td>
                </tr>
            </c:when>
            
            <%-- 공지사항이 존재할 때 --%>
            <c:otherwise>
                <c:forEach var="notice" items="${list}">
                <tr>
                    <td>${ notice.noticeNo }</td>
                    <td>${notice.noticeTitle}</td>
                    <td>${notice.memberId}</td>
                    <td>${notice.readCount}</td>
                    <td>${notice.noticeCreateDate}</td>
                </tr>
                </c:forEach>
            </c:otherwise>
        </c:choose>
            
        </tbody>
    </table>
</div>
```
<br>

```html
<script>
// 공지사항 상세보기 기능 (jquery를 통해 작업)
// id가 list-table인 요소의 후손 중에 td 하나가 클릭이 되면  해당 td의 첫번째 형제를 선택해라.
$("#list-table td").on("click", function(){
    var noticeNo = $(this).parent().children().eq(0).text();
    //	클릭 이벤트가 발생한 요소의 부모의 자식들 중의 제일 첫번째 자식.
    // console.log(noticeNo);
    
    // 얻어온 공지사항 글 번호를 쿼리스트링으로 작성하여 상세조회 요청
    location.href = "${contextPath}/notice/view.do?no=" + noticeNo;
    // ${contextPath}/notice/view.do : 상세조회를 위한 주소
    
});
</script>
```

<br>

*로그인된 회원이 해당 글의 작성자일 경우에만 수정/삭제가 보인다.*

![noticeView설정 후 상세조회](https://user-images.githubusercontent.com/73421820/103516420-dc799480-4eb3-11eb-85d6-79df96990e77.png)

<br><br>

noticeView.jsp

```html
<%-- 로그인된 회원이 해당 글의 작성자인 경우 --%>
<!-- float-right 속성 : 오른쪽으로 정렬됨, 화면상에선 목록,수정,삭제 순서대로 보임.  -->
<c:if test="${!empty loginMember && (loginMember.memberId == notice.memberId) }">
    <button class="btn btn-primary float-right" id="deleteBtn">삭제</button>
    <a href="updateForm.do?no=${param.no}" class="btn btn-primary float-right ml-1 mr-1">수정</a>
    <!-- request 파라미터 중 no가 존재하고 있음.-->
</c:if>
```

<br>

NoticeController.java

```java
// 공지사항 상세 조회 Controller **********
else if(command.equals("/view.do")) {
    // command = 요청주소 가장 끝만 잘라내어 저장하는 변수
    // 상세조회 요청이 왔다면
    errorMsg = "공지사항 상세 조회 과정에서 오류 발생";
    
    // 파라미터 얻어오기
    // 쿼리스트링으로 전달된 공지사항 번호를 int형으로 파싱하여 저장
    int noticeNo = Integer.parseInt(request.getParameter("no"));
    
    // 상세조회 비즈니스 로직 수행 결과 반환
    Notice notice = service.selectNotice(noticeNo);
    
    // 조회 결과에 따른 view 연결 처리
    if(notice != null) { // 조회 성공
        // 경로 지정
        path = "/WEB-INF/views/notice/noticeView.jsp";
        
        // 요청 위임
        request.setAttribute("notice", notice);
        view = request.getRequestDispatcher(path);
        view.forward(request, response);
    }else { // 조회 실패
        HttpSession session = request.getSession();
        session.setAttribute("swalIcon", "error");
        session.setAttribute("swalTitle", "공지사항 조회 실패");
        // 이전 주소로 이동
        response.sendRedirect(request.getHeader("referer"));
    }
}
```
<br>

NoticeService.java

```java
/** 공지사항 상세 조회 Service
    * @param noticeNo
    * @return notice
    * @throws Exception
    */
public Notice selectNotice(int noticeNo) throws Exception {
    
    // 1) 커넥션 얻어오기
    Connection conn = getConnection();
    
    // 2) dao 수행 후 결과 반환받기
    Notice notice = dao.selectNotice(conn, noticeNo);
    
    // 3) 공지사항 조회 성공 시 조회수 증가
    if(notice !=null ) {
        int result = dao.increaseReadCount(conn,noticeNo);
        
        // 트랜잭션 처리
        if(result>0) {
            commit(conn);
        // 이미 조회된 notice에서 readCount 값을 1 증가
            notice.setReadCount(notice.getReadCount()+1);
        }
        else rollback(conn);
    }
    
    // 4) 커넥션 반환
    close(conn);
    
    // 5) 결과 반환
    return notice;
}
```
<br>

NoticeDAO.java

```java
/** 공지사항 상세조회 DAO
 * @param conn
 * @param noticeNo
 * @return notice
 * @throws Exception
 */
public Notice selectNotice(Connection conn, int noticeNo) throws Exception {
// 1) 결과를 저장할 변수 선언
Notice notice = null;

// 2) SQL 구문 얻어오기
String query = prop.getProperty("selectNotice");

// 3) SQL 수행 후 결과를 notice에 저장
try {
    pstmt = conn.prepareStatement(query);
    pstmt.setInt(1, noticeNo);
    
    rset=pstmt.executeQuery();
    if(rset.next()) {
        notice = new Notice();
        notice.setNoticeTitle(rset.getString("NOTICE_TITLE"));		
        notice.setNoticeContent(rset.getString("NOTICE_CONTENT"));
        notice.setMemberId(rset.getString("MEMBER_ID"));
        notice.setReadCount(rset.getInt("READ_COUNT"));
        notice.setNoticeCreateDate(rset.getDate("NOTICE_CREATE_DT"));
    }
}finally {
    // 4) 사용한 JDBC 객체 반환
    close(rset);
    close(pstmt);
}
// 5) 결과 반환
return notice;
}
```
<br>

SQL

```sql
<!-- 공지사항 상세 조회  -->
<entry key="selectNotice">
SELECT NOTICE_TITLE, NOTICE_CONTENT, MEMBER_ID, READ_COUNT,NOTICE_CREATE_DT
FROM NOTICE
JOIN MEMBER ON(NOTICE_WRITER = MEMBER_NO)
WHERE NOTICE_NO=?
AND NOTICE_FL='Y'
</entry>
```

<br>

### 조회수 증가
<br>

*조회수가 클릭 할 때마다 +1 씩 증가됨*
![조회수 증가 0](https://user-images.githubusercontent.com/73421820/103516929-aab4fd80-4eb4-11eb-954c-94eb80ee3c1d.png)

<br><br><br>

NoticeDAO.java

```java
/** 조회수 증가 DAO
 * @param conn
 * @param noticeNo
 * @return result
 * @throws Exception
 */
public int increaseReadCount(Connection conn, int noticeNo) throws Exception {
	int result = 0;
	
	String query = prop.getProperty("increaseReadCount");
	
	try {
		pstmt = conn.prepareStatement(query);
		pstmt.setInt(1, noticeNo);
		result = pstmt.executeUpdate();
	}finally {
		close(pstmt);
	}
	return result;
}
```
<br>

SQL

```sql
<!-- 조회수 증가  -->
<entry key="increaseReadCount">
UPDATE NOTICE SET
READ_COUNT = READ_COUNT+1
WHERE NOTICE_NO=?
</entry>
```

<br><br>

### 글쓰기 버튼
관리자로 로그인했을 때만 로그인 버튼이 보이게 설정하기
<br>
<br>
noticeList.jsp

```html
<%-- 로그인된 계정이 관리자 등급인 경우 --%>
<c:if test="${!empty loginMember && loginMember.memberGrade=='A' }">
    <button type="button" class="btn btn-primary float-right" id="insertBtn" 
    onclick="location.href = 'insertForm.do';">글쓰기</button>
</c:if>		
```
<br>
<br>

### 공지사항 등록

<br>

*공지사항 등록 초기 화면*
![공지사항 글 쓰기](https://user-images.githubusercontent.com/73421820/103521979-04212a80-4ebd-11eb-8111-72ebcec936f5.png)

<br>

noticeInsert.jsp

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>공지사항</title>
<style>
	#notice-area{ height: 700px;}
</style>
</head>
<body>
  <jsp:include page="../common/header.jsp"></jsp:include>
	<div class="container my-5">


    <h3>공지사항 등록</h3>
        <hr>
        <div class="bg-white rounded shadow-sm container py-3" id="notice-area">
        <form method="POST" action="insert.do" role="form" onsubmit="return noticeValidate();">
            <div class="form-inline mb-2">
            <div class="input-group">
                <label class="input-group-addon mr-3">제목</label>
                <input type="text" class="form-control" id="noticeTitle" name="noticeTitle" size="70">
            </div>
            </div>

            <div class="form-inline mb-2">
            <div class="input-group">
                <label class="input-group-addon mr-3">작성자</label>
                <h5 class="my-0">${loginMember.memberId }</h5>
            </div>
            </div>


            <div class="form-inline mb-2">
            <div class="input-group">
                <label class="input-group-addon mr-3">작성일</label>
                <h5 class="my-0" id="today"></h5>
            </div>
            </div>

            <hr>

            <div class="form-group">
            <div><label for="content">내용</label> </div>
            <!-- textarea == input태근  -->
            <textarea class="form-control" id="noticeContent" name="noticeContent" rows="15" style="resize: none;"></textarea>
            </div>


        <hr class="mb-4">

        <div class="text-center">
            <button type="submit" class="btn btn-primary">등록</button>
            <a href="list" class="btn btn-primary">목록으로</a>
        </div>
        
        </form>
        </div>

</div>
<jsp:include page="../common/footer.jsp"></jsp:include>
```
<br>

*작성일에 오늘 날짜 출력*

```html
<script>
(function printToday(){
    // 오늘 날짜 출력 
    var now = new Date(); // 현재 시간
    var year = now.getFullYear(); // 201  Date라는 내장객체에 존재하는 함수
    var month = now.getMonth() + 1; // 0~11중 하나가 반환
    var date = now.getDate(); // 4

    var str = year + "-" + (month<10? "0"+month : month) + "-" + (date<10 ? "0" +date : date);
    
    $("#today").text(str);
})();		
</script>
```

<br>

*유효성 검사(제목/내용에 내용 필수적으로 작성해야 됨)*

```html
    <script>
// 유효성 검사
function noticeValidate(){
    // 제목에 아무것도 작성하지 않음
    if( $("#noticeTitle").val().trim().length == 0){
        alert("제목을 입력해 주세요.");
        $("#title").focus();
        return false;
    }
    
    // 내용에 아무것도 작성하지 않음
    if( $("#noticeContent").val().trim().length == 0){
        alert("내용을 입력해 주세요.");
        $("#content").focus();
        return false;
    }
}
	</script>
</body>
</html>
```
<br>

NoticeController.java

```java
// 공지사항 작성 화면 전환 Controller **********
else if(command.contentEquals("/insertForm.do")) {
    path = "/WEB-INF/views/notice/noticeInsert.jsp";
    view = request.getRequestDispatcher(path);
    view.forward(request, response);
}
```

<br><br>

*공지사항 등록 시 *

![공지사항등록1](https://user-images.githubusercontent.com/73421820/103522752-2798a500-4ebe-11eb-8ed6-6582150f88fd.png)<br>
![공지사항등록2](https://user-images.githubusercontent.com/73421820/103522753-28313b80-4ebe-11eb-931b-3c324b283fbd.png)
<br>

```java
// 공지사항 등록 Controller **********
else if(command.contentEquals("/insert.do")) {
    errorMsg = "공지사항 등록 과정에서 에러 발생.";
    
    // 파라미터로 전달된 값 저장
    String noticeTitle = request.getParameter("noticeTitle");
    String noticeContent = request.getParameter("noticeContent");
    
    // 세션에서 회원 번호 얻어오기
    HttpSession session = request.getSession();
    
    int noticeWriter = ((Member)session.getAttribute("loginMember")).getMemberNo();
    
    // Map을 이용하여 데이터 전달(VO 대신 사용하는 경우가 많음)
    Map<String, Object> map = new HashMap<String, Object>();
    map.put("noticeTitle", noticeTitle);
    map.put("noticeContent", noticeContent);
    map.put("noticeWriter", noticeWriter);
    
    // 공지사항 등록 비즈니스 로직 수행 후 결과 반환
    int result = service.insertNotice(map);
    
    if(result>0) { // 삽입 성공 시
        // result에는 삽입된 공지사항 번호가 저장되어 있음.
        path = "view.do?no="+result; // 쿼리스트링을 이용하여 상세조회 요청
        swalIcon = "success";
        swalTitle = "공지사항이 등록되었습니다.";
    }else {
        path = "list.do"; // 목록으로 돌아감
        swalIcon = "error";
        swalTitle = "공지사항 등록 실패";
    }
    
    session.setAttribute("swalIcon",swalIcon);
    session.setAttribute("swalTitle", swalTitle);
    
    response.sendRedirect(path);
}
```
<br>

NoticeService.java

```java
/** 공지사항 등록 Service
* @param map
* @return result
* @throws Exception
*/
public int insertNotice(Map<String, Object> map) throws Exception {
Connection conn = getConnection();

// 1) 작성될 글 번호를 먼저 얻어오는 DAO 메소드 수행
// 이유 1) 글 작성 성공 후 해당 글 상세 조회 페이지로 redirect 하기 위해 필요.
// 이유 2) 글 작성 + 파일 첨부 시 발생하는 시퀀스 번호 밀림 현상을 제거하기 위해
int noticeNo = dao.selectNextNo(conn);

int result = 0;


if(noticeNo >0) { // 다음 글 번호를 얻어오는데 성공한 경우 
    // 2. 실제 공지글 등록 DAO 메소드 수행
    
    // map에 얻어온 다음 글 번호를 세팅
    map.put("noticeNo", noticeNo);
    
    result = dao.insertNotice(conn,map);

    // 트랜잭션 처리
    if(result>0) {
        commit(conn);
        // 삽입 성공 시 result에 작성한 글 번호를 대입
        result = noticeNo; // 글번호가 최종적으로 반환됨
    }
    else rollback(conn);
}
close(conn);
return result;
}
```
<br>
<br>

NoticeDAO.java

*공지사항 등록 시 사용될 번호 반환*
```java
/** 공지사항 등록 시 사용될 번호 반환 DAO
 * @param conn
 * @return noticeNo
 * @throws Exception
 */
public int selectNextNo(Connection conn) throws Exception {
	int noticeNo = 0;
	String query = prop.getProperty("selectNextNo");
	
	try {
		stmt = conn.createStatement();
		
		rset = stmt.executeQuery(query);
		
		if(rset.next()) {
			noticeNo = rset.getInt(1);
		}
		
	}finally {
		close(rset);
		close(stmt);
	}
	
	return noticeNo;
}
```
<br>

SQL
```sql
<!-- 공지사항 등록 시 사용될 번호 반환 : 다음 시원스 조회  -->
<entry key="selectNextNo">
SELECT SEQ_NNO.NEXTVAL FROM DUAL
</entry>
```


<br>

*공지사항 등록*
```java
/**	공지사항 등록 DAO
 * @param conn
 * @param map
 * @return result
 * @throws Exception
 */
public int insertNotice(Connection conn, Map<String, Object> map) throws Exception{
	int result =0;
	
	String query = prop.getProperty("insertNotice");
	
	try {
		pstmt = conn.prepareStatement(query);
		pstmt.setInt(1, (int)map.get("noticeNo"));
		pstmt.setString(2, (String)map.get("noticeTitle"));
		pstmt.setString(3, (String)map.get("noticeContent"));
		pstmt.setInt(4, (int)map.get("noticeWriter"));
		
		result = pstmt.executeUpdate();
	}finally {
		close(pstmt);
	}
	return result;
}
```
<br>

SQL

```sql
<!-- 공지사항 등록  -->
<entry key="insertNotice">
INSERT INTO NOTICE(NOTICE_NO, NOTICE_TITLE, NOTICE_CONTENT, NOTICE_WRITER)
VALUES (?,?,?,?)
</entry>
```

<br>

### 크로스 사이트 스크립팅
웹 애플리케이션에서 많이 나타나는 보안 취약점 중 하나로, <br>
웹 사이트 관리자가 아닌 사용자가 웹 페이지에 악성 스크립트를 삽입할 수 있는 취약점이 있다.<br><br>

*크로스 사이트 스크립팅 방지 전 (태그 적용 됨)*
![태그를 넣어서 글 작성](https://user-images.githubusercontent.com/73421820/103523351-0edcbf00-4ebf-11eb-9dd5-fda559ec62e4.png)
![태그를 넣어서 글 작성2](https://user-images.githubusercontent.com/73421820/103523356-100dec00-4ebf-11eb-9dec-34a1b41e24c5.png)

<br><br>

*크로스 사이트 스크립팅 방지 후 (태그 사용 불가)*
![방지1](https://user-images.githubusercontent.com/73421820/103523373-156b3680-4ebf-11eb-97b6-7b0a03d53405.png)<br>
![방지2](https://user-images.githubusercontent.com/73421820/103523376-169c6380-4ebf-11eb-9da8-1c9de7b6bce5.png)
<br>


NoticeService.java

```java
// 크로스 사이트 스크립팅 방지 메소드
private String replaceParameter(String param) {
    String result = param;
    
    if(result != null) {
        result = result.replaceAll("&", "&amp;"); 
        result = result.replaceAll("<", "&lt;"); 
        result = result.replaceAll(">", "&gt;"); 
        result = result.replaceAll("\"", "&quot;"); 
    }
    return result;
}
```
<br>

공지사항 등록 Service에 스크립팅 방지 처리를 해준다.

```java
// 크로스 사이트 스크립팅 방지 처리
map.put("noticeTitle", replaceParameter((String)map.get("noticeTitle")));
                        // <script> -> &lt;script&gt; 
map.put("noticeContent", replaceParameter((String)map.get("noticeContent")));

// 개행문자 변경 처리
// textarea의 개행문자는 \r\n 이지만
// 상세조회에서 사용되는 div태그의 개행문자는 <br>이기 때문에 변경이 필요함

map.put("noticeContent", ((String)map.get("noticeContent")).replaceAll("\r\n", "<br>"));
```

<br>


### 공지사항 수정
<br>

*공지사항 수정 전*
![수정 전](https://user-images.githubusercontent.com/73421820/103523759-b6f28800-4ebf-11eb-85bc-1e1c1155aa98.png)
<br><br>

*공지사항 수정 후* 
![수정 후](https://user-images.githubusercontent.com/73421820/103523763-b78b1e80-4ebf-11eb-8fb1-af95fd09fc5c.png)
<br><br>

noticeView.jsp

```html
<a href="updateForm.do?no=${param.no}" class="btn btn-primary float-right ml-1 mr-1">수정</a>
<!-- request 파라미터 중 no가 존재하고 있음.-->
```
<br>

NoticeController.java

```java
// 공지사항 수정 화면  출력용 Controller **********
else if(command.equals("/updateForm.do")) {
    errorMsg = "공지사항 수정 화면 전환 과정에서 오류 발생.";
    
    int noticeNo = Integer.parseInt(request.getParameter("no"));
    
    Notice notice = service.updateView(noticeNo); 
    
    if(notice !=null) {
        path = "/WEB-INF/views/notice/noticeUpdate.jsp";
        request.setAttribute("notice",notice);
        view = request.getRequestDispatcher(path);
        view.forward(request, response);
    }else {
        request.getSession().setAttribute("swalIcon", "error");
        request.getSession().setAttribute("swalText", "공지사항 수정을 위한 조회 실패");
        
        response.sendRedirect("view.do?no=" + noticeNo);
    }
}
```
<br>

NoticeService.java

```java
/** 공지사항 수정 화면 Service
* @param noticeNo
* @return notice
* @throws Exception
*/
public Notice updateView(int noticeNo) throws Exception {
Connection conn = getConnection();

// 공지사항 상세조회 DAO 호출
Notice notice = dao.selectNotice(conn, noticeNo);

// textarea에 출력하기 위해 개행문자 변경
if(notice !=null) {
    notice.setNoticeContent(notice.getNoticeContent().replaceAll("<br>", "\r\n"));
}
close(conn);
return notice;
}
```
<br>



NoticeController.java

```java
// 공지사항 수정 Controller
else if(command.contentEquals("/update.do")) {
    
    errorMsg = "공지사항 수정에서 오류 발생";
    
    // 파라미터 얻어오기
    int noticeNo = Integer.parseInt(request.getParameter("no"));
    String noticeTitle = request.getParameter("noticeTitle");
    String noticeContent = request.getParameter("noticeContent");
    
    // 비즈니스 로직 수행
    
    Map<String, Object> map = new HashMap<String, Object>();
    map.put("noticeNo", noticeNo);
    map.put("noticeTitle", noticeTitle);
    map.put("noticeContent", noticeContent);
    
    int result = service.updateNotice(map);
    
    // 로직 수행 성공 시  "공지사항이 수정되었습니다." swal 출력
    if (result>0) {
        path = "view.do?no="+noticeNo; // 쿼리스트링을 이용하여 상세조회 요청
        swalIcon = "success";
        swalTitle = "공지사항 수정 성공 ";
        
    }
    // 로직 수행 실패 시 " 공지사항 수정 실패" swal 출력
    else {
        path = "list.do"; // 목록으로 돌아감
        swalIcon = "error";
        swalTitle = "공지사항 등록 실패";
    }
    
    request.getSession().setAttribute("swalIcon", swalIcon);
    request.getSession().setAttribute("swalTitle", swalTitle);

    // 수정된 공지사항 상세조회로 redirect
    response.sendRedirect(path);
    
}
```
<br>

NoticeService.java

```java
/** 공지사항 수정 Service
* @param map
* @return result
* @throws Exception
*/
public int updateNotice(Map<String, Object> map) throws Exception {
Connection conn = getConnection();

// 크로스 사이트 스크립팅 방지
map.put("noticeTitle", replaceParameter((String)map.get("noticeTitle")));
map.put("noticeContent", replaceParameter((String)map.get("noticeContent")));
map.put("noticeContent", ((String)map.get("noticeContent")).replaceAll("\r\n", "<br>"));


int result = dao.updateNotice(conn,map);

if(result>0) commit(conn);
else    rollback(conn);

close(conn);

return result;
}
```

<br>

NoticeDAO.java

```java
/** 공지사항 수정 DAO
 * @param conn
 * @param map
 * @return result
 * @throws Exception
 */
public int updateNotice(Connection conn, Map<String, Object> map) throws Exception {
	int result =0;
	String query = prop.getProperty("updateNotice");
	try {
		pstmt = conn.prepareStatement(query);
		pstmt.setString(1, (String)map.get("noticeTitle"));
		pstmt.setString(2, (String)map.get("noticeContent"));
		pstmt.setInt(3, (int)map.get("noticeNo"));
		
		result = pstmt.executeUpdate();
		
	}finally {
		close(pstmt);
	}
	return result;
}
```
<br>

SQL

```sql
<!-- 공지사항 수정  -->
<entry key="updateNotice">
UPDATE NOTICE SET
NOTICE_TITLE =?,
NOTICE_CONTENT = ?
WHERE NOTICE_NO = ?
</entry>
```

<br>
<br>


### 공지사항 삭제
<br>

*삭제 버튼을 클릭했을 때 이벤트 발생(confirm)*
![comfirm 창](https://user-images.githubusercontent.com/73421820/103524129-5d3e8d80-4ec0-11eb-82ec-a64505db041a.png)
<br><br>

noticeView.jsp

```html
<button class="btn btn-primary float-right" id="deleteBtn">삭제</button>

<!-- 삭제버튼 클릭 시  -->
<script>
    $("#deleteBtn").on("click",function(){
        if (confirm("정말 삭제 하시겠습니까?")){
            location.href ="delete.do?no=${param.no}"
        }
    });
</script>
```
<br>

*삭제 완료 후*
![공지사항 삭제](https://user-images.githubusercontent.com/73421820/103524134-5dd72400-4ec0-11eb-9492-9380d6d988fa.png)
<br><br><br>

NoticeController.java

```java
// 공지사항 삭체 Controller **********
else if (command.contentEquals("/delete.do")){
    errorMsg ="공지사항 삭제 과정에서 오류 발생";
    
    int noticeNo = Integer.parseInt(request.getParameter("no"));
    
    // 비즈니스 로직 수행
    int result = service.updateNoticeFl(noticeNo);
    
    if(result>0) {
        swalIcon = "success";
        swalTitle = "공지사항이 삭제되었습니다.";
        path = "list.do";
    }else {
        swalIcon = "error";
        swalTitle = "공지사항 삭제 실패";
        path = request.getHeader("referer");
    }
    
    request.getSession().setAttribute("swalIcon", swalIcon);
    request.getSession().setAttribute("swalTitle", swalTitle);
    
    response.sendRedirect(path);
}
```
<br>

NoticeService.java

```java
/** 공지사항 삭제 service
    * @param noticeNo
    * @return result
    * @throws Exception
    */
public int updateNoticeFl(int noticeNo) throws Exception {
    Connection conn = getConnection();
            
    int result = dao.updateNoticeFl(conn,noticeNo);
    
    if(result>0) commit(conn);
    else   rollback(conn);
    
    close(conn);
    
    return result;
}
```
<br>

NoticeDAO.java

```java
/** 공지사항 삭제 DAO
 * @param conn
 * @param noticeNo
 * @return result
 * @throws Exception
 */
public int updateNoticeFl(Connection conn, int noticeNo) throws Exception {
	int result =0;
	
	String query = prop.getProperty("updateNoticeFl");
	
	try {
		pstmt = conn.prepareStatement(query);
		pstmt.setInt(1, noticeNo);
		
		result = pstmt.executeUpdate();
	}finally {
		close(pstmt);
	}
	return result;
}
```
<br>

SQL
```sql
<!-- 공지사항 삭제 -->
<entry key="updateNoticeFl">
UPDATE NOTICE SET
NOTICE_FL= 'N'
WHERE NOTICE_NO=?
</entry>
```
<br><br><br>




## 전체 noticeList.jsp
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!-- JSTL core Tag 사용  -->
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>공지사항</title>
<style>
.pagination {
	justify-content: center;
}


/* 검색창 스타일 */
#searchForm > * {
	top: 0;
	vertical-align: top;
}

select[name='searchKey']{
	width: 100px; 
	display: inline-block;
}

input[name='searchValue']{
	width: 25%; 
	display: inline-block;
}

#searchBtn{
	width: 100px; 
	display: inline-block;
}

.list-wrapper{
	height: 540px;
}

/* 목록에 마우스를 올릴 경우 손가락 모양으로 변경  */
#list-table td:hover{
	cursor : pointer;
}


</style>

</head>
<body>
	<jsp:include page="../common/header.jsp"></jsp:include>
	<div class="container my-5 ">
	
		<h1>공지사항</h1>

		<div class="list-wrapper">
			<table class="table table-hover table-striped my-5" id="list-table">
				<thead>
					<tr>
						<th>글번호</th>
						<th>제목</th>
						<th>작성자</th>
						<th>조회수</th>
						<th>작성일</th>
					</tr>
				</thead>

				<tbody>
                <!-- 공지사항 목록 -->
                <%-- 공지사항이 존재할 때와 존재하지 않을 때에 맞는 출력 형식을 지정해야 함. --%>
                
                <c:choose>
                <%-- 공지사항이 없을 때 --%>
                <c:when test="${empty list}"> 
                    <tr>
                    <td colspan="5" align="center">존재하는 공지사항이 없습니다.</td>
                    </tr>
                </c:when>
                
                <%-- 공지사항이 존재할 때 --%>
                <c:otherwise>
                    <c:forEach var="notice" items="${list}">
                    <tr>
                        <td>${ notice.noticeNo }</td>
                        <td>${notice.noticeTitle}</td>
                        <td>${notice.memberId}</td>
                        <td>${notice.readCount}</td>
                        <td>${notice.noticeCreateDate}</td>
                    </tr>
                    </c:forEach>
                </c:otherwise>
            </c:choose>
					
				</tbody>
			</table>
		</div>


		<%-- 로그인된 계정이 관리자 등급인 경우 --%>
	<c:if test="${!empty loginMember && loginMember.memberGrade=='A' }">
		<button type="button" class="btn btn-primary float-right" id="insertBtn" 
		               onclick="location.href = 'insertForm.do';">글쓰기</button>
	</c:if>		


		<div class="my-5">
			<ul class="pagination">
				<li><a class="page-link" href="#">&lt;</a></li>
				<li><a class="page-link" href="#">1</a></li>
				<li><a class="page-link" href="#">2</a></li>
				<li><a class="page-link" href="#">3</a></li>
				<li><a class="page-link" href="#">4</a></li>
				<li><a class="page-link" href="#">5</a></li>
				<li><a class="page-link" href="#">&gt;</a></li>
			</ul>
		</div>

		<div class="mb-5">
			<form action="search" method="GET" class="text-center" id="searchForm">
				<select name="searchKey" class="form-control">
					<option value="title">글제목</option>
					<option value="content">내용</option>
					<option value="titcont">제목+내용</option>
				</select>
				<input type="text" name="searchValue" class="form-control">
				<button class="form-control btn btn-primary" id="searchBtn">검색</button>
			</form>


		</div>
	</div>
	<jsp:include page="../common/footer.jsp"></jsp:include>

	<script>
		// 공지사항 상세보기 기능 (jquery를 통해 작업)
		// id가 list-table인 요소의 후손 중에 td 하나가 클릭이 되면  해당 td의 첫번째 형제를 선택해라.
		$("#list-table td").on("click", function(){
			var noticeNo = $(this).parent().children().eq(0).text();
			//	클릭 이벤트가 발생한 요소의 부모의 자식들 중의 제일 첫번째 자식.
			// console.log(noticeNo);
			
			// 얻어온 공지사항 글 번호를 쿼리스트링으로 작성하여 상세조회 요청
			location.href = "${contextPath}/notice/view.do?no=" + noticeNo;
			// ${contextPath}/notice/view.do : 상세조회를 위한 주소
			
		});
	</script>


</body>
</html>
```
<br>

## 전체 noticeView.jsp
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>공지사항</title>
<style>
	#notice-area{ height: 700px;}
	#notice-content{ padding-bottom:150px;}
	
</style>
</head>
<body>
	<jsp:include page="../common/header.jsp"></jsp:include>
	<div class="container my-5">

		<div id="notice-area">

			<!-- Title -->
			<h1 class="mt-4">${notice.noticeTitle }</h1>

			<!-- Writer -->
			<p class="lead">
				작성자 : <a class="idSelect">${notice.memberId }</a>
			</p>

			<hr>

			<!-- Date -->
			<p>
				 ${notice.noticeCreateDate }
				 <span class="float-right">조회수  ${notice.readCount }</span>
			</p>

			<hr>

			<!-- Content -->
			<div id="notice-content">${notice.noticeContent }</div>
			
			<hr>
			
			<div>
	
				<%-- 로그인된 회원이 해당 글의 작성자인 경우 --%>
				<!-- float-right 속성 : 오른쪽으로 정렬됨, 화면상에선 목록,수정,삭제 순서대로 보임.  -->
				<c:if test="${!empty loginMember && (loginMember.memberId == notice.memberId) }">
					<button class="btn btn-primary float-right" id="deleteBtn">삭제</button>
					<a href="updateForm.do?no=${param.no}" class="btn btn-primary float-right ml-1 mr-1">수정</a>
					<!-- request 파라미터 중 no가 존재하고 있음.-->
				</c:if>
				
				<a href="list.do" class="btn btn-primary float-right">목록으로</a>
			</div>
			
		</div>

	</div>
	<jsp:include page="../common/footer.jsp"></jsp:include>
	
	
	<!-- 삭제버튼 클릭 시  -->
	<script>
		$("#deleteBtn").on("click",function(){
			if (confirm("정말 삭제 하시겠습니까?")){
				location.href ="delete.do?no=${param.no}"
			}
		});
			
	</script>
	
</body>
</html>

```
<br>

## 전체 noticeInsert.jsp

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>공지사항</title>
<style>
	#notice-area{ height: 700px;}
</style>
</head>
<body>
  <jsp:include page="../common/header.jsp"></jsp:include>
	<div class="container my-5">


			<h3>공지사항 등록</h3>
		      <hr>
		      <div class="bg-white rounded shadow-sm container py-3" id="notice-area">
		        <form method="POST" action="insert.do" role="form" onsubmit="return noticeValidate();">
		          <div class="form-inline mb-2">
		            <div class="input-group">
		              <label class="input-group-addon mr-3">제목</label>
		              <input type="text" class="form-control" id="noticeTitle" name="noticeTitle" size="70">
		            </div>
		          </div>
		
		          <div class="form-inline mb-2">
		            <div class="input-group">
		              <label class="input-group-addon mr-3">작성자</label>
		              <h5 class="my-0">${loginMember.memberId }</h5>
		            </div>
		          </div>
		
		
		          <div class="form-inline mb-2">
		            <div class="input-group">
		              <label class="input-group-addon mr-3">작성일</label>
		              <h5 class="my-0" id="today"></h5>
		            </div>
		          </div>
		
		          <hr>
		
		          <div class="form-group">
		            <div><label for="content">내용</label> </div>
		            <!-- textarea == input태근  -->
		            <textarea class="form-control" id="noticeContent" name="noticeContent" rows="15" style="resize: none;"></textarea>
		          </div>
		
		
		        <hr class="mb-4">
		
		        <div class="text-center">
					<button type="submit" class="btn btn-primary">등록</button>
					<a href="list" class="btn btn-primary">목록으로</a>
				</div>
		        
		        </form>
		      </div>

	</div>
	<jsp:include page="../common/footer.jsp"></jsp:include>
	
	<script>
		(function printToday(){
			// 오늘 날짜 출력 
			var now = new Date(); // 현재 시간
			var year = now.getFullYear(); // 201  Date라는 내장객체에 존재하는 함수
			var month = now.getMonth() + 1; // 0~11중 하나가 반환
			var date = now.getDate(); // 4
		
			var str = year + "-" + (month<10? "0"+month : month) + "-" + (date<10 ? "0" +date : date);
			
			$("#today").text(str);
		})();
		
		
		// 유효성 검사
		function noticeValidate(){
			// 제목에 아무것도 작성하지 않음
			if( $("#noticeTitle").val().trim().length == 0){
				alert("제목을 입력해 주세요.");
				$("#title").focus();
				return false;
			}
			
			// 내용에 아무것도 작성하지 않음
			if( $("#noticeContent").val().trim().length == 0){
				alert("내용을 입력해 주세요.");
				$("#content").focus();
				return false;
			}
		}
		
	</script>
</body>
</html>
```
<br>

## 전체 NoticeController

```java
package com.kh.wsp.notice.controller;

import java.io.IOException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import com.kh.wsp.member.model.vo.Member;
import com.kh.wsp.notice.model.service.NoticeService;
import com.kh.wsp.notice.model.vo.Notice;

// 모든 notice 관련 요청을 받아들임
@WebServlet("/notice/*")
public class NoticeController extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		// Front Controller 패턴
		// - 클라이언트의 요청을 한 곳으로 집중시켜 개발하는 패턴
		// - 요청마다 Servlet을 생성하는 것이 아닌 하나의 Servlet에 작성하여 관리가 용이해짐.
		
		// Controller의 역할 : 요청에 맞는 Service를 호출, 응답을 위한 View 선택
		
		String uri = request.getRequestURI(); // 전체 요청 주소
		// ex) /wsp/notice/list.do
		
		String contextPath = request.getContextPath();
		// ex) /wsp
		
		String command = uri.substring((contextPath + "/notice").length() );
		
//		System.out.println(uri);
//		System.out.println(contextPath);
//		System.out.println(command);
		
		// 컨트롤러 내에서 공용으로 사용할 변수 미리 선언
		String path = null; // forward 또는  redirect 경로를 저장할 변수
		RequestDispatcher view = null; // 요청 위임 객체
		
		// sweet alert 메세지 전달하는 용도
		String swalIcon = null;
		String swalTitle = null;
		String swalText = null;
		
		String errorMsg = null;
		
		try {
			NoticeService service = new NoticeService();
			
			// 공지사항 목록 조회 Controller **********
			if(command.equals("/list.do")) {
				errorMsg = "공지사항 목록 조회 중 오류 발생";
				
				// 비즈니스 로직 처리 후 결과 반환받기
				List<Notice> list = service.selectList();
				// list를 jsp로 가져가서 화면에 출력해야함
				
				// 요청을 위임할 jsp 경로 지정
				path ="/WEB-INF/views/notice/noticeList.jsp";
				
				// 요청 위임 시 전달할 값 세팅
				request.setAttribute("list", list);
				
				// 요청 위임 객체 생성 후 위임 진행
				view = request.getRequestDispatcher(path);
				view.forward(request, response);
			}
			
			// 공지사항 상세 조회 Controller **********
			else if(command.equals("/view.do")) {
				// command = 요청주소 가장 끝만 잘라내어 저장하는 변수
				// 상세조회 요청이 왔다면
				errorMsg = "공지사항 상세 조회 과정에서 오류 발생";
				
				// 파라미터 얻어오기
				// 쿼리스트링으로 전달된 공지사항 번호를 int형으로 파싱하여 저장
				int noticeNo = Integer.parseInt(request.getParameter("no"));
				
				// 상세조회 비즈니스 로직 수행 결과 반환
				Notice notice = service.selectNotice(noticeNo);
				
				// 조회 결과에 따른 view 연결 처리
				if(notice != null) { // 조회 성공
					// 경로 지정
					path = "/WEB-INF/views/notice/noticeView.jsp";
					
					// 요청 위임
					request.setAttribute("notice", notice);
					view = request.getRequestDispatcher(path);
					view.forward(request, response);
				}else { // 조회 실패
					HttpSession session = request.getSession();
					session.setAttribute("swalIcon", "error");
					session.setAttribute("swalTitle", "공지사항 조회 실패");
					// 이전 주소로 이동
					response.sendRedirect(request.getHeader("referer"));
				}
			}
			
			// 공지사항 작성 화면 전환 Controller **********
			else if(command.contentEquals("/insertForm.do")) {
				path = "/WEB-INF/views/notice/noticeInsert.jsp";
				view = request.getRequestDispatcher(path);
				view.forward(request, response);
			}
			
			
			
			// 공지사항 등록 Controller **********
			else if(command.contentEquals("/insert.do")) {
				errorMsg = "공지사항 등록 과정에서 에러 발생.";
				
				// 파라미터로 전달된 값 저장
				String noticeTitle = request.getParameter("noticeTitle");
				String noticeContent = request.getParameter("noticeContent");
				
				// 세션에서 회원 번호 얻어오기
				HttpSession session = request.getSession();
				
				int noticeWriter = ((Member)session.getAttribute("loginMember")).getMemberNo();
				
				// Map을 이용하여 데이터 전달(VO 대신 사용하는 경우가 많음)
				Map<String, Object> map = new HashMap<String, Object>();
				map.put("noticeTitle", noticeTitle);
				map.put("noticeContent", noticeContent);
				map.put("noticeWriter", noticeWriter);
				
				// 공지사항 등록 비즈니스 로직 수행 후 결과 반환
				int result = service.insertNotice(map);
				
				if(result>0) { // 삽입 성공 시
					// result에는 삽입된 공지사항 번호가 저장되어 있음.
					path = "view.do?no="+result; // 쿼리스트링을 이용하여 상세조회 요청
					swalIcon = "success";
					swalTitle = "공지사항이 등록되었습니다.";
				}else {
					path = "list.do"; // 목록으로 돌아감
					swalIcon = "error";
					swalTitle = "공지사항 등록 실패";
				}
				
				session.setAttribute("swalIcon",swalIcon);
				session.setAttribute("swalTitle", swalTitle);
				
				response.sendRedirect(path);
			}
			
			
			
			
			// 공지사항 수정 화면  출력용 Controller **********
			else if(command.equals("/updateForm.do")) {
				errorMsg = "공지사항 수정 화면 전환 과정에서 오류 발생.";
				
				int noticeNo = Integer.parseInt(request.getParameter("no"));
				
				Notice notice = service.updateView(noticeNo); 
				
				if(notice !=null) {
					path = "/WEB-INF/views/notice/noticeUpdate.jsp";
					request.setAttribute("notice",notice);
					view = request.getRequestDispatcher(path);
					view.forward(request, response);
				}else {
					request.getSession().setAttribute("swalIcon", "error");
					request.getSession().setAttribute("swalText", "공지사항 수정을 위한 조회 실패");
					
					response.sendRedirect("view.do?no=" + noticeNo);
				}
			}
			
			
			
			
			// 공지사항 수정 Controller
			else if(command.contentEquals("/update.do")) {
				
				errorMsg = "공지사항 수정에서 오류 발생";
				
				// 파라미터 얻어오기
				int noticeNo = Integer.parseInt(request.getParameter("no"));
				String noticeTitle = request.getParameter("noticeTitle");
				String noticeContent = request.getParameter("noticeContent");
				
				// 비즈니스 로직 수행
				
				Map<String, Object> map = new HashMap<String, Object>();
				map.put("noticeNo", noticeNo);
				map.put("noticeTitle", noticeTitle);
				map.put("noticeContent", noticeContent);
				
				int result = service.updateNotice(map);
				
				// 로직 수행 성공 시  "공지사항이 수정되었습니다." swal 출력
				if (result>0) {
					path = "view.do?no="+noticeNo; // 쿼리스트링을 이용하여 상세조회 요청
					swalIcon = "success";
					swalTitle = "공지사항 수정 성공 ";
					
				}
				// 로직 수행 실패 시 " 공지사항 수정 실패" swal 출력
				else {
					path = "list.do"; // 목록으로 돌아감
					swalIcon = "error";
					swalTitle = "공지사항 등록 실패";
				}
				
				request.getSession().setAttribute("swalIcon", swalIcon);
				request.getSession().setAttribute("swalTitle", swalTitle);

				// 수정된 공지사항 상세조회로 redirect
				response.sendRedirect(path);
				
			}
			
			
			
			
			
			// 공지사항 삭체 Controller **********
			else if (command.contentEquals("/delete.do")){
				errorMsg ="공지사항 삭제 과정에서 오류 발생";
				
				int noticeNo = Integer.parseInt(request.getParameter("no"));
				
				// 비즈니스 로직 수행
				int result = service.updateNoticeFl(noticeNo);
				
				if(result>0) {
					swalIcon = "success";
					swalTitle = "공지사항이 삭제되었습니다.";
					path = "list.do";
				}else {
					swalIcon = "error";
					swalTitle = "공지사항 삭제 실패";
					path = request.getHeader("referer");
				}
				
				request.getSession().setAttribute("swalIcon", swalIcon);
				request.getSession().setAttribute("swalTitle", swalTitle);
				
				response.sendRedirect(path);
			}
					
			
			
		}catch(Exception e) {
			e.printStackTrace();
			path = "/WEB-INF/views/common/errorPage.jsp";
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

## 전체 NoticeService

```java
package com.kh.wsp.notice.model.service;

import static com.kh.wsp.common.JDBCTemplate.*;

import java.sql.Connection;
import java.util.List;
import java.util.Map;

import com.kh.wsp.notice.model.dao.NoticeDAO;
import com.kh.wsp.notice.model.vo.Notice;

public class NoticeService {

	private NoticeDAO dao = new NoticeDAO();

	
	
	/** 공지사항 목록 조회 Service
	 * @return list
	 * @throws Exception
	 */
	public List<Notice> selectList() throws Exception {
		Connection conn = getConnection();
		
		List<Notice> list = dao.selectList(conn);
		
		close(conn);
		
		return list;
	}



	/** 공지사항 상세 조회 Service
	 * @param noticeNo
	 * @return notice
	 * @throws Exception
	 */
	public Notice selectNotice(int noticeNo) throws Exception {
		
		// 1) 커넥션 얻어오기
		Connection conn = getConnection();
		
		// 2) dao 수행 후 결과 반환받기
		Notice notice = dao.selectNotice(conn, noticeNo);
		
		// 3) 공지사항 조회 성공 시 조회수 증가
		if(notice !=null ) {
			int result = dao.increaseReadCount(conn,noticeNo);
			
			// 트랜잭션 처리
			if(result>0) {
				commit(conn);
			// 이미 조회된 notice에서 readCount 값을 1 증가
				notice.setReadCount(notice.getReadCount()+1);
			}
			else rollback(conn);
			
		}
		
		// 4) 커넥션 반환
		close(conn);
		
		// 5) 결과 반환
		return notice;
	}



	/** 공지사항 등록 Service
	 * @param map
	 * @return result
	 * @throws Exception
	 */
	public int insertNotice(Map<String, Object> map) throws Exception {
		Connection conn = getConnection();
		
		// 1) 작성될 글 번호를 먼저 얻어오는 DAO 메소드 수행
		// 이유 1) 글 작성 성공 후 해당 글 상세 조회 페이지로 redirect 하기 위해 필요.
		// 이유 2) 글 작성 + 파일 첨부 시 발생하는 시퀀스 번호 밀림 현상을 제거하기 위해
		int noticeNo = dao.selectNextNo(conn);
		
		int result = 0;
		
		
		if(noticeNo >0) { // 다음 글 번호를 얻어오는데 성공한 경우 
			// 2. 실제 공지글 등록 DAO 메소드 수행
			
			// map에 얻어온 다음 글 번호를 세팅
			map.put("noticeNo", noticeNo);
			
			// 크로스 사이트 스크립팅 방지 처리
			map.put("noticeTitle", replaceParameter((String)map.get("noticeTitle")));
									// <script> -> &lt;script&gt; 
			map.put("noticeContent", replaceParameter((String)map.get("noticeContent")));
			
			// 개행문자 변경 처리
			// textarea의 개행문자는 \r\n 이지만
			// 상세조회에서 사용되는 div태그의 개행문자는 <br>이기 때문에 변경이 필요함
			
			map.put("noticeContent", ((String)map.get("noticeContent")).replaceAll("\r\n", "<br>"));

			result = dao.insertNotice(conn,map);
		
			// 트랜잭션 처리
			if(result>0) {
				commit(conn);
				// 삽입 성공 시 result에 작성한 글 번호를 대입
				result = noticeNo; // 글번호가 최종적으로 반환됨
			}
			else rollback(conn);
		}
		close(conn);
		return result;
	}
		
		
		// 크로스 사이트 스크립팅
		// 웹 애플리케이션에서 많이 나타나는 보안 취약점 중 하나로
		// 웹 사이트 관리자가 아닌 사용자가 웹 페이지에 악성 스크립트를 삽입할 수 있는 취약점
		
		// 크로스 사이트 스크립팅 방지 메소드
		private String replaceParameter(String param) {
			String result = param;
			
			if(result != null) {
				result = result.replaceAll("&", "&amp;"); 
				result = result.replaceAll("<", "&lt;"); 
				result = result.replaceAll(">", "&gt;"); 
				result = result.replaceAll("\"", "&quot;"); 
			}
			
			return result;
		}


		/** 공지사항 수정 화면 Service
		 * @param noticeNo
		 * @return notice
		 * @throws Exception
		 */
		public Notice updateView(int noticeNo) throws Exception {
			Connection conn = getConnection();
			
			// 공지사항 상세조회 DAO 호출
			Notice notice = dao.selectNotice(conn, noticeNo);
			
			// textarea에 출력하기 위해 개행문자 변경
			if(notice !=null) {
				notice.setNoticeContent(notice.getNoticeContent().replaceAll("<br>", "\r\n"));
			}
			
			close(conn);
			
			return notice;
			
		}

		/** 공지사항 수정 Service
		 * @param map
		 * @return result
		 * @throws Exception
		 */
		public int updateNotice(Map<String, Object> map) throws Exception {
			Connection conn = getConnection();
			
			// 크로스 사이트 스크립팅 방지
			map.put("noticeTitle", replaceParameter((String)map.get("noticeTitle")));
			map.put("noticeContent", replaceParameter((String)map.get("noticeContent")));
			map.put("noticeContent", ((String)map.get("noticeContent")).replaceAll("\r\n", "<br>"));
			
			
			int result = dao.updateNotice(conn,map);
			
			if(result>0) commit(conn);
			else    rollback(conn);
			
			close(conn);
			
			return result;
		}

		
		/** 공지사항 삭제 service
		 * @param noticeNo
		 * @return result
		 * @throws Exception
		 */
		public int updateNoticeFl(int noticeNo) throws Exception {
			Connection conn = getConnection();
					
			int result = dao.updateNoticeFl(conn,noticeNo);
			
			if(result>0) commit(conn);
			else   rollback(conn);
			
			close(conn);
			
			return result;
		}
}
```
<br>

## 전체 NoticeDAO

```java
package com.kh.wsp.notice.model.dao;

import static com.kh.wsp.common.JDBCTemplate.*;

import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.Properties;

import com.kh.wsp.notice.model.vo.Notice;

public class NoticeDAO {
   
   // 자주 사용하는 JDBC 참조 변수 미리 선언
   private Statement stmt = null;
   private PreparedStatement pstmt = null;
   private ResultSet rset = null;
   
   
   // 외부 XML 파일에 작성된 sql을 읽어올 변수 선언
   Properties prop = null;
   
   // 기본 생성자로 NoticeDAO 객체 생성 시 SQL이 작성된 xml 파일 얻어오기
   public NoticeDAO() {
      String fileName = NoticeDAO.class.getResource("/com/kh/wsp/sql/notice/notice-query.xml").getPath();
      try {
         prop = new Properties();
         prop.loadFromXML(new FileInputStream(fileName)); 
      } catch (Exception e) {
         e.printStackTrace();
      }
   }

   
   
   
/**	공지사항 목록 조회 DAO
 * @param conn
 * @return list
 * @throws Exception
 */
public List<Notice> selectList(Connection conn) throws Exception {
	List<Notice> list = null;
	
	String query = prop.getProperty("selectList");
	
	try {
		stmt = conn.createStatement();
		rset = stmt.executeQuery(query);
		
		
		// sql 수행 후 DB관련 문제가 발생하지 않으면 
		// 조회 내용을 저장할 수 있는 ArrayList 생성
		list = new ArrayList<Notice>();
		
		while(rset.next()) {
			Notice notice = new Notice();
			notice.setNoticeNo(rset.getInt("NOTICE_NO"));
			notice.setNoticeTitle(rset.getString("NOTICE_TITLE"));
			notice.setMemberId(rset.getString("MEMBER_ID"));
			notice.setReadCount(rset.getInt("READ_COUNT"));
			notice.setNoticeCreateDate(rset.getDate("NOTICE_CREATE_DT"));
			
			list.add(notice);
		}
		
	}finally {
		close(rset);
		close(stmt);
	}
	
	return list;
}


/** 공지사항 상세조회 DAO
 * @param conn
 * @param noticeNo
 * @return notice
 * @throws Exception
 */
public Notice selectNotice(Connection conn, int noticeNo) throws Exception {
	// 1) 결과를 저장할 변수 선언
	Notice notice = null;
	
	// 2) SQL 구문 얻어오기
	String query = prop.getProperty("selectNotice");
	
	// 3) SQL 수행 후 결과를 notice에 저장
	try {
		pstmt = conn.prepareStatement(query);
		pstmt.setInt(1, noticeNo);
		
		rset=pstmt.executeQuery();
		if(rset.next()) {
			notice = new Notice();
			notice.setNoticeTitle(rset.getString("NOTICE_TITLE"));		
			notice.setNoticeContent(rset.getString("NOTICE_CONTENT"));
			notice.setMemberId(rset.getString("MEMBER_ID"));
			notice.setReadCount(rset.getInt("READ_COUNT"));
			notice.setNoticeCreateDate(rset.getDate("NOTICE_CREATE_DT"));
		}
		
	}finally {
		// 4) 사용한 JDBC 객체 반환
		close(rset);
		close(pstmt);
	}
	
	// 5) 결과 반환
	return notice;
}


/** 조회수 증가 DAO
 * @param conn
 * @param noticeNo
 * @return result
 * @throws Exception
 */
public int increaseReadCount(Connection conn, int noticeNo) throws Exception {
	int result = 0;
	
	String query = prop.getProperty("increaseReadCount");
	
	try {
		pstmt = conn.prepareStatement(query);
		pstmt.setInt(1, noticeNo);
		result = pstmt.executeUpdate();
	}finally {
		close(pstmt);
	}
	return result;
}


/** 공지사항 등록 시 사용될 번호 반환 DAO
 * @param conn
 * @return noticeNo
 * @throws Exception
 */
public int selectNextNo(Connection conn) throws Exception {
	int noticeNo = 0;
	String query = prop.getProperty("selectNextNo");
	
	try {
		stmt = conn.createStatement();
		
		rset = stmt.executeQuery(query);
		
		if(rset.next()) {
			noticeNo = rset.getInt(1);
		}
		
	}finally {
		close(rset);
		close(stmt);
	}
	
	return noticeNo;
}


/**	공지사항 등록 DAO
 * @param conn
 * @param map
 * @return result
 * @throws Exception
 */
public int insertNotice(Connection conn, Map<String, Object> map) throws Exception{
	int result =0;
	
	String query = prop.getProperty("insertNotice");
	
	try {
		pstmt = conn.prepareStatement(query);
		pstmt.setInt(1, (int)map.get("noticeNo"));
		pstmt.setString(2, (String)map.get("noticeTitle"));
		pstmt.setString(3, (String)map.get("noticeContent"));
		pstmt.setInt(4, (int)map.get("noticeWriter"));
		
		result = pstmt.executeUpdate();
	}finally {
		close(pstmt);
	}
	
	return result;
}


/** 공지사항 수정 DAO
 * @param conn
 * @param map
 * @return result
 * @throws Exception
 */
public int updateNotice(Connection conn, Map<String, Object> map) throws Exception {
	int result =0;
	String query = prop.getProperty("updateNotice");
	try {
		pstmt = conn.prepareStatement(query);
		pstmt.setString(1, (String)map.get("noticeTitle"));
		pstmt.setString(2, (String)map.get("noticeContent"));
		pstmt.setInt(3, (int)map.get("noticeNo"));
		
		result = pstmt.executeUpdate();
		
	}finally {
		close(pstmt);
	}
	return result;
}


/** 공지사항 삭제 DAO
 * @param conn
 * @param noticeNo
 * @return result
 * @throws Exception
 */
public int updateNoticeFl(Connection conn, int noticeNo) throws Exception {
	int result =0;
	
	String query = prop.getProperty("updateNoticeFl");
	
	try {
		pstmt = conn.prepareStatement(query);
		pstmt.setInt(1, noticeNo);
		
		result = pstmt.executeUpdate();
	}finally {
		close(pstmt);
	}
	return result;
}
  
}
```
<br>

## 전체 SQL

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>

<!-- 공지사항 목록 조회  -->
<entry key="selectList">
SELECT * FROM V_NOTICE_LIST
WHERE NOTICE_FL='Y'
</entry>

<!-- 공지사항 상세 조회  -->
<entry key="selectNotice">
SELECT NOTICE_TITLE, NOTICE_CONTENT, MEMBER_ID, READ_COUNT,NOTICE_CREATE_DT
FROM NOTICE
JOIN MEMBER ON(NOTICE_WRITER = MEMBER_NO)
WHERE NOTICE_NO=?
AND NOTICE_FL='Y'
</entry>

<!-- 조회수 증가  -->
<entry key="increaseReadCount">
UPDATE NOTICE SET
READ_COUNT = READ_COUNT+1
WHERE NOTICE_NO=?
</entry>

<!-- 공지사항 등록 시 사용될 번호 반환 : 다음 시원스 조회  -->
<entry key="selectNextNo">
SELECT SEQ_NNO.NEXTVAL FROM DUAL
</entry>

<!-- 공지사항 등록  -->
<entry key="insertNotice">
INSERT INTO NOTICE(NOTICE_NO, NOTICE_TITLE, NOTICE_CONTENT, NOTICE_WRITER)
VALUES (?,?,?,?)
</entry>


<!-- 공지사항 수정  -->
<entry key="updateNotice">
UPDATE NOTICE SET
NOTICE_TITLE =?,
NOTICE_CONTENT = ?
WHERE NOTICE_NO = ?
</entry>


<!-- 공지사항 삭제 -->
<entry key="updateNoticeFl">
UPDATE NOTICE SET
NOTICE_FL= 'N'
WHERE NOTICE_NO=?
</entry>
</properties>
```

<br><br><br><br>