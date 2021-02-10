---
title: "2020년 02월 10일"
excerpt: "Spring 8 (게시글 수정-미완성-)"
search: true
categories: 
  - Academy
tags: 
  - FrameWork
  - Java
  - Spring
toc: true
---

## 게시글 수정

### 게시글 수정 버튼

```java
// boardView.jsp
<c:url var="updateUrl" value="${board.boardNo}/update"/>
<!-- 로그인된 회원이 글 작성자인 경우 -->
        <c:if test="${(loginMember != null) && (board.memberId == loginMember.memberId)}">
            <a href="${updateUrl}" class="btn btn-success ml-1 mr-1">수정</a>
            <button id="deleteBtn" class="btn btn-success">삭제</button> 
        </c:if>
```

<br><br>


### 게시글 수정 화면 전환

```java
// BoardController.java

// @PathVarilable , QueryString 각각 언제 써야 되는가?
// @PathVarilable : 식별 용도로 사용한다.
// QueryString(주소 마지막에 k=v 형태로 파라미터를 전달하는 문자열) : 필터링, 정렬 등에 사용한다. 
// 게시글 수정 화면 전환용 Controller
@RequestMapping("{type}/{boardNo}/update")
public String update(@PathVariable("type") int type, @PathVariable("boardNo") int boardNo, Model model) {
        //  @PathVariable("boardNo") int boardNo -> @PathVariable int boardNo
        //	("boardNo")를 삭제하면 경로상에서 지정된 이름이  변수명과 같은 것을 매핑해줌.

// 1) 게시글 상세 조회
Board board = service.selectBoard(boardNo, type);

// 2) 해당 게시글에 포함된 이미지 목록 조회
if(board!=null) {
    
    List<Attachment> attachmentList = service.selectAttachmentList(boardNo);
    
    model.addAttribute("attachmentList", attachmentList);
    // null 값이 전달되어도 EL이 빈 문자열로 처리해 줌
}

model.addAttribute("board", board);

return "board/boardUpdate";
}

```

<br><br>

```html
//boardUpdate.jsp

<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>	
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>	

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>게시글 수정</title>
<style>
    .insert-label {
      display: inline-block;
      width: 80px;
      line-height: 40px
    }
    
    #content-main{ margin: 100px auto;}
    
    .deleteImg{
	    position: absolute;
	    display : inline-block;
	    margin-left: -15px;
	    border: none;
	    background-color: rgba(1,1,1,0);
	    width: 20px;
	    height: 20px;
	    cursor: pointer;
    }
</style>
</head>
<body>
<div class="container">
<jsp:include page="../common/header.jsp"/>

<div class="container pb-5 mb-5" id="content-main">

<h3>게시글 수정</h3>
<hr>
<form action="updateAction" method="post" enctype="multipart/form-data" name="updateForm" role="form" onsubmit="return validate();">
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
        <input type="text" class="form-control" id="title" name="boardTitle" size="70"
            value="${board.boardTitle }">
    </div>

    <div class="form-inline mb-2">
        <label class="input-group-addon mr-3 insert-label">작성자</label>
        <h5 class="my-0" id="writer">${loginMember.memberId}</h5>
    </div>


    <div class="form-inline mb-2">
        <label class="input-group-addon mr-3 insert-label">작성일</label>
        <h5 class="my-0" id="today">
            <jsp:useBean id="now" class="java.util.Date" />
            <fmt:formatDate value="${now}" pattern="yyyy-MM-dd"/>
        </h5>
    </div>

    <hr>
    
    
    <div class="form-inline mb-2">
        <label class="input-group-addon mr-3 insert-label">썸네일</label>
        <div class="mx-2 boardImg" id="titleImgArea">
            <img id="titleImg" width="200" height="200"> 
            <span class="deleteImg">x</span>
        </div>
    </div>

    <div class="form-inline mb-2">
        <label class="input-group-addon mr-3 insert-label">업로드<br>이미지</label>
        <div class="mx-2 boardImg" id="contentImgArea1">
            <img id="contentImg1" width="150" height="150">
            <span class="deleteImg">x</span>
        </div>

        <div class="mx-2 boardImg" id="contentImgArea2">
            <img id="contentImg2" width="150" height="150">
            <span class="deleteImg">x</span>
        </div>

        <div class="mx-2 boardImg" id="contentImgArea3">
            <img id="contentImg3" width="150" height="150">
            <span class="deleteImg">x</span>
        </div>
        
    </div>
    

    <!-- 파일 업로드 하는 부분 -->
    <div id="fileArea">
        <input type="file" id="img0" name="images" onchange="LoadImg(this,0)"> 
        <input type="file" id="img1" name="images" onchange="LoadImg(this,1)">
        <input type="file" id="img2" name="images" onchange="LoadImg(this,2)"> 
        <input type="file" id="img3" name="images" onchange="LoadImg(this,3)">
    </div>

    <div class="form-group">
        <div>
            <label for="content">내용</label>
        </div>
        <textarea class="form-control" id="content" name="boardContent"
            rows="10" style="resize: none;">${board.boardContent }</textarea>
    </div>


    <hr class="mb-4">

    <div class="text-center">
        <button type="submit" class="btn btn-primary">수정</button>
        <a class="btn btn-primary float-right" href="${header.referer}">목록으로</a>
    </div>

</form>
</div>

</div>
<jsp:include page="../common/footer.jsp"/>


<script>

// 게시글에 업로드 된 이미지 삭제
var deleteImages = [];
// 배열을 생성하여 이미지 삭제버튼 수 만큼 배열에 false 요소를 추가
// -> 배열에 4개의 false가 추가됨 == 인덱스 0~3 == fileLevel 0~3과 같음
// 이미지 삭제 버튼이 클릭 될 경우 해당 fileLevel과 같은 index 값을  true로 변경해서 
// 해당 이미지가 삭제되었음을 전달하기 위한 용도로 사용할 예정.

// deleteImages 배열에 false 4개 추가
for(var i=0; i<$(".deleteImg").length ; i++){
deleteImages.push(false);
}



// 카테고리 선택
$.each($("#category > option"), function(index, item){
if($(item).text() == "${board.categoryName}"){
    $(item).prop("selected", "true");
}
});

// 이미지 배치
<c:forEach var="at" items="${attachmentList}">
$(".boardImg").eq(${at.fileLevel}).children("img").attr("src", "${contextPath}${at.filePath}/${at.fileName}");
</c:forEach>


// 이미지 영역을 클릭할 때 파일 첨부 창이 뜨도록 설정하는 함수
$(function(){
$("#fileArea").hide(); // #fileArea 요소를 숨김.		

$(".boardImg").on("click", function(){ // 이미지 영역이 클릭 되었을 때
    var index = $(".boardImg").index(this);// 클릭한 이미지 영역 인덱스 얻어오기
    $("#img" + index).click(); // 클릭된 영역 인덱스에 맞는 input file 태그 클릭
});

});


// 각각의 영역에 파일을 첨부 했을 경우 미리 보기가 가능하도록 하는 함수
function LoadImg(value, num) {

if(value.files && value.files[0]){ // 해당 요소에 업로드된 파일이 있을 경우
    
    var reader = new FileReader();
    reader.readAsDataURL(value.files[0]);
    
    // reader.onload  == 파일 업로드에 성공했을 때
    // 미리보기
    reader.onload = function(e){ 
        $(".boardImg").eq(num).children("img").attr("src", e.target.result);
        
        // 특정 fileLevel에 이미지가 업로드된 경우
        // == deleteImages 배열에서 해당 fileLevel과 일치하는 인덱스의 값을 false로 바꿔 삭제되지 않음을 알려줌.
        deleteImages[num] = false;
    }
}
}


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

// 유효성 검사에서 문제가 없을 경우 서버에 제출 전
// deleteImages배열의 내용을 hidden 타입으로 하여 form태그 마지막에 추가하여 파라미터로 전달
for(var i=0 ; i<deleteImages.length ; i++){
$deleteImages = $("<input>", {type : "hidden", name : "deleteImages", value : deleteImages[i]});
$("form[name=updateForm]").append($deleteImages);
}


}




// 이미지 삭제 버튼 동작
$(".deleteImg").on("click",function(event){
// event : 현재 발생한 이벤트에 대한 정보가 담긴 객체
event.stopPropagation(); // 이벤트가 연달아 실행되는 것을 방지

// 기존 img태그를 삭제하고 새로운 img태그를 만들어서 제자리에 추가

// 기존 img태그
var $beforeImg = $(this).prev(); // 이벤트가 발생한 요소의 이전 요소 선택

// 기존 정보를 토대로 새로운 img 태그 생성
var $newImg =$("<img>", {id:$beforeImg.attr("id"), 
                            width : $beforeImg.css("width"), 
                            height : $beforeImg.css("height")});

$(this).prev().remove(); // 기존 img태그 삭제
$(this).before($newImg); // 새로운 img태그 추가

// 특정 fileLevel의 요소가 삭제 되었음을 알리기 위해 deleteImages에 기록.
// == deleteImages에 클릭된 삭제버튼 인덱스와 일치하는 true로 변경

// $(".deleteImg").index(this):
// deleteImg 클래스 중 현재 클릭된 버튼의 인덱스를 반환
deleteImages[$(".deleteImg").index(this)] = true;

console.log(deleteImages);

// 삭제버튼이 클릭된 경우
// 해당 이미지와 연결된 input type="file" 태그의 값을 없앤다.
$("#img" + ($(".deleteImg").index(this) )).val("");

});
</script>
</body>
</html>
```

<br><br>

### 게시글 수정


```java

// boardController.java

// 게시글 수정 Controller
@RequestMapping("{type}/{boardNo}/updateAction")
public String updateAction(@PathVariable("boardNo") int boardNo,
        @ModelAttribute Board updateBoard,
        Model model, RedirectAttributes ra, HttpServletRequest request,
        @RequestParam("deleteImages") boolean[] deleteImages,
        @RequestParam(value="images", required=false) List<MultipartFile> images) {
// updateBoard = boardTitle, boardContent, categoryName이 넘어옴

/*
* System.out.println(Arrays.toString(deleteImages)); for(MultipartFile m :
* images) { System.out.println(m.getOriginalFilename()); }
*/

// boardNo를 updateBoard에 세팅
updateBoard.setBoardNo(boardNo);

// 파일 저장 경로 얻어오기
String savePath = request.getSession().getServletContext().getRealPath("resources/uploadImages");

// 파일 수정 Service 호출
int result = service.updateBoard(updateBoard,images,savePath,deleteImages);
        // updateBoard : .수정된 게시글의 정보
        // images: 새롭게 업데이트 되거나, 추가된 이미지 정보 (파일 정보 전체)
        // savePath: 파일들을 저장할  저장 경로
        // deleteImages: 삭제된 파일 레벨을 기록하고 있는 배열
String url = null;

if(result > 0) {
swalIcon = "success";
swalTitle = "게시글 수정 성공";
url = "redirect:../"+boardNo;
}else {
swalIcon = "error";
swalTitle = "게시글 수정 실패";
url = "redirect:" + request.getHeader("referer");
}

ra.addFlashAttribute("swalIcon", swalIcon);
ra.addFlashAttribute("swalTitle", swalTitle);

return url;
}
```

<br><br>

```java
//BoardService.java

/** 게시글 수정 Service
* @param updateBoard
* @param images
* @param savePath
* @param deleteImages
* @return result
*/
public abstract int updateBoard(Board updateBoard, List<MultipartFile> images, String savePath,
    boolean[] deleteImages);
```

<br><br>

```java
//BoardServiceImpl.java

// 게시글 수정 Service 구현
@Transactional(rollbackFor=Exception.class)
@Override
public int updateBoard(Board updateBoard, List<MultipartFile> images, String savePath, boolean[] deleteImages) {

// 1) 게시글(text) 수정
// 제목, 내용 크로스사이트 스크립팅 방지 처리
updateBoard.setBoardTitle(replaceParameter(updateBoard.getBoardTitle()));
updateBoard.setBoardContent(replaceParameter(updateBoard.getBoardContent()));

// 게시글 수정 DAO 호출
int result = dao.updateBoard(updateBoard);

if(result>0) {
    // 2) 이미지 수정
    
    // 수정 전 업로드된 파일 정보를 얻어옴.
    // -> 새롭게 삽입 또는 수정되는 파일과 비교하기 위함.
    List<Attachment> oldFiles = dao.selectAttachmentList(updateBoard.getBoardNo());
    
    // 새로 업로드된 파일 정보를 담을 리스트
    List<Attachment> uploadImages = new ArrayList<Attachment>();
    // 삭제 되어야할 파일 정보를 담을 리스트
    List<Attachment> removeFileList = new ArrayList<Attachment>();
    
    // DB에 저장할 웹 상 이미지 접근 경로
    String filePath = "/resources/uploadImages";
    
    // 새롭게 업로드된 파일 정보를 가지고 있는 images에 반복 접근
    for(int i=0; i<images.size(); i++) {
        if(!images.get(i).getOriginalFilename().equals("")) { // 업로드된 이미지가 있을 경우
            
            // 파일명 변경
            String fileName = rename(images.get(i).getOriginalFilename());
            
            // Attachment 객체 생성
            Attachment at = new Attachment(filePath, fileName, i, updateBoard.getBoardNo());
            
            uploadImages.add(at); // 업로드 이미지 리스트에 추가
            
            // true : update 진행
            // false : insert 진행
            boolean flag = false;
            
            // 새로운 파일 정보와 이전 파일 정보를 비교하는 반복문
            for(Attachment old : oldFiles) {
                
            if(old.getFileLevel()==i) {
                // 현재 접근한 이전 파일의 레벨이 새롭게 업로드한 파일의 레벨과 같은 경우
                // -> 같은 레벨에 새로운 이미지 업로드된 경우 --> update
                flag = true;
                
                // DB에서 파일 번호가 일치하는 행의 내용을 수정하기 위해 파일번호를 얻어옴.
                at.setFileNo(old.getFileNo());
                removeFileList.add(old); // 삭제할 파일 목록에 이전 파일 정보 추가
                }
            }
            
            // flag 값에 따른 insert / update 제어
            if(flag) { // true : update 진행
                result = dao.updateAttachment(at);
            }else { // false : insert 진행
                result = dao.insertAttachment(at);
            }
            
            
            // insert 또는 update 실패 시 Rollback 수행
            // -> 예외를 발생시켜서 @Transactional을 이용해 수행
            if(result<=0) {
                throw new UpdateAttachmentFailException("파일 정보 수정 실패");
            }
            
        }else { // 업로드된 이미지가 없을 경우
            
        }
        
    } // images 반복 접근 for문 종료
    
    // uploadImages == 업로드된 파일 정보 --> 서버에 파일 저장
    // removeFileList == 제거해야될 파일 정보 --> 서버에서 파일 삭제
    
    // 수정되거나 새롭게 삽입된 이미지를 서버에 저장하기 위해 transferTo() 수행
    if(result > 0) {
        for(int i=0 ; i<uploadImages.size(); i++) {
            
            try {
                images.get(uploadImages.get(i).getFileLevel())
                    .transferTo(new File(savePath + "/" + uploadImages.get(i).getFileName()) );                                             
            }catch (Exception e) {
                e.printStackTrace();
                throw new UpdateAttachmentFailException("파일 정보 수정 실패");
            }
        }
    }
    
    // ------------------------------------------
    // 이전 파일 서버에서 삭제하는 코드 
    for(Attachment removeFile : removeFileList) {
        File tmp = new File(savePath + "/" + removeFile.getFileName());
        tmp.delete();
    }
    // ------------------------------------------
}
return result;
}
```
<br><br>

```java
//BoardDAO.java

/** 게시글 수정 DAO
* @param updateBoard
* @return result
*/
public int updateBoard(Board updateBoard) {
return sqlSession.update("boardMapper.updateBoard", updateBoard);
}



/** 파일 정보 수정 DAO
* @param at
* @return result
*/
public int updateAttachment(Attachment at) {
return sqlSession.update("boardMapper.updateAttachment", at);
}


/** 파일 정보 삽입 DAO
* @param at
* @return result
*/
public int insertAttachment(Attachment at) {
return sqlSession.insert("boardMapper.insertAttachment", at);
}

```
<br><br>


```sql
/* board-mapper.xml */ 

<!-- 게시글 수정   -->
<update id="updateBoard" parameterType="Board">
UPDATE BOARD SET
BOARD_TITLE = #{boardTitle} ,
BOARD_CONTENT = #{boardContent},
CATEGORY_CD = #{categoryName},
BOARD_MODIFY_DT = SYSDATE
WHERE BOARD_NO = #{boardNo}
</update>




<!-- 파일 정보 수정 -->
<update id="updateAttachment" parameterType="Attachment">
    UPDATE ATTACHMENT SET
    FILE_PATH = #{filePath},
    FILE_NAME = #{fileName}
    WHERE FILE_NO = #{fileNo}
</update>

<!-- 파일 정보 삽입  -->
<insert id="insertAttachment" parameterType="Attachment">
INSERT INTO ATTACHMENT
VALUES(SEQ_FNO.NEXTVAL, #{filePath}, #{fileName}, #{fileLevel}, #{parentBoardNo})
</insert>
```

<br><br><br>

