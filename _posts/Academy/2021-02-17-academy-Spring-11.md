---
title: "2020년 02월 17일"
excerpt: "Spring 9 (SummerNote이용 게시글 등록)"
search: true
categories: 
  - Academy
tags: 
  - FrameWork
  - Java
  - Spring
  - SummerNote
toc: true
---


## SummerNote
- Summer note는 글쓰는 것을 효율적으로 하기위한 API
- API는 이미 만들어진 것을 원하는 형태로 가공해서 사용하는것
<br>

1. 홈페이지 예제를 보면서 어떻게 사용하는지 분석해서 활용하기
2. 글자 크기, 색, 폰트, 이미지, 동영상이 어떻게 첨부되는지 원리 파악하기 -> 글을 작성하거나, 이미지 등을 업로드하면  HTML 코드로 번역돼서 작성됨.-> DB저장 시 HTML 코드를 유지해야 한다.-> 섬머노트 사용해서 저장할 때는 크로스 사이팅 스크립팅 처리를 하지 않음. 
<br>

1. https://summernote.org/ ->  getting started -> js,css 다운로드->압축풀기
2. Deployed Resources -> webapp -> resources -> summernote 폴더 생성
- 생성된 폴더에 섬머노트 드래그 앤 드롭해서 넣어주기.<br>
>font폴더 -> 섬머노트에서 제공하는 폰트들이 모여있음<br>
>summernote.css/js -> 섬머노트에 관련된 모든 파일이 있음 <br>
>summernote-lite.css/js -> 중요한 것만 압축되어있음.<br>
>summernote-lite.js.map<br>
>lang폴더 -> summernote-ko-KR  -> 섬머노트 한글버전으로 바꾸기.<br>

<br>

### 섬머노트 활용하기 위한 코드 작성 

```js
// mySummernote.js

$(document).ready(function() {
    $('#summernote').summernote({
        width : 1000,
        height : 600,
        lang : 'ko-KR', // 언어 : 한국어

        // 이미지 업로드 이벤트가 발생했을 때 callback 함수 수행
        callbacks : {
            onImageUpload : function(files){
                // files : 업로드된 이미지가 배열로 저장되어 있음.
                // editor==this :  이미지가 업로드된 섬머노트 에디터.
                sendFile(files[0], this);
            }
        }
    });
  });

// 섬머노트에 업로드된 이미지를 비동기로 서버로 전송하여 저장하는 함수
function sendFile(file,editor){
    var formData = new FormData();
    // FormData : form 태그 내부 값 전송을 위한 객체로 
    // 추가된 값을 k=v 형태로 쉽게 생성해주는 객체
    // 알아서 쿼리 스트링을 만들어준다.

    formData.append("uploadFile", file);

    $.ajax({
        url : "insertImage", // spring/board/2/insertImage
        type : "post",
        enctype : "multipart/form-data", // 파일 전송 형식으로 인코딩 지정
        data : formData, // {"uploadFile": file}
        contentType : false, // 서버로 전성되는 데이터 형식
        // 기본값 : application/x-www-form-urlencoded; charset=utf-8(텍스트를 의미함)
        // false : 데이터 형식을 바이트 코드 있는 그대로. 
        cache : false,
        // 캐시메모리(메모리에 미리 저장) false = 메모리에 저장안하고 바로 서버로 전송
        processData : false,
        // 서버로 전달되는 값을 쿼리스트링으로 전달할 경우 true, 아니면 false
        // 쿼리스트링으로 전달 -> 주소에 담아서 데이터를 전송할 지
        dataType : "json",
        success : function(at){ //at == 파일 정보(객체), fileName, filePath 담겨있음.
            // console.log(at);
            
            // 자바스크립트를 이용한 contextPath 얻어오는 방법
            var contextPath = location.pathname.substring(0, window.location.pathname.indexOf("/",2));
            // localhost:8080/spring

            // 저장된 이미지를 summernote 에디터에 반영(삽입)
            $(editor).summernote('editor.insertImage', contextPath  + at.filePath + "/" + at.fileName);



        }
    });

}
```

<br><br>


### 글 등록 코드 수정하기

- Summernote가 적용될 페이지

```java
// boardInsert2.jsp


<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>게시글 등록</title>
<style>
    .insert-label {
      display: inline-block;
      width: 80px;
      line-height: 40px
    }
</style>


<!-- summernote 사용 시 필요한 css 파일 추가  -->
<link rel="stylesheet" href="${contextPath }/resources/summernote/css/summernote-lite.css">

</head>
<body>
	<jsp:include page="../common/header.jsp"/>
	
	<!-- summernote 사용 시 필요한 js 파일 추가  -->
	<script src="${contextPath }/resources/summernote/js/summernote-lite.js"></script> <!-- 이 코드가 있어야 섬머노트 사용 가능  -->
	<script src="${contextPath }/resources/summernote/js/summernote-ko-KR.js"></script> <!-- 한글 패치  -->
	<script src="${contextPath }/resources/summernote/js/mySummernote.js"></script> <!-- 개인이 만든 js  -->
	
<div class="container">

<div class="container pb-5 mb-5">

    <h3>게시글 등록</h3>
    <hr>
    <!-- 파일 업로드 시 enctype="multipart/form-data" 지정 -->
    <!-- 
        - enctype : form 태그 데이터가 서버로 제출 될 때 인코딩 되는 방법을 지정. (POST 방식일 때만 사용 가능)
        - application/x-www-form-urlencoded : 모든 문자를 서버로 전송하기 전에 인코딩 (form태그 기본값)
        - multipart/form-data : 모든 문자를 인코딩 하지 않음.(원본 데이터가 유지되어 이미지, 파일등을 서버로 전송 할 수 있음.) 
    -->
    <form action="insertAction" enctype="multipart/form-data" method="post" role="form" onsubmit="return validate();">
        <div class="mb-2">
            <label class="input-group-addon mr-3 insert-label">카테고리</label> 
            <select	class="custom-select" id="category" name="categoryName" style="width: 150px;">
                <option value="10">운동</option>
                <option value="20">영화</option>
                <option value="30">음악</option>
                <option value="40">요리</option>
                <option value="50">게임</option>
                <option value="60">기타</option>
            </select>
        </div>
        <div class="form-inline mb-2">
            <label class="input-group-addon mr-3 insert-label">제목</label> 
            <input type="text" class="form-control" id="title" name="boardTitle" size="70">
        </div>

        <div class="form-inline mb-2">
            <label class="input-group-addon mr-3 insert-label">작성자</label>
            <h5 class="my-0" id="writer">${loginMember.memberId}</h5>
        </div>


        <div class="form-inline mb-2">
            <label class="input-group-addon mr-3 insert-label">작성일</label>
            <h5 class="my-0" id="today">
                <!--java.util.Date를 이용해서 객체를 하나 만들어 now라고 부른다.  now의 출력 형식은 yyyy-MM-dd로  설정한다.  -->
                <jsp:useBean id="now" class="java.util.Date" />
                <fmt:formatDate value="${now}" pattern="yyyy-MM-dd"/>
            </h5>
        </div>  

        <hr>

        <div class="form-inline mb-2">
            <label class="input-group-addon mr-3 insert-label">썸네일</label>
            <div class="boardImg" id="titleImgArea">
                <img id="titleImg" width="200" height="200">
            </div>
        </div>



        <!-- 파일 업로드 하는 부분 -->
        <div id="fileArea">
            <input type="file" id="img0" name="images" onchange="LoadImg(this,0)"> 
        </div>


        <div class="form-group">
            <div>
                <label for="content">내용</label>
            </div>
            <textarea class="form-control" id="summernote" name="boardContent"
                rows="10" style="resize: none;"></textarea>
        </div>


        <hr class="mb-4">

        <div class="text-center">
            <button type="submit" class="btn btn-success">등록</button>
            <a class="btn btn-success float-right" href="${sessionScope.returnListURL}">목록으로</a>
        </div>

    </form>
</div>

</div>
<jsp:include page="../common/footer.jsp"/>

<script>

// 유효성 검사
function validate() {
    if ($("#title").val().trim().length == 0) {
        alert("제목을 입력해 주세요.");
        $("#title").focus();
        return false;
    }

    if ($("#content").val().trim().length == 0) {
        alert("내용을 입력해 주세요.");
        $("#content").focus();
        return false;
    }
}

// 이미지 영역을 클릭할 때 파일 첨부 창이 뜨도록 설정하는 함수
$(function(){
    $("#fileArea").hide(); // #fileArea 요소를 숨김.		
    
    $(".boardImg").on("click", function(){ // 이미지 영역이 클릭 되었을 때
        
        // 클릭한 이미지 영역 인덱스 얻어오기
        var index = $(".boardImg").index(this);
                // -> 클릭된 요소가 .boardImg 중 몇번째 인덱스인지 반환
                
        //console.log(index);
        
        // 클릭된 영역 인덱스에 맞는 input file 태그 클릭
        $("#img" + index).click();
    });
    
});
    


// 각각의 영역에 파일을 첨부 했을 경우 미리 보기가 가능하도록 하는 함수
function LoadImg(value, num) {
    // value.files : 파일이 업로드되어 있으면 true
    // value.files[0] : 여러 파일 중 첫번째 파일이 업로드 되어 있으면 true
    
    if(value.files && value.files[0]){ // 해당 요소에 업로드된 파일이 있을 경우
        
        var reader = new FileReader();
// 자바스크립트 FileReader
// 웹 애플리케이션이 비동기적으로 데이터를 읽기 위하여 
// 읽을 파일을 가리키는 File 혹은 Blob객체를 이용해 
// 파일의 내용을 읽고 사용자의 컴퓨터에 저장하는 것을 가능하게 해주는 객체

reader.readAsDataURL(value.files[0]);
// FileReader.readAsDataURL()
// 지정된의 내용을 읽기 시작합니다. 
// Blob완료되면 result속성 data:에 파일 데이터를 나타내는 URL이 포함 됩니다.	

reader.onload = function(e){
    // FileReader.onload
            // load 이벤트의 핸들러. 
            // 이 이벤트는 읽기 동작이 성공적으로 완료 되었을 때마다 발생합니다.	
    
            // 읽어들인 내용(이미지 파일)을 화면에 출력
            
            $(".boardImg").eq(num).children("img").attr("src", e.target.result);
            // e.target.result : 파일 읽기 동작을 성공한 요소가 읽어들인 파일 내용
            
}
    }
}
		
	</script>
</body>
</html>

```
<br><br>


- summernote에 업로드된 이미지 저장 Controller

```java
// BoardController.java

// summernote에 업로드된 이미지 저장 Controller
@ResponseBody
@RequestMapping("{type}/insertImage")
public String insertImage(HttpServletRequest request, 
                            @RequestParam("uploadFile") MultipartFile uploadFile) {
    // 실제 파일 경로(파일을 어디에 저장할지) 지정할 때 사용 -> HttpServletRequest request
    // 비동기로 파일 저장 == 저장하려는 이미지 얻어옴  
    // ->@RequestParam("uploadFile") MultipartFile uploadFile (uploadFile->파일자체를 담고있음 )
    
    
    // 서버에 파일(이미지)를 저장할 폴더 경로 얻어오기
    String savePath = request.getSession().getServletContext().getRealPath("resources/infoImages");
    
    Attachment at = service.insertImage(uploadFile,savePath);
    // 어느 위치에, 어떤 파일을 저장하겠다는 service 수행
    
    
    // java에서 js로 객체 전달할 때 JSON을 사용한다.
    // 문자열이지만 자바스크립트 객체 모양 "{}"을 하고있다.
    // gson을 사용함.
    return new Gson().toJson(at);
}
```
<br><br>

- summernote 업로드 이미지 저장 Service 구현


```java
// BoardServiceImpl.java

// summernote 업로드 이미지 저장 Service 구현
@Override
public Attachment insertImage(MultipartFile uploadFile, String savePath) {
    
    // 중복을 방지하기 위해, 파일명 변경해 줌
    String fileName = rename(uploadFile.getOriginalFilename());

    // 웹 상 접근 주소
    String filePath = "/resources/infoImages";
    
    // 돌려 보내줄 파일 정보를 Attachment 객체에 담아서 전달.
    Attachment at = new Attachment();
    at.setFilePath(filePath);
    at.setFileName(fileName);
    
    // 서버에 파일 저장(transferTo())
    
    try {
        uploadFile.transferTo(new File(savePath + "/" + fileName));
        // savePath : ~~~~/infoImages   fileName = 20210217~~.png
    }catch(Exception e) {
        e.printStackTrace();
        
        throw new InsertAttachmentFailException("summernote 파일 업로드 실패");
    }
    
    return at;
}
```



- 게시글 등록 Controller 수정

```java
// BoardController.java
// 게시글 등록Controller
@RequestMapping("{type}/insertAction")
public String insertAction(@PathVariable("type") int type, 
                        @ModelAttribute Board board, 
                        @ModelAttribute("loginMember") Member loginMember,
                        @RequestParam(value="images", required=false) List<MultipartFile> images,
                        HttpServletRequest request, RedirectAttributes ra) {
                        // @ModelAttribute Board board == categoryName,boardTitle, boardContent 가져옴
                        // @RequestParam(value="images", required=false) List<MultipartFile> images
                        // -> <input type="file" name="images"> 태그를 모두 얻어와 images라는  List에 매핑
    
    //System.out.println("type:" + type);
    //System.out.println("board:" + board);
    //System.out.println("loginMember:" + loginMember);
    
    // map을 이용하여 필요한 데이터 모두 담기
    Map<String,Object> map = new HashMap<String,Object>();
    map.put("memberNo", loginMember.getMemberNo());
    map.put("boardTitle", board.getBoardTitle());
    map.put("boardContent", board.getBoardContent());
    map.put("categoryCode",board.getCategoryName()); // name 은 categoryName 이지만 value는 코드로 되어있다.
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
    String savePath = null;
    
    if(type == 1) {
        savePath = request.getSession().getServletContext().getRealPath("resources/uploadImages");
    }else {
        savePath = request.getSession().getServletContext().getRealPath("resources/infoImages");
    }
    // System.out.println(savePath);
            
    // 게시글 map, 이미지 images, 저장경로 savePath
    // 게시글 삽입 Service 호출
    int result = service.insertBoard(map, images, savePath);
    
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




- 게시글 등록 ServiceImpl 수정

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
            String filePath = null;
            
            if((int)map.get("boardType") ==1) {
                filePath = "/resources/uploadImages";
            }else {
                filePath ="/resources/infoImages";
            }
            
            
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
            
            
            
            //-----------------------------summernote----------------------------
            // 게시판 타입이 2번(summernote를 이용한 게시글 작성)일 경우
            // boardContent 내부에 업로드된 이미지 정보 (filePath, fileName)이 들어있음
            // -> boardContent에서 <img> 태그만을 골라내어 img 태그의 src 속성값을 추출 후 filePath, fileName을 얻어냄.
            
            if((int)map.get("boardType") == 2) {
                Pattern pattern = Pattern.compile("<img[^>]*src=[\"']?([^>\"']+)[\"']?[^>]*>"); 
                //img 태그 src 추출 정규표현식
                
                // SummerNote에 작성된 내용 중 img태그의 src속성의 값을 검사하여 매칭되는 값을 Matcher객체에 저장함.
                Matcher matcher = pattern.matcher((String)map.get("boardContent"));     
                    
                String fileName = null; // 파일명 변환 후 저장할 임시 참조 변수
                String src = null; // src 속성값을 저장할 임시 참조 변수
                
                // matcher.find() : Matcher 객체에 저장된 값(검사를 통해 매칭된 src 속성 값)에 반복 접근하여 값이 있을 경우 true 
                while(matcher.find()){
                    src=  matcher.group(1); // 매칭된 src 속성값을  Matcher 객체에서 꺼내서 src에 저장 
                    
                    filePath = src.substring(src.indexOf("/", 2), src.lastIndexOf("/")); 
                    // 파일명을 제외한 경로만 별도로 저장.--> 2번째 / 부터   마지막 / 까지
                    
                    fileName = src.substring(src.lastIndexOf("/")+ 1); // 업로드된 파일명만 잘라서 별도로 저장.
                    
                    // Attachment 객체를 이용하여 DB에 파일 정보를 저장
                    Attachment at = new Attachment(filePath, fileName, 1, boardNo);
                    // 파일 레벨 숫자는 상관없음,
                    
                    uploadImages.add(at);
                }
            }
            
            //-----------------------------summernote----------------------------
            
            
            if(!uploadImages.isEmpty()) {
                // 파일 정보 삽입 DAO 호출
                result = dao.insertAttachmentList(uploadImages);
                // result == 삽입된 행의 개수
                
                if(result == uploadImages.size()) {
                    
                    result = boardNo;
                    // 반환 될 result 에 boardNo 저장. (상세정보 페이지로 넘어가기 위해서)
                    
                    // MultipartFile.transferTo()
                    // -> MultipartFile 객체에 저장된 파일을 지정된 경로에 실제 파일의 형태로 변h환하여 저장하는 메소드.
                    
                    int size = 0;
                    
                    if((int)map.get("boardType")==1) {
                        size = uploadImages.size();
                    }else if (!images.get(0).getOriginalFilename().equals("")){
                        size = images.size(); //==1
                    }
                    
                    for(int i=0; i<size; i++) {
                    // uploadImages라는  List에는 Attachment가 담겨져 있다. (파일이 업로드된 개수만큼 담겨져있으면서,파일 레벨이 지정되어 있음)
                    // uploadImages.get(i).getFileLevel() = i에 따라서 파일 레벨이 나온다.
                    // images는  input type="file" 태그 정보를 담은 , 실제 파일이 업로드 된 MultipartFile 들이 모여있는 List
                    // savePath : 실제 컴퓨터의 서버 경로
                        
                    // uploadImages를 만들 때 각 요소의 파일 레벨은 images의 index를 이용하여 부여한다.--> images의 index를 통해 uploadImages의 레벨을 구할 수 있다.
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
                
            }else { // 업로드된 이미지가 없을 경우
                result = boardNo;
            }
            
        }
    }
    return result;
}
```

<br><br>
<br><br>