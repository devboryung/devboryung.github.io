---
title: "[동물병원] 등록하기 -2"
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

## [동물병원] 등록하기

> 동물병원 등록 화면에서 등록 버튼을 누르면 /hospital/insert로 요청 됨

- 동물병원 등록 Controller


```java
else if(command.equals("/insert")) {
    errorMsg ="동물병원 등록 과정에서 오류 발생";
    
    // form태그에서 encType이 multipart/form-data형식이면
    // 기존에 사용하던 request  객체로 파라미터를 얻어올 수 없다.
    
    // 1. MultipartRequest 객체 생성하기
    // 1-1. 전송 파일 용량 지정(byte단위)
    int maxSize = 20* 1024 * 1024; // 20MB
    
    // 1-2. 서버에 업로드된 파일을 저장할 경로 지정
    String root = request.getSession().getServletContext().getRealPath("/");
    
    String filePath = root + "resources/image/uploadHospitalImages/";
    
    // 1-3. 파일명 변환을 위한 클래스 작성 (Attachment.java)
    
    // 1-4. MultipartRequest 객체 생성 (객체 생성과 동시에 파라미터로 넘어온 내용 중 파일이 서버에 바로 저장됨)
    MultipartRequest multiRequest = new MultipartRequest(request, filePath, maxSize, "UTF-8", new MyFileRenamePolicy());
    
    
    // 2. 생성한 객체에서 파일 정보만을 얻어와 별도의 List에 모두 저장
    
    // 2-1. 파일 정보를 모두 저장할 List 객체 생성
    List<Attachment> fList = new ArrayList<Attachment>();
    
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
    String location1 = multiRequest.getParameter("location1"); //지역1
    
    String hospNm = multiRequest.getParameter("hospNm"); // 병원명
    
    String phone1 = multiRequest.getParameter("phone1"); // 전화번호
    String phone2 = multiRequest.getParameter("phone2"); // 전화번호
    String phone3 = multiRequest.getParameter("phone3"); // 전화번호
    String phone = phone1+"-" + phone2 + "-" + phone3; // 전화번호 합치기

    String location2 = multiRequest.getParameter("location2"); //상세주소
    
    
    String openTime = multiRequest.getParameter("openTime"); //오픈시간
    String closeTime = multiRequest.getParameter("closeTime"); //오픈시간
    
    String[] facilityArr = multiRequest.getParameterValues("hosp_facility"); // 병원시설 배열로 받기
    String facility = null;
    
    
    if(facilityArr!=null) { //병원 시설 배열이 비어있지 않다면.
        facility= String.join(",", facilityArr);
    }
    
    String hospitalInfo = multiRequest.getParameter("hospital_info");
    
    // 세션에서 로그인한 회원의 번호를 얻어옴
    Member loginMember = (Member)request.getSession().getAttribute("loginMember");
    int memberNo = loginMember.getMemberNo();
    
    // 얻어온 변수들을 모두 저장할 Map  생성
    Map<String,Object> map = new HashMap<String,Object>();
    
    map.put("fList",fList);
    map.put("location1", location1);
    map.put("hospNm", hospNm);
    map.put("phone", phone);
    map.put("location2", location2);
    map.put("openTime", openTime);
    map.put("closeTime", closeTime);
    map.put("facility", facility);
    map.put("hospitalInfo", hospitalInfo);
    map.put("memberNo", memberNo);
    
    
    // 4. 게시글 등록 비즈니스 로직 수행 후 결과 반환받기
    int result = service.insertHospital(map);
    
    if(result>0) {// DB에 데이터 등록 성공하면 result에 병원번호가 저장되어 있다.
        swalIcon = "success";
        swalTitle = "병원 등록 완료";
        path = "view?cp=1&hospitalNo="+result;
    } else {
        swalIcon = "error";
        swalTitle ="병원 등록 실패";
        path = "list";
    }
    
    request.getSession().setAttribute("swalIcon", swalIcon);
    request.getSession().setAttribute("swalTitle", swalTitle);
    response.sendRedirect(path);
    
}
```

<br><br>


- 동물병원 등록 Service

```java
	public int insertHospital(Map<String, Object> map) throws Exception {
		Connection conn = getConnection();
		
		int result =0;
		
		// 1. 동물병원 번호 얻어오기. (다음 번호 얻어오기임,, 추가되면 다음번호로 추가됨)
		int insertNo = dao.selectNextNo(conn);
		
		if(insertNo>0) {
			// 얻어온 번호를 map에 추가해준다.
			map.put("insertNo", insertNo);
			
			// 병원이름/병원소개 창에  크로스 사이트 스크립팅 방지처리..
			String hospNm = (String)map.get("hospNm");
			String hospitalInfo = (String)map.get("hospitalInfo");
			
			hospNm = replaceParameter(hospNm);
			hospitalInfo = replaceParameter(hospitalInfo);
			
			// 내용에 개행문자 변경처리.
			
			hospitalInfo = hospitalInfo.replaceAll("\r\n", "<br>");
			
			// 처리된 내용을 다시 map에 추가
			map.put("hospNm",hospNm);
			map.put("hospitalInfo",hospitalInfo);
			
			
			try {
				
				// 파일 정보 제외한 정보  등록하는 DAO호출
				result = dao.insertHospital(conn,map);
				
				// 파일 정보만 등록하는 DAO
				List<Attachment> fList = (List<Attachment>)map.get("fList");
				
				if(result>0 && !fList.isEmpty()) {
					// 게시글부분, 파일정보 모두 있다면.
					 result =0; // result를  재활용하기 위해 초기화시킴
					 
					 
					 // fList의 요소를 하나씩 반복접근하여 DAO메소드를 반복 호출해 정보를 삽입함.
					 for(Attachment at : fList) {
						 // 파일 정보가 저장된 Attachment 객체에 해당 파일이 작성된 게시글 번호를 추가 세팅
						 at.setHospNo(insertNo);
						 
						 result = dao.insertAttachment(conn,at); 
						 
						 if(result==0) { // 등록 실패.이미지 정보가 DB에 안들어갔을떄.
							 
							 // 강제로 예외를 발생시킨다. ( 사용자 정의 예외 )
							 throw new FileInsertFailedException("파일 정보 삽입 실패");
							 
						 }
						 
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
			
		} // end catch
		
		// 트랜잭션 처리
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

- 동물병원 등록 시 크로스 사이트 스크립팅 방지 메소드

```java
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

- 등록될 동물병원 번호 얻어오기 DAO

```java
public int selectNextNo(Connection conn) throws Exception {
    int hospitalNo =0;
    String query = prop.getProperty("selectNextNo");
    
    try {	
        stmt = conn.createStatement();
        rset = stmt.executeQuery(query);
        
        if(rset.next()) {
            hospitalNo = rset.getInt(1);
        }
        
    }finally {
        close(rset);
        close(stmt);
    }
    return hospitalNo;
}
```

<br><br>

- 동물병원 번호 얻어오는 sql문

```sql
<entry key="selectNextNo">
SELECT SEQ_HOSPITAL.NEXTVAL FROM DUAL
</entry>
```

<br><br>

- 동물병원 정보 등록하는 DAO

```java
public int insertHospital(Connection conn, Map<String, Object> map) throws Exception {
    int result =0;
    
    String query = prop.getProperty("insertHospital");
    
    try {
        
        pstmt = conn.prepareStatement(query);
        pstmt.setInt(1,(int)map.get("insertNo"));
        pstmt.setString(2,(String)map.get("hospNm"));
        pstmt.setString(3,(String)map.get("location1"));
        pstmt.setString(4,(String)map.get("location2"));
        pstmt.setString(5,(String)map.get("phone"));
        pstmt.setString(6,(String)map.get("openTime"));
        pstmt.setString(7,(String)map.get("closeTime"));
        pstmt.setString(8,(String)map.get("facility"));
        pstmt.setString(9,(String)map.get("hospitalInfo"));
        pstmt.setInt(10,(int)map.get("memberNo"));
        
        result = pstmt.executeUpdate();
    
    }finally {
        close(pstmt);
    }
    
    return result;
}
```

<br><br>

- 동물병원 정보 등록 sql 문

```sql
<entry key="insertHospital">
INSERT INTO HOSPITAL
VALUES(?, ?, ?, ?, ?, ?, ?, ?, ?, DEFAULT, DEFAULT, ?)
</entry>
```

<br><br>

- 첨부 파일 등록 DAO

```java
public int insertAttachment(Connection conn, Attachment at) throws Exception {
    int result =0;
    
    String query = prop.getProperty("insertAttachment");
    
    try {
        pstmt = conn.prepareStatement(query);
        pstmt.setString(1, at.getFilePath());
        pstmt.setString(2, at.getFileName());
        pstmt.setInt(3, at.getFileLevel());
        pstmt.setInt(4, at.getHospNo());
        
        result = pstmt.executeUpdate();
    }finally {
        close(pstmt);
    }
    return result;
}
```

<br><br>

- 첨부파일 등록 sql 문

```sql
<entry key="insertAttachment">
INSERT INTO HOSPITAL_IMG
VALUES(SEQ_HOSIMG.NEXTVAL, ?, ?, ?, ?)
</entry>
```
<br><br>
