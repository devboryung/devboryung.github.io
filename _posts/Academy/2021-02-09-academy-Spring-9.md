---
title: "2020년 02월 09일"
excerpt: "Spring 7 (자유게시판 게시글 등록)"
search: true
categories: 
  - Academy
tags: 
  - FrameWork
  - Java
  - Spring
toc: true
---

## 게시글 등록

- Spring에서는 파일 업로드를 위한 MultipartResolver라는 인터페이스를 제공하고 있다.<br>
- 실제 파일 업로드를 하기 위해서는 MultipartResolver 의 구현체가 필요하다. <br><br>
* MultipartResolver의 구현체를 생성하는 방법 2가지.<br>
1. Servlet 3.0이상 버전에서 MultipartRequest를 사용해 StandardServletMultipartResolver를 생성하는 방법.<br>
2. Apache Commons FileUpload 라이브러리를 이용해 CommonsMultipartResolver를 구현하는 방법.<br><br>
* StandardServletMultipartResolver,CommonsMultipartResolver 모두 스프링이 기본적으로 내장하고 있는 객체이며, MultipartResolver를 상속받은 객체이다. <br><br>


### 2번째 방법으로 파일 업로드 하기.

- Spring에서 라이브러리 추가하는 방법: <br>
1. Maven을 이용하여 commons-fileupload 라이브러리 추가하기 <br>
https://mvnrepository.com/ 접속 -> commons fileupload검색 ->  Apache Commons FileUpload클릭 -> 1.3.3클릭 -> Maven에 적혀있는 코드 복사 -> pom.xml에 dependencies태그 안에 붙여넣기 <br>
2. root-context.xml 파일에 CommonsMultipartResolver를 bean으로 등록하는 코드 추가<br><br>

```xml
<!--pom.xml-->
<!-- 파일 업로드를 위한 라이브러리  -->
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.3</version>
</dependency>
```
<br><br>

```xml
<!-- root-context.xml -->
 <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
   		<property name="maxUploadSize" value="104857600"/>
   		<property name="maxUploadSizePerFile" value="20971520"/>
   		<property name="maxInMemorySize" value="104857600"/>
</bean>
```

* MultipartResolver 클래스 bean 등록시 나타나는 효과 <br>
MultipartResolver를 bean으로 등록하면 multipart/form-data 형식의 요청을 받게 될 경우 
스프링 컨테이너가 해당 bean을 제어하여 input type="file" 태그를 변도로 얻어와서 처리하여 MultipartFile 객체로 반환해 준다.   <br>
input type="file"에 파일이 업로드가 됐든, 안됐든 무조건 얻어 옴. 태그 하나 또는 업로드된 파일 하나 당 MultipartFile 객체 하나로 반환해 준다.<br>
(파라미터로 받아올때 배열이나 리스트로 받아올 수 있다. 리스트로 가져오는게 편리하다.)<br>
MultipartResolver가 파일 관련된 내용(multipart/form-data)를 처리하므로 나머지 일반 텍스트 형태의 데이터를 별도 처리 없이 사용 가능.<br><br>

* CommonsMultipartResolver 클래스 bean 등록 시 사용하는 속성<br>
- maxUploadSize : 한 요청당 업로드가 허용되는 최대 용량(바이트 단위)(기본값 -1==무제한)<br>
- maxUploadSizePerFile : 한 파일당 업로드가 허용되는 최대 용량(바이트 단위)(기본값 -1==무제한)<br>
- maxInMemorySize : 파일을 디스크에 저장하지 않고 메모리에 유지하도록 허용하는 최대 용량(바이트단위), <br>지정된 용량보다 업로드 파일이 더 큰 경우 파일로 저장함.(기본값 10240byte)<br><br>


### 게시글 상세조회 Controller

- 파일 == 서버에 저장 된다.<br>
HttpServletRequest 객체가 있어야지만 파일 저장 경로를 얻어올 수 있다. <br>
HttpServletRequest는 Controller에서만 사용 가능.<br>

```java
// BoardController.java

// 게시글 등록Controller
@RequestMapping("{type}/insertAction")
public String insertAction(@PathVariable("type") int type, 
@ModelAttribute Board board, @ModelAttribute("loginMember") Member loginMember,
@RequestParam(value="images", required=false) List<MultipartFile> images,
HttpServletRequest request, RedirectAttributes ra) {
// @ModelAttribute Board board == categoryName,boardTitle, boardContent 가져옴
// @RequestParam(value="images", required=false) List<MultipartFile> images
// <input type="file" name="images"> 태그를 모두 얻어와 images라는  List에 매핑

//System.out.println("type:" + type);
//System.out.println("board:" + board);
//System.out.println("loginMember:" + loginMember);

// map을 이용하여 필요한 데이터 모두 담기
Map<String,Object> map = new HashMap<String,Object>();
map.put("memberNo", loginMember.getMemberNo());
map.put("boardTitle", board.getBoardTitle());
map.put("boardContent", board.getBoardContent());
map.put("categoryCode",board.getCategoryName()); 
// name 은 categoryName 이지만 value는 코드로 되어있다.
map.put("boardType",type);


// 파일 업로드 확인
/*  for(int i=0; i<images.size(); i++) {
System.out.println("images["+i+"] : "  + images.get(i).getOriginalFilename());
} */
// 파일이 업로드 되지 않은 부분도 출력되고 있음을 확인 함.
// == 모든 input type="file" 태그가 순서대로 넘어오고 있음을 확인.
// 넘어오는 순서를 file level로 사용 가능하다.


// 파일 저장 경로 설정
// HttpServletRequest 객체가 있어야지만 파일 저장 경로를 얻어올 수 있다.
// HttpServletRequest는 Controller에서만 사용 가능.
String savePath = request.getSession().getServletContext().getRealPath("resources/uploadImages");
// System.out.println(savePath);

// 게시글 map, 이미지 images, 저장경로 savePath
// 게시글 삽입 Service 호출
int result = service.insertBoard(map, images,savePath);

String url = null;

// 게시글 삽입 결과에 따른 View 연결 처리
if(result>0) {
swalIcon= "success";
swalTitle= "게시글 등록 성공";
url = "redirect:"+result;
// 새로 작성한 게시글 상세 조회 시 목록으로 버튼 경로 지정하기
request.getSession().setAttribute("returnListURL", "../list/"+type);
}else {
swalIcon="error";
swalTitle = "게시글 등록 실패";
url = "redirect:insert";
}

ra.addFlashAttribute("swalIcon",swalIcon);
ra.addFlashAttribute("swalTitle",swalTitle);

return url;
}
```
<br><br>

### 게시글 등록 Service

```java
// BoardService.java
/** 게시글 등록 + 파일 업로드 Service
* @param map
* @param savePath 
* @param images 
* @return
*/
public abstract int insertBoard(Map<String, Object> map, List<MultipartFile> images, String savePath);
```
<br><br>

### 게시글 등록 ServiceImpl

* Service에서 트랜잭션 처리를 하는 이유?<br>
service의 목적 비즈니스 로직 처리<br>
(데이터 가공, 필요한 여러 DAO 메소드 호출)<br>
수행된 여러 DAO 구문의 트랜잭션 내용을 한번에 DB에 Commit 또는 Rollback하기 위해 트랜잭션 처리는 Service에서 수행된다.<br>

```java
// BoardServiceImpl.java
// 게시글 등록 Service 구현
@Transactional(rollbackFor = Exception.class)
@Override
public int insertBoard(Map<String, Object> map, List<MultipartFile> images, String savePath) {
int result =0; // 최종 결과 저장 변수 선언

// 1) 게시글 번호 얻어오기 -> SEQ_BNO.NEXTVAL
int boardNo = dao.selectNextNo();

// 2) 게시글 삽입
if(boardNo>0) { // 다음 게시글 번호를 얻어온 경우
    map.put("boardNo", boardNo); // map에 boardNo 추가
    
    // 크로스 사이트 스크립팅 방지 처리
    // summernote API 사용 시 html, css 로 작성되기 때문에 크로스 사이트 스크립팅 방지 처리가 되면 안된다.
    // 게시판 타입별로 크로스 사이트 스크립팅 방지 처리를 선택적으로 진행.
    if( (int)map.get("boardType")==1) { //자유게시판
        String boardTitle = (String)map.get("boardTitle");
        String boardContent = (String)map.get("boardContent");
        
        // 크로스 사이트 스크립팅 방지 처리 적용
        boardTitle = replaceParameter(boardTitle);
        boardContent = replaceParameter(boardContent);
        
        // 처리된 문자열을 다시 map 에 세팅
        map.put("boardTitle",boardTitle);
        map.put("boardContent",boardContent);
        // 개행문자 처리 -> 화면에서 JSTL을 이용해 처리할 예정
    }
    
    // 게시글 삽입 DAO 메소드 호출
    result = dao.insertBoard(map);
    

    // 3) 게시글 삽입 성공 시 이미지 정보 삽입
    if(result>0) { // 게시글 삽입에 성공한다면 result에 글 번호 등록(상세 조회를 위해서)
        
        // 이미지 정보를 Attachment 객체에 저장하여 List에 추가
        List<Attachment> uploadImages = new ArrayList<Attachment>();
        
        // images.get(i).getOriginalFileName() 메소드를 수행하면 업로드된 파일의 원본 파일명이 출력된다.
        // --> 중복 상황을 대비하여 파일명 변경하는 코드 필요.(rename() 메소드)
        
        // DB에 저장할 웹 상 접근 주소(filePath)
        String filePath = "/resources/uploadImages";
        
        
        // for문을 이용하여 파일정보가 담긴 images를 반복 접근
        // -> 업로드된 파일이 있을 경우에만 uploadImages 리스트에 추가
        for(int i=0; i<images.size(); i++) {
            // i == 인덱스 == FileLevel과 같은 값
            
            // 현재 접근한 images의 요소(MultipartFile)에 업로드된 파일이 있는지 확인.
            if( !images.get(i).getOriginalFilename().equals("") ) { 
                // 파일이 업로드 된 경우 == 업로드된 원본 파일명이 있는 경우
                
                // 원본 파일명 변경
                String fileName = rename(images.get(i).getOriginalFilename());
                
                // Attachment 객체 생성
                Attachment at = new Attachment(filePath, fileName, i, boardNo);
                 // filePath, fileName, fileLever, parentBoardNo
                
                uploadImages.add(at); // 리스트에 추가
            }
        }
        
    // uploadImage 확인
    /* for(Attachment at : uploadImages) {
        System.out.println(at);
    } */
    
    if(!uploadImages.isEmpty()) {
        // 파일 정보 삽입 DAO 호출
        result = dao.insertAttachmentList(uploadImages);
        // result == 삽입된 행의 개수
        
        if(result == uploadImages.size()) {
            
            result = boardNo;
            // 반환 될 result 에 boardNo 저장. (상세정보 페이지로 넘어가기 위해서)
            
            // MultipartFile.transferTo()
            // -> MultipartFile 객체에 저장된 파일을 지정된 경로에 실제 파일의 형태로 변h환하여 저장하는 메소드.
            
            for(int i=0; i<uploadImages.size(); i++) {
            // uploadImages라는  List에는 Attachment가 담겨져 있다. 
            (파일이 업로드된 개수만큼 담겨져있으면서,파일 레벨이 지정되어 있음)
            // uploadImages.get(i).getFileLevel() = i에 따라서 파일 레벨이 나온다.
            // images는  input type="file" 태그 정보를 담은 , 실제 파일이 업로드 된 MultipartFile 들이 모여있는 List
            // savePath : 실제 컴퓨터의 서버 경로
                
            // uploadImages를 만들 때 각 요소의 파일 레벨은 images의 index를 이용하여 부여한다.
            // --> images의 index를 통해 uploadImages의 레벨을 구할 수 있다.
            // uploadImages.get(i).getFileName() : 업로드된 i번째 이미지의 변경된 이름을 가져온다.
            // 실제 파일들을 변경된  파일의 이름으로  새로운 경로에 저장한다.
                try {
                    images.get(uploadImages.get(i).getFileLevel()).transferTo(new File(savePath+"/"+uploadImages.get(i).getFileName()) );
                    
                }catch(Exception e) {
                    e.printStackTrace();
                    
        // transferTo()는 IOException(CheckedException)을 발생시킨다. 어쩔 수 없이 try catch를 작성해서 예외처리 함.
        // 예외가 처리되면 @Transactional이 정상 동작하지 못 함.
        // 꼭 예외처리를 하지 않아도 되는 UncheckedException을 강제 발생시켜 @Transactional이 예외가 발생했음을 감지하게 함.
        // 상황에 맞는 사용자 정의 예외를 작성함.
        throw new InsertAttachmentFailException("파일 서버 저장 실패");
      }
     }
    
    }else { // 파일 정보를 DB에 삽입 실패
        throw new InsertAttachmentFailException("파일 정보 DB 삽입 실패");
    }
   }
  }
}
return result;
}





// 크로스 사이트 스크립트 방지 처리 메소드
private String replaceParameter(String param) {
String result = param;
if(param != null) {
    result = result.replaceAll("&", "&amp;");
    result = result.replaceAll("<", "&lt;");
    result = result.replaceAll(">", "&gt;");
    result = result.replaceAll("\"", "&quot;");
}

return result;
}





// 파일명 변경 메소드
public String rename(String originFileName) {
SimpleDateFormat sdf = new SimpleDateFormat("yyMMddHHmmss");
String date = sdf.format(new java.util.Date(System.currentTimeMillis()));

int ranNum = (int)(Math.random()*100000); // 5자리 랜덤 숫자 생성

String str = "_" + String.format("%05d", ranNum);
//String.format : 문자열을 지정된 패턴의 형식으로 변경하는 메소드
// %05d : 오른쪽 정렬된 십진 정수(d) 5자리(5)형태로 변경. 빈자리는 0으로 채움(0)

String ext = originFileName.substring(originFileName.lastIndexOf("."));

return date + str + ext;
}

```
<br><br>

### 파일 정보 등록 DAO

```java
// BoardDao.java

/**  다음 게시글 번호 얻어오기 DAO
    * @return result
    */
public int selectNextNo() {
    return sqlSession.selectOne("boardMapper.selectNextNo");
}



/** 게시글 삽입 DAO
    * @param map
    * @return result
    */
public int insertBoard(Map<String, Object> map) {
    return sqlSession.insert("boardMapper.insertBoard",map);
}


/** 파일 정보 삽입 DAO
* @param uploadImages
* @return result(성공한 행의 개수)
*/
public int insertAttachmentList(List<Attachment> uploadImages) {
return sqlSession.insert("boardMapper.insertAttachmentList", uploadImages);
}
```
<br><br>

### 파일 정보 등록 mapper

* 파일 정보 삽입 sql문
- INSERT시 서브쿼리를 사용하면 서브쿼리의 결과 행 수 만큼 INSERT가 수행된다.
- 더미 테이블을 이용해서 원하는 형태의 결과를 1행 조회하는 구문을 만들 수 있다.
- UNION ALL을 이용하면 컬럼 구성이 같은 RESULT SET 을 하나로 합칠 수 있다.
- 서브쿼리를 이용한 INSERT시 시퀀스.NEXTVAL가 서브 쿼리에 포함되어 있다면 각 행이 삽입 될 때 마다 시퀀스 값이 증가함.

```sql
<!-- 다음 게시글 번호 얻어오기 -->
<select id="selectNextNo" resultType="_int">
SELECT SEQ_BNO.NEXTVAL FROM DUAL
</select>

<!-- 게시글 삽입  -->
<insert id="insertBoard" parameterType="map">
INSERT INTO BOARD
VALUES(#{boardNo}, #{boardTitle}, #{boardContent},
                DEFAULT, DEFAULT, DEFAULT, DEFAULT,
                #{memberNo}, #{categoryCode}, #{boardType} )
</insert>

<!-- 파일 정보 삽입(List이용)  -->
<insert id="insertAttachmentList" parameterType="list">
INSERT INTO ATTACHMENT
SELECT SEQ_FNO.NEXTVAL, A.* FROM (
    <foreach collection="list" item="item" separator="UNION ALL">
        SELECT #{item.filePath} FILE_PATH,
                        #{item.fileName} FILE_NAME,
                        #{item.fileLevel} FILE_LEVEL,
                        #{item.parentBoardNo} PARENT_BOARD_NO
        FROM DUAL
    </foreach>
)A
</insert>
```

<br><br>
<br><br>
