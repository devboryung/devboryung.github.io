---
title: "[숙소] 목록 조회"
excerpt: "세미프로젝트 - 뭉개뭉개"
search: true
categories: 
  - SemiProject
tags: 
  - JSP
  - Servlet
  - Java
  - HTML
  - CSS
toc: true
---


## [숙소] 목록 조회

- 출력 화면

![image](https://user-images.githubusercontent.com/73421820/112473858-3b172600-8db2-11eb-8c9a-085e1d9ab956.png)


<br><br>


> 헤더에서 숙소를 클릭하면 요청되는 주소 

```html
<li><a href="${contextPath }/room/list" class="nav-items" id="nav-room">숙소</a></li>
```

<br><br>


- 전달받을 Controller<br>
 room으로 시작되는 모든 주소를 받은 후 contextPath+/room 을 잘라 command 에 저장해  일치하는 주소와 매핑시킨다.

```java
@WebServlet("/room/*")
public class RoomController extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
   
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String uri = request.getRequestURI(); //전체 요청 주소
				
		String contextPath = request.getContextPath();
		
		String command = uri.substring((contextPath+"/room").length());
		
		// 컨트롤러 내에서 공용으로 사용할 변수 미리 선언
		
		String path = null; // forward 또는 redirect 경로를 저장할 변수
		RequestDispatcher view = null; // 요청 위임 객체
		
		// sweet alert 메세지 전달하는 용도
		String swalIcon = null;
		String swalTitle = null;
		String swalText = null;
		String errorMsg = null;
    try {
    RoomService service = new RoomService();
    
    // 현재 페이지를 얻어와 커리스트링으로 사용할 것.
    // 쿼리스트링은 서버측에서 파라미터로 인식된다.
    String cp = request.getParameter("cp");
    // CurrentPage 사용 이유
    // 1) 페이징 처리 시 계산에 필요한 값이기 때문
    // 2) 효율적인 UI/UX를 제공하기 위해서 사용한다(상세조회/북마크...)
    // 처음엔 얻어 올 값이 없어서 null 이 된다.
```
<br><br>


- 숙소 목록 조회 컨트롤러

```java
if(command.equals("/list")) {
  
  errorMsg = "숙소 목록 조회 중 오류 발생";
  
  
  // 1) 페이징 처리를 위한 값 계산 Service호출
  PageInfo pInfo = service.getPageInfo(cp);
  
  // 2) 숙소 목록 조회 비즈니스 로직 수행
  List<Room> rList = service.selectRoomList(pInfo);
  
  // ************* 썸네일추가 *************
  if(rList!=null) {
    List<Attachment> fList = service.selectThumbnailList(pInfo);
    
    if(!fList.isEmpty()) {
      request.setAttribute("fList", fList);
    }
  }

  // rList, pInfo, fList를 jsp로 가져가 화면에 뿌린다.
  path = "/WEB-INF/views/room/roomList.jsp";
  request.setAttribute("rList", rList);
  request.setAttribute("pInfo", pInfo);
  view = request.getRequestDispatcher(path);
  view.forward(request, response);
  
}
```

<br><br>


- 페이징 처리를 위한 service

```java
public PageInfo getPageInfo(String cp) throws Exception {
  Connection conn = getConnection();
  
  // cp가 null일 경우 1, 아니면 cp를 얻어옴.
  int currentPage = cp == null ? 1 : Integer.parseInt(cp);
  
  // System.out.println(currentPage); 숙소 화면 들어가자마자 1 출력 됨
  
  // DB에서 전체 게시글 수를 조회하여 반환받기
  int listCount = dao.getListCount(conn);
  
  close(conn);
  
  return new PageInfo(currentPage, listCount);
}
```
<br><br>

- 전체 게시글 수를 조회하여 반환받는 DAO

```java
public int getListCount(Connection conn) throws Exception {
  int listCount =0;
  String query = prop.getProperty("getListCount");
  
  try {
    stmt = conn.createStatement();
    rset = stmt.executeQuery(query);
    
    while(rset.next()){
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

- 전체 게시글 수를 조회 시 수행되는 sql문

```sql
<entry key="getListCount">
SELECT COUNT(*) FROM ROOM
WHERE ROOM_DEL_FL = 'N'
</entry>
```


<br><br>


- 숙소 목록 조회 Service

```java
public List<Room> selectRoomList(PageInfo pInfo) throws Exception {
  Connection conn = getConnection();
  
  List<Room> rList = dao.selectRoomList(conn,pInfo);
  
  close(conn);
  return rList;
}
```
<br><br>


- 숙소 목록 조회 DAO

```java
public List<Room> selectRoomList(Connection conn, PageInfo pInfo) throws Exception {
  List<Room> rList = null;
  String query = prop.getProperty("selectRoomList");
  
  try {
    // SQL 구문 조건절에 대입할 변수 생성
    int startRow = (pInfo.getCurrentPage()-1) * pInfo.getLimit()+1;
    int endRow = startRow + pInfo.getLimit()-1;
    // 7개의 글 중에서 1페이지에 해당하는 글을 가져옴 : 7~2번째의 글만 가져오게 됨.
    
    pstmt = conn.prepareStatement(query);
    pstmt.setInt(1, startRow);
    pstmt.setInt(2,  endRow);
    
    rset = pstmt.executeQuery();
    
    rList = new ArrayList<Room>();
    
    while(rset.next()) {
      Room room = new Room(rset.getInt("ROOM_NO"), 
          rset.getString("ROOM_NAME"), rset.getString("LOCATION2"));
      rList.add(room);
    }
  }finally {
    close(rset);
    close(pstmt);
  }
  
  return rList;
}
```
<br><br>

- 숙소 목록 조회 시 수행되는 sql문

```sql
<entry key="selectRoomList">
SELECT * FROM
	(SELECT ROWNUM RNUM, R.*
	 FROM (SELECT * FROM ROOM WHERE ROOM_DEL_FL = 'N' ORDER BY ROOM_NO DESC)R)
WHERE RNUM BETWEEN ? AND ?
</entry>
```

<br><br>

- 게시글에 일치하는 썸네일 목록 조회 Service

```java
public List<Attachment> selectThumbnailList(PageInfo pInfo) throws Exception {
  Connection conn = getConnection();
  
  List<Attachment> fList = dao.selectThumbnailList(conn,pInfo);
  
  close(conn);
  
  return fList;
}
```

<br><br>


- 게시글에 일치하는 썸네일 목록 조회 DAO

```java
public List<Attachment> selectThumbnailList(Connection conn, PageInfo pInfo) throws Exception {
  List<Attachment> fList =null;
  
  String query = prop.getProperty("selectThumbnailList");
  
  // 위치 홀더에 들어갈 시작 행, 끝 행번호 계산
        int startRow = (pInfo.getCurrentPage() -1) * pInfo.getLimit() +1;
        int endRow = startRow + pInfo.getLimit()-1;
      try {		
        pstmt = conn.prepareStatement(query);
        pstmt.setInt(1, startRow);
        pstmt.setInt(2, endRow);
        
        rset = pstmt.executeQuery();
        
        fList = new ArrayList<Attachment>();
        
        while(rset.next()) {
          Attachment at = new Attachment();
          at.setFileName(rset.getString("FILE_NAME"));
          at.setRoomNo(rset.getInt("ROOM_NO"));

          
          fList.add(at);
        }
      }finally {
        close(rset);
        close(pstmt);
      }
  
  return fList;
}
```
<br><br>

- 일치하는 썸네일 목록 조회 시 수행되는 sql문

```sql
<entry key="selectThumbnailList">
SELECT * FROM ROOM_IMG
WHERE ROOM_NO
IN (SELECT ROOM_NO FROM  
	(SELECT ROWNUM RNUM, R.* FROM 
		(SELECT ROOM_NO FROM ROOM
		WHERE ROOM_DEL_FL='N'
		 ORDER BY ROOM_NO DESC)R)
WHERE RNUM BETWEEN ? AND ? )
AND FILE_LEVEL =0
</entry>
```

<br><br>

>전달받은 값을 화면에 뿌려준다

- JSP

```html
<c:choose>
  <c:when test="${empty rList }">
    <div class="row-item">
      <div style="text-align: center; font-size: 18px;">등록된 숙소가
        없습니다.</div>
    </div>
  </c:when>


  <c:otherwise>
    <div class="row-item">
      <table id="list">
        <tr>
          <td><c:forEach var="room" items="${rList}">
              <div class="roomList numberSelect">
                <!-- 썸네일 출력  -->
                <c:set var="flag" value="true" />
                <c:forEach var="thumbnail" items="${fList }">
                  <c:if test="${room.roomNo == thumbnail.roomNo }">
                    <div class="thumbnail_area " style="margin-left: 10px;">
                      <img class="thumbnail_img"
                        src="${contextPath}/resources/image/uploadRoomImages/${thumbnail.fileName}"></img>
                      <c:set var="flag" value="false" />
                    </div>
                  </c:if>
                </c:forEach>
                <c:if test="${flag == 'true'}">
                  <img class="thumbnail_img"
                    src="${contextPath }/resources/image/icon/nonImage.png">
                </c:if>


                <div class="title_area" style="cursor: pointer;">
                  <p class="title">${room.roomName }</p>
                </div>
                <div class="address_area"
                  style="cursor: pointer; margin-left: 10px;">
                  <p class="address">${room.location2 }</p>
                </div>
                <span style="visibility: hidden">${room.roomNo }</span>
              </div>

            </c:forEach></td>
        </tr>
      </table>
    </div>
  </c:otherwise>

</c:choose>

```

<br><br>