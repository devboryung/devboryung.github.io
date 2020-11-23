---
title: "2020년 11월 23일"
excerpt: "JDBC-6"
search: true
categories: 
  - Academy
tags: 
  - JAVA
  - JDBC
  - TIL
toc: true
---

## JDBC
### jdbc-5번의 게시글 상세조회 메소드에 댓글 기능 추가.

```java

//BoardView.java
private void selectBoard() {
		System.out.println("[게시글 상세 조회]");
		System.out.print("조회할 글 번호 : ");
		int boardNo = sc.nextInt();
		sc.nextLine();

		try {
			VBoard vboard = bService.selectBoard(boardNo);

			if(vboard==null) {
				System.out.println("해당 번호의 글이 존재하지 않습니다.");
			}else {
				int sel = 0;
				do {
					System.out.println("----------------------------------------------------------------------------");
					System.out.printf("%d | [%s] %s\n",vboard.getBoardNo(),  vboard.getCategoryNm(), vboard.getTitle());
					System.out.printf("작성자 : %s | 작성일  : %s | 조회수 %d\n",vboard.getMemNm(), vboard.getCreateDt(), vboard.getReadCount());
					System.out.println("----------------------------------------------------------------------------");
					System.out.println(vboard.getContent());
					System.out.println("----------------------------------------------------------------------------");

					// ******************* 댓글 기능 추가 ********************
					// CommentView 객체 생성
					CommentView commentView = new CommentView();

					// 1) 해당 게시글의 댓글 조회
					commentView.selectComment(boardNo);

					// 2) 댓글 삽입, 수정, 삭제
					System.out.println("------[댓글 메뉴]-----");
					System.out.println("1. 댓글 작성/ 2. 댓글 수정/ 3. 댓글 삭제 / 0. 이전으로");
					System.out.print("메뉴 선택 : ");
					sel = sc.nextInt();
					sc.nextLine();
					
					// 작성, 수정, 삭제에 모두 필요한 회원번호, 게시글 번호를 저장한
					// Comment 객체 생성
					Comment comment = new Comment();
					comment.setMemNo(JDBCView.loginMember.getMemNo());
					comment.setBoardNo(boardNo);
					
					switch(sel) {
					case 1: commentView.insertComment(comment); break; // 작성 (회원번호, 게시글번호, 댓글내용)
					case 2: // 수정 (게시글번호, 댓글내용 + 회원 번호)
						System.out.println("[댓글 수정]");
						commentView.updateComment(comment);
						
						break; 
						
					case 3: // 삭제 (게시글 번호 + 회원 번호)
						System.out.println("[댓글 삭제]");
						commentView.updateDeleteFl(comment);
						break; 
					case 0: System.out.println("게시판 메뉴로 돌아갑니다.");break;
					default : System.out.println("잘못 입력하셨습니다.");
					}
					// --> 코드 길이가 길어서  comment 클래스 만들어서 작성
				}while(sel!=0);
			}	

		}catch(Exception e) {
			System.out.println("게시글 상세 조회 중 오류 발생");
			e.printStackTrace();
		}
	}
```

```java
//Comment.java
package com.kh.jdbc.comment.model.vo;

import java.sql.Date;

public class Comment {
	
	private int commentNo;
	private String content;
	private Date createDt;
	private char deleteFl;
	private int memNo;
	private int boardNo;
	
	public Comment() {}

	public Comment(int commentNo, String content, Date createDt, char deleteFl, int memNo, int boardNo) {
		super();
		this.commentNo = commentNo;
		this.content = content;
		this.createDt = createDt;
		this.deleteFl = deleteFl;
		this.memNo = memNo;
		this.boardNo = boardNo;
	}

	public int getCommentNo() {
		return commentNo;
	}

	public void setCommentNo(int commentNo) {
		this.commentNo = commentNo;
	}

	public String getContent() {
		return content;
	}

	public void setContent(String content) {
		this.content = content;
	}

	public Date getCreateDt() {
		return createDt;
	}

	public void setCreateDt(Date createDt) {
		this.createDt = createDt;
	}

	public char getDeleteFl() {
		return deleteFl;
	}

	public void setDeleteFl(char deleteFl) {
		this.deleteFl = deleteFl;
	}

	public int getMemNo() {
		return memNo;
	}

	public void setMemNo(int memNo) {
		this.memNo = memNo;
	}

	public int getBoardNo() {
		return boardNo;
	}

	public void setBoardNo(int boardNo) {
		this.boardNo = boardNo;
	}

	@Override
	public String toString() {
		return "Comment [commentNo=" + commentNo + ", content=" + content + ", createDt=" + createDt + ", deleteFl="
				+ deleteFl + ", memNo=" + memNo + ", boardNo=" + boardNo + "]";
	}
	
	

}

```


```java
//CommentView.java
package com.kh.jdbc.comment.view;

import java.util.List;
import java.util.Map;
import java.util.Scanner;

import com.kh.jdbc.comment.model.service.CommentService;
import com.kh.jdbc.comment.model.vo.Comment;

public class CommentView {
	private Scanner sc = new Scanner(System.in);
	private CommentService cService = new CommentService();
	
	
	/** 해당 게시글 댓글 조회 View
	 * @param boardNo
	 */
	public void selectComment(int boardNo) {
		 System.out.println("[댓글]");
		 
		 try {
			 List<Map<String, Object>> list = cService.selectComment(boardNo);
			 
			 // 댓글 리스트 출력
			 for(Map<String, Object> map : list) {
				 // map에 저장된 요소들을 꺼내두기
				 Comment comment = (Comment)map.get("comment"); // 다운캐스팅
				 String memNm = (String)map.get("memNm"); // 다운캐스팅
				 
				 // %d : 10진 정수  / %f : 실수
				 // %s : 문자열(String)  / %c : 문자(char)
				 
				 // %10d : 오른쪽 정렬된 10칸짜리 정수
				 // %-10d : 왼쪽 정렬된 10칸짜리 정수
				 System.out.printf("%3d | %-30s | %5s | %s\n",
						       comment.getCommentNo(), comment.getContent(), 
						       memNm, comment.getCreateDt());
				 
				 
			 }
		 }catch (Exception e) {
			 System.out.println("댓글 조회 과정에서 오류 발생");
			 e.printStackTrace();
			 
		 }
		
	}


	/** 댓글 작성 View
	 * @param comment
	 */
	public void insertComment(Comment comment) {
		System.out.println("[댓글 작성]");
		String content = sc.nextLine();
		
		// comment 객체에 입력받은 content를 저장하여 comment 객체 하나로 데이터 전달
		comment.setContent(content);
		
		try {
			int result = cService.insertComment(comment);
			
			if(result > 0) System.out.println("댓글 작성 성공");
			else System.out.println("댓글 작성 실패"); 
			
		}catch (Exception e) {
			System.out.println("댓글 작성 과정에서 오류 발생");
			e.printStackTrace();
		}
		
	}


	/** 댓글 수정 View
	 * @param comment
	 */
	public void updateComment(Comment comment){
		// 댓글 번호를 입력받아 자신의 댓글이 맞는지 확인
		int commentNo = checkMyComment(comment);
		
		if(commentNo >0) { // 자신의 댓글이 맞다면
			comment.setCommentNo(commentNo);
			//자신의 댓글이라고 판명된 댓글 번호를 comment 객체에 세팅
			
			System.out.print("[댓글 수정] :");
			String content = sc.nextLine();
			
			comment.setContent(content);// 입력받은 수정 내용도 comment 객체에 세팅
			try {
				int result = cService.updateComment(comment);
				
				if(result>0) {
					System.out.println("댓글 수정이 되었습니다.");
				}else {
					System.out.println("댓글 수정이 실패하였습니다.");
				}
				
			}catch (Exception e) {
				System.out.println("댓글 수정 과정에서 오류 발생");
				e.printStackTrace();
			}
		}
		
	}



	/** 댓글 삭제 View
	 * @param comment
	 */
	public void updateDeleteFl(Comment comment) {
		// 댓글 번호를 입력받아 자신의 댓글이 맞는지 확인
		int commentNo = checkMyComment(comment);
		// 자기 댓글이 맞을 경우 "정말 삭제하시겠습니까? (Y/N) : "출력 후 Y/N 을 입력 받아 앎자은 서비스 수행
		
		if(commentNo>0) {
			comment.setCommentNo(commentNo);
			System.out.println("정말 삭제하시겠습니가? (Y/N) : ");
			char input = sc.nextLine().toUpperCase().charAt(0);
		try {
			
			if(input == 'Y') {
				int result = cService.updateDeleteFl(comment);
				
				if(result>0) {
					System.out.println("삭제 성공");
				}else {
					System.out.println("삭제 실패");
				}
				
			}else {
				System.out.println("삭제를 취소합니다.");
			}
		}catch (Exception e) {
			System.out.println("오류 발생");
			e.printStackTrace();
		}
			
			
		}else {
			System.out.println("자신의 댓글이 아닙니다."); 
		}
		
		
	}
	
	
	
	/** 자신의 댓글인지 확인하는 View
	 * @param comment
	 * @return result
	 */
	private int checkMyComment(Comment comment) {
								//comment -> 회원번호, 게시글 번호
		System.out.println("댓글 번호 입력 : ");
		int commentNo = sc.nextInt();
		sc.nextLine();
		
		// 게시글 번호, 회원번호가 저장 되어있는 comment 객체에 
		// 입력받은 댓글번호 commentNo를 세팅
		comment.setCommentNo(commentNo);
		
		int result = 0; // 자신의 댓글이 맞다면 댓글번호, 아니면 0을 반환
		
		try {
			result = cService.chekMyComment(comment);
			
			if(result>0) { //자신의 댓글이 맞는 경우
				System.out.println("(확인완료)");
				result = commentNo; // 댓글 번호
			}else {
				System.out.println("자신의 댓글이 아니거나 없는 댓글 번호입니다.");
			}
				
		}catch (Exception e) {
			System.out.println("댓글 확인 과정에서 오류 발생.");
			e.printStackTrace();
		}
		return result;
	}

}

```


```java
//CommentService
package com.kh.jdbc.comment.model.service;

import com.kh.jdbc.comment.model.dao.CommentDAO;
import com.kh.jdbc.comment.model.vo.Comment;

import static com.kh.jdbc.common.JDBCTemplate.*;

import java.sql.Connection;
import java.util.List;
import java.util.Map;


public class CommentService {
	
	private CommentDAO cDAO = new CommentDAO();
	
	
	/** 댓글 조회 Service
	 * @param boardNo
	 * @return list
	 */
	public List<Map<String, Object>> selectComment(int boardNo)throws Exception{
		Connection conn = getConnection();
		
		List<Map<String,Object>> list = cDAO.selectComment(conn, boardNo);
		
		// select만 했을 경우 트랜잭션 처리 필요없음
		close(conn);
		
		return list;
	}


	/** 댓글 작성 Service
	 * @param content
	 * @return result
	 * @throws Exception
	 */
	public int insertComment(Comment comment) throws Exception {
		Connection conn = getConnection();
		
		int result = cDAO.insertComment(conn, comment);
		
		// 트랜잭션 처리
		if(result >0) 		commit(conn);
		else	rollback(conn);
		
		close(conn);
		
		return result;
	}



	/** 댓글 수정 Service
	 * @param comment
	 * @return result
	 * @throws Exception
	 */
	public int updateComment(Comment comment) throws Exception {
		Connection conn = getConnection();
		int result = cDAO.updateComment(conn,comment);
		
		if (result>0)  commit(conn);
		else	rollback(conn);
		
		close(conn);
		
		return result;
	}

	/** 댓글 삭제 Service
	 * @param comment
	 * @return result
	 * @throws Exception
	 */
	public int updateDeleteFl(Comment comment) throws Exception {
		Connection conn = getConnection();
		int result = cDAO.updateDeleteFl(conn, comment);
		
		if (result>0) commit(conn);
		else rollback(conn);
		
		close(conn);
		return result;
	}

	/** 댓글 확인 Service
	 * @param comment
	 * @return result
	 * @throws Exception
	 */
	public int chekMyComment(Comment comment) throws Exception {
		Connection conn = getConnection();
		
		int result = cDAO.chekMyComment(conn, comment);
		
		close(conn);
		
		return result;
	}

	

}

```

```java
//CommentDAO.java
package com.kh.jdbc.comment.model.dao;

import static com.kh.jdbc.common.JDBCTemplate.*;

import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Properties;

import com.kh.jdbc.comment.model.vo.Comment;

public class CommentDAO {
	
	private Statement stmt = null;
	private PreparedStatement pstmt = null;
	private ResultSet rset = null;
	
	private Properties prop = null;
	
	
	// 기본 생성자. 외부 XML파일 읽어오기
	public CommentDAO() {
		try {
			prop = new Properties();
			prop.loadFromXML(new FileInputStream("comment-query.xml"));
		}catch (Exception e) {
			e.printStackTrace();
		}
	}


	/** 댓글 조회 DAO
	 * @param conn
	 * @param boardNo
	 * @return list
	 * @throws Exception
	 */
	public List<Map<String, Object>> selectComment(Connection conn, int boardNo) throws Exception {
		
		List<Map<String, Object>> list = null;
		
		try {
			//xml 내용 -> Properties 객체에 불러와서
			// -> 그 중 key가 selectComment인 entry의 내용을 얻어옴
			String query = prop.getProperty("selectComment");
			
			// SQL을 수항해고 결과를 반환받을 PreparedStatement 객체 생성
			pstmt = conn.prepareStatement(query);
			
			// 위치 홀더에 알맞은 값 배치
			pstmt.setInt(1, boardNo);
			
			// SQL 수행 후 결과인 ResultSet을 반환받아 저장
			rset = pstmt.executeQuery();
			
			// rset에 저장된 값을 따로 저장할 List 객체 생성
			list = new ArrayList<Map<String,Object>>();
			// 조회 결과를 수렴할 수 있는 vo가 없으므로  Map을 활용
			
			//rset에 저장된 내용을 한 행씩 접근하여  list에 추가
			while(rset.next()) {
				// COMMENT_NO, CONTENT, CREATE_DT 를 Comment 객체에 세팅
				Comment comment = new Comment();
				comment.setCommentNo(rset.getInt("COMMENT_NO"));
				comment.setContent(rset.getString("CONTENT"));
				comment.setCreateDt(rset.getDate("CREATE_DT"));
				
				// comment, MEM_NM 두 값을 Map에 저장
				// map에 추가는 put
				Map<String ,Object> map = new HashMap<String, Object>();
				map.put("comment", comment);
				map.put("memNm", rset.getNString("MEM_NM"));

				// map을 list의 요소로 추가
				list.add(map);
			}
		}finally {
			close(rset);
			close(pstmt);
		}
		return list;
	}


	/** 댓글 작성 DAO
	 * @param conn
	 * @param comment
	 * @return result
	 * @throws Exception
	 */
	public int insertComment(Connection conn, Comment comment) throws Exception {
		int result = 0;
		try {
			String query = prop.getProperty("insertComment");
			
			pstmt = conn.prepareStatement(query);
			pstmt.setString(1, comment.getContent());
			pstmt.setInt(2, comment.getMemNo());
			pstmt.setInt(3, comment.getBoardNo());
			
			result = pstmt.executeUpdate();
			
		}finally {
			close(pstmt);
		}
		return result;
	}



	/** 댓글 수정 DAO
	 * @param conn
	 * @param comment
	 * @return result
	 * @throws Exception
	 */
	public int updateComment(Connection conn, Comment comment)throws Exception {
		int result =0;
		
		try {
			String query = prop.getProperty("updateComment");
			
			pstmt = conn.prepareStatement(query);
			pstmt.setString(1, comment.getContent());
			pstmt.setInt(2, comment.getCommentNo());
			
			result = pstmt.executeUpdate();
			
		}finally {
			close(pstmt);
		}
		return result;
	}
	
	
	/**댓글 삭제 DAO
	 * @param conn
	 * @param comment
	 * @return result
	 * @throws Exception
	 */
	public int updateDeleteFl(Connection conn, Comment comment)throws Exception {
		int result =0;
		try {
			String query = prop.getProperty("updateDeleteFl");
			pstmt = conn.prepareStatement(query);
			pstmt.setInt(1, comment.getCommentNo());
			
			result = pstmt.executeUpdate();
			
		}finally {
			close(pstmt);
		}
		
		return result;
	}
	
	
	
	/**댓글 확인 DAO
	 * @param conn
	 * @param comment
	 * @return result
	 * @throws Exception
	 */
	public int chekMyComment(Connection conn, Comment comment) throws Exception {
		int result =0;
		try {
			String query = prop.getProperty("checkMyComment");
			
			pstmt = conn.prepareStatement(query);
			pstmt.setInt(1, comment.getBoardNo());
			pstmt.setInt(2, comment.getCommentNo());
			pstmt.setInt(3, comment.getMemNo());
			
			rset = pstmt.executeQuery();
			
			if(rset.next()) {
				result = rset.getInt(1);	
			}
		}finally {
			close(rset);
			close(pstmt);
		}
		return result;
	}


	
}

```

```java
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
<!-- Comment 관련 SQL구문을 작성하는 xml파일 -->


<!-- 댓글 조회 -->
<entry key="selectComment">
SELECT COMMENT_NO, CONTENT, CREATE_DT, MEM_NM
FROM TB_COMMENT
JOIN TB_MEMBER USING(MEM_NO)
WHERE BOARD_NO = ?
AND DELETE_FL = 'N'
ORDER BY COMMENT_NO DESC
</entry>

<!-- 댓글 작성 -->
<entry key="insertComment">
INSERT INTO TB_COMMENT(COMMENT_NO, CONTENT, MEM_NO, BOARD_NO)
VALUES(SEQ_CNO.NEXTVAL, ?, ?, ?)
</entry>

<!-- 댓글 수정 -->
<entry key="updateComment">
UPDATE TB_COMMENT SET
CONTENT = ?
WHERE COMMENT_NO = ?
</entry>

<!-- 댓글 삭제 -->
<entry key="updateDeleteFl">
UPDATE TB_COMMENT SET
DELETE_FL = 'Y'
WHERE COMMENT_NO = ?
</entry>


<!-- 자신의 댓글 확인 -->
<entry key="checkMyComment">
SELECT COUNT(*) FROM TB_COMMENT
WHERE BOARD_NO = ?
AND COMMENT_NO =?
AND MEM_NO = ?
AND DELETE_FL = 'N'
</entry>


</properties>
```

<br/><br/>