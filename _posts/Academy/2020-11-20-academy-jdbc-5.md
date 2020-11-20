---
title: "2020년 11월 20일"
excerpt: "JDBC-5"
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

```java
//Board.java
package com.kh.jdbc.board.model.vo;

import java.sql.Date;

public class Board {
	private int boardNo;
	private String title;
	private String content;
	private Date createDt;
	private int readCount;
	private char deleteFl;
	private int memNo;
	private int categoryCd;
	
	public Board() {}

	
	// 게시글 작성용 생성자
	public Board(String title, String content, int memNo, int categoryCd) {
		super();
		this.title = title;
		this.content = content;
		this.memNo = memNo;
		this.categoryCd = categoryCd;
	}



	public Board(int boardNo, String title, String content, Date createDt, int readCount, char deleteFl, int memNo,
			int categoryCd) {
		super();
		this.boardNo = boardNo;
		this.title = title;
		this.content = content;
		this.createDt = createDt;
		this.readCount = readCount;
		this.deleteFl = deleteFl;
		this.memNo = memNo;
		this.categoryCd = categoryCd;
	}

	public int getBoardNo() {
		return boardNo;
	}

	public void setBoardNo(int boardNo) {
		this.boardNo = boardNo;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
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

	public int getReadCount() {
		return readCount;
	}

	public void setReadCount(int readCount) {
		this.readCount = readCount;
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

	public int getCategoryCd() {
		return categoryCd;
	}

	public void setCategoryCd(int categoryCd) {
		this.categoryCd = categoryCd;
	}

	@Override
	public String toString() {
		return "게시판 번호 : " + boardNo + ", 제목 : " + title + ", 내용 : " + content + ", 작성일 : " + createDt
				+ ", 조회수 : " + readCount + ", 삭제여부 :" + deleteFl + ", 회원 번호 : " + memNo + ", 카테고리 코드: "
				+ categoryCd + "]";
    }
}


```

```java
//VBoard.java
package com.kh.jdbc.board.model.vo;

import java.sql.Date;

public class VBoard { // V_BOARD 조회 시 사용할 VO
	
	private int boardNo;
	private String title;
	private String content;
	private Date createDt;
	private int readCount;
	private String memNm;
	private String categoryNm;

	public VBoard() {}
	
	
	// 콘텐트 제외 전체 조회용 
	public VBoard(int boardNo, String title, Date createDt, int readCount, String memNm, String categoryNm) {
		super();
		this.boardNo = boardNo;
		this.title = title;
		this.createDt = createDt;
		this.readCount = readCount;
		this.memNm = memNm;
		this.categoryNm = categoryNm;
	}


	public VBoard(int boardNo, String title, String content, Date createDt, int readCount, String memNm,
			String categoryNm) {
		super();
		this.boardNo = boardNo;
		this.title = title;
		this.content = content;
		this.createDt = createDt;
		this.readCount = readCount;
		this.memNm = memNm;
		this.categoryNm = categoryNm;
	}
	public int getBoardNo() {
		return boardNo;
	}
	public void setBoardNo(int boardNo) {
		this.boardNo = boardNo;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
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
	public int getReadCount() {
		return readCount;
	}
	public void setReadCount(int readCount) {
		this.readCount = readCount;
	}
	public String getMemNm() {
		return memNm;
	}
	public void setMemNm(String memNm) {
		this.memNm = memNm;
	}
	public String getCategoryNm() {
		return categoryNm;
	}
	public void setCategoryNm(String categoryNm) {
		this.categoryNm = categoryNm;
	}

	@Override
	public String toString() {
		return "VBoard [boardNo=" + boardNo + ", title=" + title + ", content=" + content + ", createDt=" + createDt
				+ ", readCount=" + readCount + ", memNm=" + memNm + ", categoryNm=" + categoryNm + "]";
	}
	
}

```


```java
//BoardView.java
package com.kh.jdbc.board.view;


import java.util.InputMismatchException;
import java.util.List;
import java.util.Scanner;

import com.kh.jdbc.board.model.service.BoardService;
import com.kh.jdbc.board.model.vo.Board;
import com.kh.jdbc.board.model.vo.VBoard;
import com.kh.jdbc.view.JDBCView;

public class BoardView {
	
	private Scanner sc = new Scanner(System.in);
	private BoardService bService = new BoardService();
	
	/**
	 * 게시판 메뉴
	 */
	/**
	 * 
	 */
	public void boardMenu() {
		int sel = 0;
		
		do {
			try {
				System.out.println("====================================");
				System.out.println("[게시판 메뉴]");
				System.out.println("1. 게시글 목록 조회");
				System.out.println("2. 게시글 상세 조회(게시글 번호)");
				System.out.println("3. 게시글 작성");
				System.out.println("4. 게시글 수정");
				System.out.println("5. 게시글 삭제");
				System.out.println("6. 게시글 검색(제목, 작성자, 내용, 제목+내용)");
				System.out.println("0. 메인 메뉴로 돌아가기");
				System.out.println("====================================");
				
				System.out.print("메뉴 선택 : ");
				sel = sc.nextInt();
				sc.nextLine();
				System.out.println();
				
				switch(sel) {
				case 1 : selectAllBoard(); break; 
				case 2 : selectBoard(); break; 
				case 3 : insertBoard(); break; 
				case 4 :  break; 
				case 5 :  break; 
				case 6 :  break; 
				case 0 : System.out.println("프로그램 종료"); break;
				default : System.out.println("메인 메뉴로 돌아갑니다.");
				}
				
			}catch(InputMismatchException e) {
				System.out.println("숫자만 입력해 주세요.");
				sel = -1;
				sc.nextLine(); // 버퍼에 남아있는 잘못 된 문자 제거 
			}
			
		}while(sel != 0);
	}



	/**
	 * 게시글 목록 조회 View
	 */
	private void selectAllBoard() {
		System.out.println("[게시글 목록]");
		// 카테고리 명 | 글번호 | 제목 | 작성일 | 작성자 | 조회수
		
		try {
			//게시글 목록 조회 Service 호출 후 결과를 반환 받아 출력
			List<VBoard> list = bService.selectAllBoard();
			
			if(list.isEmpty()) {
				System.out.println("조회 결과가 없습니다.");
			}else {
				System.out.printf(" %s | %s | %s | %s | %s | %s\n",
						"카테고리", "글번호", "제목", "작성일", "작성자", "조회수");
				for(VBoard board : list) {
					System.out.printf(" %s | %d | %s | %s | %s | %d\n",
							board.getCategoryNm(), board.getBoardNo(), 
							board.getTitle(), board.getCreateDt(), 
							board.getMemNm(),board.getReadCount());
				}
			}
		}catch (Exception e) {
			System.out.println("게시글 목록 조회 중 오류 발생");
			e.printStackTrace();
		}
		
	}
	


	/**
	 * 게시글 상세 조회 View
	 */
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
				System.out.println("----------------------------------------------------------------------------");
				System.out.printf("%d | [%s] %s\n",vboard.getBoardNo(),  vboard.getCategoryNm(), vboard.getTitle());
				System.out.printf("작성자 : %s | 작성일  : %s | 조회수 %d\n",vboard.getMemNm(), vboard.getCreateDt(), vboard.getReadCount());
				System.out.println("----------------------------------------------------------------------------");
				System.out.println(vboard.getContent());
				System.out.println("----------------------------------------------------------------------------");
			}
			
		}catch(Exception e) {
			System.out.println("게시글 상세 조회 중 오류 발생");
			e.printStackTrace();
		}
	}
	

	/**
	 * 게시글 작성 View
	 */
	private void insertBoard() {
		System.out.println("[게시글 작성]");
		// 카테고리 
		System.out.print("카테고리(1.JAVA / 2.DB / 3.JDBC) : ");
		int categoryCd = sc.nextInt();
		sc.nextLine();
		
		// 제목
		System.out.print("제목 : ");
		String title = sc.nextLine();
		
		// 내용
		StringBuffer sb = new StringBuffer(); // 입력되는 모든 내용을 저장할 객체 생성
		String str = null; //임시로 입력된 값을 한 줄씩 저장할 곳
		
		System.out.println("---내용 입력(exit 입력 시 내용 입력 종료)---");
		while(true) {
			str = sc.nextLine();
			if(str.equals("exit")) {
				break;
			}
			sb.append(str + "\n");
			// 입력된 문자열을 StringBuffer에 누적
		}
		
		try {
			// 카테고리 코드, 제목, 내용, 회원번호를 저장할 수 있는 Board 객체 생성
			Board board = new Board(title, sb.toString(), JDBCView.loginMember.getMemNo(), categoryCd);
									//sb.toString() = content, sb에 있는 모든 정보
			int result = bService.insertBoard(board);
			
			if(result>0){
				System.out.println("게시글 작성 성공");
			}else {
				System.out.println("게시글 작성 실패..");
			}
		}catch(Exception e) {
			System.out.println("게시글 작성 중 오류 발생");
			e.printStackTrace();
		}
	}
}

```


```java
//BoardService
package com.kh.jdbc.board.model.service;

import static com.kh.jdbc.common.JDBCTemplate.*;

import java.sql.Connection;
import java.util.List;

import com.kh.jdbc.board.model.dao.BoardDAO;
import com.kh.jdbc.board.model.vo.Board;
import com.kh.jdbc.board.model.vo.VBoard;

public class BoardService {
	private BoardDAO bDAO = new BoardDAO();

	/** 게시글 목록 조회 Service
	 * @return list
	 * @throws Exception
	 */
	public List<VBoard> selectAllBoard() throws Exception{
		// 커넥션 얻어오기
		Connection conn = getConnection();
		
		// DAO메소드 호출 후 결과를 반환 받음
		List<VBoard> list = bDAO.selectAllBoard(conn);
		
		close(conn);
		
		return list;
	}

	/** 게시글 상세 조회 Service
	 * @param boardNo
	 * @return vboard
	 * @throws Exception
	 */
	public VBoard selectBoard(int boardNo) throws Exception {
		Connection conn = getConnection();
		
		VBoard vboard = bDAO.selectBoard(conn, boardNo);
		
		// DB에서 게시글 정보를 성공적으로 조회를 해왔을 때
		// --> 해당 게시글의 조회수를 증가
		
		if(vboard!=null) {
			// DB에서 해당 글의 조회수를 증가시킬 수 있는 DAO 메소드 호출
			// -->UPDATE 수행
			int result = bDAO.updateReadCount(conn, boardNo);
			
			if(result>0) {
				commit(conn);
				
				vboard.setReadCount( vboard.getReadCount()+1); 
				// vboard 그대로 반환하면 이전값임,,
			}else {
				rollback(conn);
			}
		
		close(conn);
		}
		
		return vboard;
	}

	/** 게시글 작성 Service
	 * @param board
	 * @return result
	 * @throws Exception
	 */
	public int insertBoard(Board board) throws Exception {
		
		Connection conn = getConnection();
		
		int result = bDAO.insertBoard(conn, board);
		
		if(result>0) {
			commit(conn);
		}else {
			rollback(conn);
		}
		
		return result;
	}
	
}

```

```java
//BoardDAO.java
package com.kh.jdbc.board.model.dao;

import static com.kh.jdbc.common.JDBCTemplate.*;
import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Properties;

import com.kh.jdbc.board.model.vo.Board;
import com.kh.jdbc.board.model.vo.VBoard;


public class BoardDAO {

	private Statement stmt = null;
	private PreparedStatement pstmt =null;
	private ResultSet rset = null;
	
	private Properties prop = null;
	
	public BoardDAO() {
		try {
			prop = new Properties();
			prop.loadFromXML(new FileInputStream("board-query.xml"));
		}catch (Exception e) {
			e.printStackTrace();
		}
	}

	
	/**게시글 목록 조회 DAO
	 * @param conn
	 * @return list
	 * @throws Exception
	 */
	public List<VBoard> selectAllBoard(Connection conn) throws Exception {
		// 조회 결과를 저장하고 반환할 변수 선언
		List<VBoard> list = null;
		
		try {
			String query = prop.getProperty("selectAllBoard");
			stmt = conn.createStatement();
			
			// SQL 수행 후 조회 결과 반환
			rset = stmt.executeQuery(query);
			
			// SQL 수행 시 문제가 없었다면 조회 내용을 저장할 list 객체를 생성
			list = new ArrayList<VBoard>();
			
			while(rset.next()) {
				
				list.add(new VBoard(rset.getInt("BOARD_NO"), 
						            rset.getString("TITLE"), 
						            rset.getDate("CREATE_DT"), 
						            rset.getInt("READ_COUNT"), 
						            rset.getString("MEM_NM"), 
						            rset.getString("CATEGORY_NM")) );
			}
		}finally {
			close(rset);
			close(stmt);
		}
		return list;
	}


	/** 게시글 상세조회 DAO
	 * @param conn
	 * @param boardNo
	 * @return vboard
	 * @throws Exception
	 */
	public VBoard selectBoard(Connection conn, int boardNo) throws Exception {
		VBoard vboard = null;
		
		try {
			String query = prop.getProperty("selectBoard");
			pstmt = conn.prepareStatement(query);
			pstmt.setInt(1, boardNo);
			
			rset = pstmt.executeQuery();
			
			if(rset.next()) {
				vboard =new VBoard( rset.getInt("BOARD_NO"), 
			            rset.getString("TITLE"), 
			            rset.getString("CONTENT"),
			            rset.getDate("CREATE_DT"), 
			            rset.getInt("READ_COUNT"), 
			            rset.getString("MEM_NM"), 
			            rset.getString("CATEGORY_NM"));
			}
			
		}finally {
			close(rset);
			close(pstmt);
		}
		return vboard;
	}


	/** 게시글 조회수 증가 DAO
	 * @param conn 
	 * @param boardNo
	 * @return result
	 * @throws Exception
	 */
	public int updateReadCount(Connection conn, int boardNo) throws Exception {
		int result = 0;
		try {
			String query = prop.getProperty("updateReadCount");
			
			pstmt = conn.prepareStatement(query);
			pstmt.setInt(1,boardNo);
			
			result = pstmt.executeUpdate();
		}finally {
			close(pstmt);
		}
		
		return result;
	}


	/** 게시글 작성 DAO
	 * @param conn
	 * @param board
	 * @return result
	 * @throws Exception
	 */
	public int insertBoard(Connection conn, Board board)throws Exception {
		
		int result = 0; // DB 수행 결과를 저장할 변수 선언
		
		try {
			String query = prop.getProperty("insertBoard");
			
			pstmt = conn.prepareStatement(query);
			pstmt.setString(1, board.getTitle());
			pstmt.setString(2, board.getContent());
			pstmt.setInt(3, board.getMemNo());
			pstmt.setInt(4, board.getCategoryCd());
			
			result = pstmt.executeUpdate();
			
		}finally {
			close(pstmt);
		}
		return result;
	}
}

```

```java
// 사용한 xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
<!-- board 관련 SQL구문을 작성하는 xml파일 -->

<!--특정 게시글이 특정 회원의 글이 맞는지 조회 -->
<entry key="checkMyBoard">
SELECT COUNT(*) FROM TB_BOARD
WHERE MEM_NO = ?
AND BOARD_NO =?
AND DELETE_FL = 'N'
</entry>

<!--게시글 업데이트 -->
<entry key="updateBoard">
UPDATE TB_BOARD
SET TITLE = ?,
CONTENT = ?,
CATEGORY_CD = ?
WHERE BOARD_NO = ?
</entry>

<!--게시글 삭제여부 변경-->
<entry key="updateDeleteFl">
UPDATE TB_BOARD
SET DELETE_FL = 'Y'
WHERE BOARD_NO = ?
</entry>
```

<br/><br/>