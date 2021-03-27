---
title: "[숙소] 등록하기 -2"
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

## [숙소] 등록하기

> 숙소 등록화면에서 등록 버튼을 누르면 /room/insert로 요청 됨

- 숙소 등록 Controller

```java
else if(command.equals("/insert")) {
    errorMsg = "숙소 등록 과정에서 오류 발생";
    
    // form태그에서 encType이 multipart/form-data형식이면
    // 기존에 사용하던 request  객체로 파라미터를 얻어올 수 없다.
    
    // 1. MultipartRequest 객체 생성하기
    // 1-1. 전송 파일 용량 지정(byte단위)
    int maxSize = 20* 1024 * 1024; // 20MB
    
    // 1-2. 서버에 업로드된 파일을 저장할 경로 지정
    String root = request.getSession().getServletContext().getRealPath("/");
    
    String filePath = root + "resources/image/uploadRoomImages/";
    
    // 1-3. 파일명 변환을 위한 클래스 작성 (Attachment.java)
    
    // 1-4. MultipartRequest 객체 생성 (객체 생성과 동시에 파라미터로 넘어온 내용 중 파일이 서버에 바로 저장됨)
    MultipartRequest multiRequest = new MultipartRequest(request, filePath, maxSize, "UTF-8", new MyFileRenamePolicy());
    
    
    // 2. 생성한 객체에서 파일 정보만을 얻어와 별도의 List에 모두 저장
    
    // 2-1. 파일 정보를 모두 저장할 List 객체 생성
    List<Attachment> fList= new ArrayList<Attachment>();
    
    // 2-2. 업로드된 파일의 name 모두 받기
    Enumeration<String> files = multiRequest.getFileNames();
    
    
    // 2-3. 얻어온 Enumeration 객체에 요소를 하나씩 반복 접근, 
    // 업로드된 파일 정보를 Attachment객체에 저장 후 iList에 추가
    
    while(files.hasMoreElements()) { //다음 요소가 있다면
        String name = files.nextElement(); //img0
        
        // 제출받은 file태그 요소 중 업로드된 파일이 있을 경우
        if(multiRequest.getFilesystemName(name)!=null) {
            // Attachment 객체에 파일 정보 저장
            Attachment temp = new Attachment();
            
            temp.setFileName(multiRequest.getFilesystemName(name));
            temp.setFilePath(filePath);
            

            // name 속성에 따라 fileLevel 지정
            int fileLevel =0;
            
            switch(name) {
            case "img0" : fileLevel =0; break;
            case "img1" : fileLevel =1; break;
            case "img2" : fileLevel =2; break;
            case "img3" : fileLevel =3; break;
            case "img4" : fileLevel =4; break;
            case "img5" : fileLevel =5; break;
            
            }
            temp.setFileLevel(fileLevel);
            
            // fList에 추가
            fList.add(temp);
        }
    }// end while
    
    // 3.파일정보를 제외한 게시글 정보를 얻어와 저장하기
    String roomName = multiRequest.getParameter("roomName");
    String location2 = multiRequest.getParameter("location2");
    String phone = multiRequest.getParameter("phone");
    String checkin = multiRequest.getParameter("checkin");
    String checkout = multiRequest.getParameter("checkout");
    String[] facilityArr = multiRequest.getParameterValues("facility");
    String facility = null;
    if(facilityArr!=null) { // 숙소 시설 배열이 비어있지 않다면.
        facility= String.join(",", facilityArr);
    }
    String[] dogArr = multiRequest.getParameterValues("dog");
    String dog = null;
    if(dogArr!=null) { // 견종이 비어있지 않다면.
        dog= String.join(",", dogArr);
    }
    String roomInfo = multiRequest.getParameter("room_info");
    
    
    
    
    // 세션에서 로그인한 회원의 번호를 얻어옴
    Member loginMember = (Member)request.getSession().getAttribute("loginMember");
    int memberNo = loginMember.getMemberNo();
    

    
    // 얻어온 변수들을 모두 저장할 Map  생성
    Map<String,Object> map = new HashMap<String,Object>();
    map.put("roomName",roomName);
    map.put("location2",location2);
    map.put("phone",phone);
    map.put("fList",fList);
    map.put("checkin", checkin);
    map.put("checkout", checkout);
    map.put("facility", facility);
    map.put("dog", dog);
    map.put("roomInfo", roomInfo);
    map.put("memberNo", memberNo);
    
    // 4. 게시글 등록 비즈니스 로직 수행 후 결과 반환받기
    int result = service.insertRoom(map);
    
    if(result>0) {// DB에 데이터 등록 성공하면 result에 병원번호가 저장되어 있다.
        swalIcon = "success";
        swalTitle = "숙소 등록 성공";
        path = "view?cp=1&roomNo="+result;
        
        
    } else {
        swalIcon = "error";
        swalTitle ="등록 실패";
        path = "list";
    }
    
    request.getSession().setAttribute("swalIcon", swalIcon);
    request.getSession().setAttribute("swalTitle", swalTitle);
        
        response.sendRedirect(path);
}
```

<br><br>

- 게시글 등록 Service

```java
	public int insertRoom(Map<String, Object> map) throws Exception{
		Connection conn = getConnection();
		int result = 0;
		
		// 1. 추가될 번호 가져오기
		int insertNo = dao.selectNextNo(conn);
		
		if(insertNo>0) {
			// 얻어온 번호를 map에 추가해준다.
			map.put("insertNo", insertNo);
			
			// 크로스 사이트 스크립팅 방지
			String roomInfo = (String)map.get("roomInfo");
			
			roomInfo = replaceParameter(roomInfo);
			
			// 내용에 개행문자 변경처리.
			roomInfo = roomInfo.replaceAll("\r\n", "<br>");
			
			// 처리된 내용을 다시 map에 추가
			map.put("roomInfo",roomInfo);
			
			
			try {
				// 파일 정보 제외한 정보  등록하는 DAO호출
				result = dao.insertRoom(conn,map);
				
				
				// 파일 정보만 등록하는 DAO
				List<Attachment> fList = (List<Attachment>)map.get("fList");
				
				if(result>0 && !fList.isEmpty()) {
					// 게시글부분, 파일정보 모두 있다면.
					 result =0; // result를  재활용하기 위해 초기화시킴
					 
					 
					 // fList의 요소를 하나씩 반복접근하여 DAO메소드를 반복 호출해 정보를 삽입함.
					 for(Attachment at : fList) {
						 // 파일 정보가 저장된 Attachment 객체에 해당 파일이 작성된 게시글 번호를 추가 세팅
						 at.setRoomNo(insertNo);
						 
						 result = dao.insertAttachment(conn,at); 
						 
						 if(result==0) { // 등록 실패.이미지 정보가 DB에 안들어갔을떄.
							 
							 // 강제로 예외를 발생시킨다. ( 사용자 정의 예외 )
							 throw new FileInsertFailedException("파일 정보 삽입 실패");
							 
						 }
						System.out.println(result);
					 }
				}
				
			}catch(Exception e) {
				// 게시글 또는 파일 정보 삽입 중 에러 발생 시 서버에 저장된 파일을 삭제하는 작업이 필요함.
				// 왜? MultipartRequest 객체가 생성되면 바로 서버에 등록이 되기 때문에
				// 객체는 생성되었지만 비즈니스 로직 실패시  데이버와 서버의 정보 불일치가 일어남.
				List<Attachment> fList = (List<Attachment>)map.get("fList");
				
				if(!fList.isEmpty()) {
					for(Attachment at : fList ) {
						String filePath = at.getFilePath();
						String fileName = at.getFileName();
						
						File deleteFile = new File(filePath + fileName);
						
						if(deleteFile.exists()) {
							// 해당 경로에 해당 파일이 존재하면
							deleteFile.delete(); // 해당 파일 삭제
						}
				}
				
			}
			// 에러페이지가 보여질 수 있도록 catch 한 Exception을 Controller로 던져줌
			throw e;
				
			}// end catch
			
			if(result > 0) {
				commit(conn);
				// 삽입 성공 시 해당 병원의 상세 조회 화면으로 이동해야되기 때문에 
				// 글 번호를 받환할 수 있도록 result에 hospitalNo를 대입
				result = insertNo;
				
			}else {
				rollback(conn);
			}
			
		}
		close(conn);
		return result;
	}
```
<br><br>

- 사용자가 문자열에 스크립트를 삽입하여 실행하는 것을 막기 위해 
  크로스 사이트 스크립팅 방지처리를 해준다.(HTML태그 불허용)

```java
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
```
<br><br>


- 다음 게시글 번호 가져오는 DAO

> 게시글 등록 시 이용자들 간 게시글 번호가 꼬일 수 있어서 따로 불러온다.

```java
public int selectNextNo(Connection conn) throws Exception {
    int insertNo = 0;
    String query = prop.getProperty("selectNextNo");
    
    try {
        stmt = conn.createStatement();
        rset = stmt.executeQuery(query);
        
        if(rset.next()) {
            insertNo = rset.getInt(1);
        }
    }finally {
        close(rset);
        close(stmt);
    }
    return insertNo;
}
```

<br><br>

- 다음 게시글 번호 가져올 때 사용되는 sql문

```sql
<entry key="selectNextNo">
SELECT SEQ_ROOM.NEXTVAL FROM DUAL
</entry>
```

<br><br>

- 숙소 정보 등록 Service(파일정보 제외)

```java
public int insertRoom(Connection conn, Map<String, Object> map) throws Exception {
    int result =0;
    
    String query = prop.getProperty("insertRoom");
    
    try {
        pstmt = conn.prepareStatement(query);
        pstmt.setInt(1, (int)map.get("insertNo"));
        pstmt.setString(2, (String)map.get("roomName"));
        pstmt.setString(3, (String)map.get("location2"));
        pstmt.setString(4, (String)map.get("phone"));
        pstmt.setString(5, (String)map.get("roomInfo"));
        pstmt.setString(6, (String)map.get("checkin"));
        pstmt.setString(7, (String)map.get("checkout"));
        pstmt.setString(8, (String)map.get("facility"));
        pstmt.setString(9, (String)map.get("dog"));
        pstmt.setInt(10, (int)map.get("memberNo"));
        
        result = pstmt.executeUpdate();
    }finally {
        close(pstmt);
    }
    
    return result;
}
```
<br><br>

- 숙소 등록 시 사용되는 sql문

```sql
<entry key="insertRoom">
INSERT INTO ROOM
VALUES(?, ?, ?, ?, ?, ?, ?, ?, ?, DEFAULT, DEFAULT, ? )
</entry>
```

<br><br>


- 파일 정보 등록 Service

```java
public int insertAttachment(Connection conn, Attachment at) throws Exception {
    int result =0;
    
    String query = prop.getProperty("insertAttachment");
    
    try {
        pstmt = conn.prepareStatement(query);
        pstmt.setString(1, at.getFilePath());
        pstmt.setString(2, at.getFileName());
        pstmt.setInt(3, at.getFileLevel());
        pstmt.setInt(4, at.getRoomNo());
        
        result = pstmt.executeUpdate();
    }finally {
        close(pstmt);
    }
    return result;
}
```

<br><br>


- 파일 정보 등록 시 사용되는 sql문

```sql
<entry key="insertAttachment">
INSERT INTO ROOM_IMG
VALUES(SEQ_ROOMIMG.NEXTVAL,?,?,?,?)
</entry>
```

<br><br>