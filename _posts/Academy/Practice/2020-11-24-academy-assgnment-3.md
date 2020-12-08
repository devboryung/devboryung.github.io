---
title: "2020년 11월 24일"
excerpt: "JDBC 과제 "
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
//Member.java
package com.kh.admin.model.vo;

import java.sql.Date;

public class Member {
	private int memNo; // 회원 번호
	private String memId; // 아이디
	private String memPw; // 비밀번호
	private String memNm; // 이름
	private String phone; // 전화번호
	private char gender; // 성별
	private Date hireDt; // 가입일
	private char scsnFl; // 탈퇴여부
	
	public Member() {}
	
	
	// 모든 회원 조회용
	public Member(int memNo, String memId, String memNm, String phone, char gender, Date hireDt, char scsnFl) {
		super();
		this.memNo = memNo;
		this.memId = memId;
		this.memNm = memNm;
		this.phone = phone;
		this.gender = gender;
		this.hireDt = hireDt;
		this.scsnFl = scsnFl;
	}


	public Member(int memNo, String memId, String memPw, String memNm, String phone, char gender, Date hireDt,
			char scsnFl) {
		super();
		this.memNo = memNo;
		this.memId = memId;
		this.memPw = memPw;
		this.memNm = memNm;
		this.phone = phone;
		this.gender = gender;
		this.hireDt = hireDt;
		this.scsnFl = scsnFl;
	}


	public int getMemNo() {return memNo;}

	public void setMemNo(int memNo) {this.memNo = memNo;}

	public String getMemId() {return memId;	}

	public void setMemId(String memId) {this.memId = memId;	}

	public String getMemPw() {return memPw;}

	public void setMemPw(String memPw) {this.memPw = memPw;	}

	public String getMemNm() {return memNm;	}

	public void setMemNm(String memNm) {this.memNm = memNm;	}

	public String getPhone() {return phone;	}

	public void setPhone(String phone) {this.phone = phone;	}

	public char getGender() {return gender;	}

	public void setGender(char gender) {this.gender = gender;}

	public Date getHireDt() {return hireDt;	}

	public void setHireDt(Date hireDt) {this.hireDt = hireDt;}

	public char getScsnFl() {return scsnFl;	}

	public void setScsnFl(char scsnFl) {this.scsnFl = scsnFl;	}
	
	@Override
	public String toString() {
		return "회원 번호 :" + memNo + ", 아이디 : " + memId + ", 비밀번호 :" + memPw + ", 이름 :" + memNm + ", 전화번호 :"
				+ phone + ", 성별 : " + gender + ", 가입일 : " + hireDt + ", 탈퇴 여부 : " + scsnFl;
	}
	
}

```

```java
//Board.java
package com.kh.admin.model.vo;

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

	
	// 모든 게시글 조회용
	public Board(int boardNo, String title, String content, Date createDt, int readCount, char deleteFl, int memNo) {
		super();
		this.boardNo = boardNo;
		this.title = title;
		this.content = content;
		this.createDt = createDt;
		this.readCount = readCount;
		this.deleteFl = deleteFl;
		this.memNo = memNo;
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

	public int getBoardNo() {return boardNo;}

	public void setBoardNo(int boardNo) {this.boardNo = boardNo;}

	public String getTitle() {return title;	}

	public void setTitle(String title) {this.title = title;	}

	public String getContent() {return content;	}

	public void setContent(String content) {this.content = content;	}

	public Date getCreateDt() {return createDt;	}

	public void setCreateDt(Date createDt) {this.createDt = createDt;}

	public int getReadCount() {return readCount;}

	public void setReadCount(int readCount) {this.readCount = readCount;}

	public char getDeleteFl() {return deleteFl;	}

	public void setDeleteFl(char deleteFl) {this.deleteFl = deleteFl;	}

	public int getMemNo() {return memNo;	}

	public void setMemNo(int memNo) {this.memNo = memNo;	}

	public int getCategoryCd() {return categoryCd;	}

	public void setCategoryCd(int categoryCd) {this.categoryCd = categoryCd;	}

	@Override
	public String toString() {
		return "게시판 번호 : " + boardNo + ", 제목 : " + title + ", 내용 : " + content + ", 작성일 : " + createDt
				+ ", 조회수 : " + readCount + ", 삭제여부 :" + deleteFl + ", 회원 번호 : " + memNo + ", 카테고리 코드: "
				+ categoryCd + "]";
	}
	
}

```

```java
//Comment.java
package com.kh.admin.model.vo;

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

	public int getCommentNo() {return commentNo;}

	public void setCommentNo(int commentNo) {this.commentNo = commentNo;}

	public String getContent() {return content;	}

	public void setContent(String content) {this.content = content;	}

	public Date getCreateDt() {return createDt;	}

	public void setCreateDt(Date createDt) {this.createDt = createDt;}

	public char getDeleteFl() {return deleteFl;	}

	public void setDeleteFl(char deleteFl) {this.deleteFl = deleteFl;}

	public int getMemNo() {return memNo;}

	public void setMemNo(int memNo) {this.memNo = memNo;}

	public int getBoardNo() {return boardNo;}

	public void setBoardNo(int boardNo) {this.boardNo = boardNo;}

	@Override
	public String toString() {
		return "Comment [commentNo=" + commentNo + ", content=" + content + ", createDt=" + createDt + ", deleteFl="
				+ deleteFl + ", memNo=" + memNo + ", boardNo=" + boardNo + "]";
	}
		
}

```


```java
//AdminView.java
package com.kh.admin.view;

import java.util.InputMismatchException;
import java.util.List;
import java.util.Scanner;

import com.kh.admin.model.service.AdminService;
import com.kh.admin.model.vo.Board;
import com.kh.admin.model.vo.Comment;
import com.kh.admin.model.vo.Member;

public class AdminView {

	private Scanner sc = new Scanner(System.in);
	private AdminService aService = new AdminService();

	public void displayMain() {
		int sel = 0;

		do {
			try {
				System.out.println("원하는 서비스 번호를 입력해주세요");
				System.out.println("1. 회원 Service");
				System.out.println("2. 게시글 Service");
				System.out.println("3. 댓글 Service");
				System.out.println("0. 프로그램 종료");
				System.out.print("메뉴 선택 >>");
				sel = sc.nextInt();
				sc.nextLine();
				System.out.println();

				switch(sel){
				case 1: memberService(); break;
				case 2: boardService(); break;
				case 3: commentService(); break;
				case 0: System.out.println("프로그램이 종료되었습니다.");break;
				default : System.out.println("다시 입력해주세요");
				}
			}catch(InputMismatchException e) {
				System.out.println("잘못 입력하셨습니다.");
				sel = -1;
				sc.nextLine();
			}catch (Exception e) {
				System.out.println("에러가 발생했습니다.");
				e.printStackTrace();
			}
		}while(sel!=0);
	}



	/**
	 * 회원 서비스 View
	 */
	private void memberService() {
		int sel =0;
		do {
			try {
				System.out.println("[회원 Service]");
				System.out.println("1. 모든 회원 조회");
				System.out.println("2. 탈퇴 회원 조회");
				System.out.println("3. 탈퇴 회원 복구");
				System.out.println("0. 메인 메뉴로");
				System.out.print("메뉴 선택 >>");
				sel = sc.nextInt();
				sc.nextLine();
				System.out.println();

				switch(sel) {
				case 1 : selectAllMem(); break;
				case 2 : selectDeleteMem(); break;
				case 3 : recoverDeleteMem(); break;
				case 0 : displayMain(); break;
				default : System.out.println("잘못 입력하셨습니다.");
				}

			}catch (InputMismatchException e) {
				System.out.println("잘못 입력하셨습니다. 다시 입력해주세요");
				sel =-1;
				sc.nextLine();
			}catch (Exception e) {
				System.out.println("에러가 발생했습니다.");
				e.printStackTrace();
			}

		}while(sel!=0);
	}




	/**
	 * 전체 회원 조회 View
	 */
	private void selectAllMem() {
		System.out.println("[전체 회원 조회]");

		try {
			List<Member> list = aService.selectAllMem();

			for(Member mem : list) {

				System.out.println("회원 번호 : "+ mem.getMemNo() +", 회원 아이디 : "+mem.getMemId()+
						", 회원 이름 : " + mem.getMemNm() + ", 전화번호 : " + mem.getPhone() +
						", 성별 : " + mem.getGender() + ", 가입일 : " + mem.getHireDt()
						+ ", 탈퇴여부 : " + mem.getScsnFl());
			}
		}catch (Exception e) {
			System.out.println("조회 도중 에러가 발생했습니다.");
			e.printStackTrace();
		}

	}



	/**
	 * 탈퇴 회원 조회
	 */
	private void selectDeleteMem() {
		System.out.println("[탈퇴한 회원 조회]");
		try {
			List<Member> list = aService.selectDeleteMem();

			if(list.isEmpty()) {
				System.out.println("조회 결과가 없습니다.");
			}else {

				for(Member mem : list) {
					System.out.println("회원 번호 : "+ mem.getMemNo() +", 회원 아이디 : "+mem.getMemId()+
							", 회원 이름 : " + mem.getMemNm() + ", 전화번호 : " + mem.getPhone() +
							", 성별 : " + mem.getGender() + ", 가입일 : " + mem.getHireDt()
							+ ", 탈퇴여부 : " + mem.getScsnFl());
				}
			}

		}catch (Exception e) {
			System.out.println("조회 과정에서 오류가 발생했습니다.");
			e.printStackTrace();
		}

	}





	/**
	 * 탈퇴 회원 복구
	 */
	private void recoverDeleteMem() {
		System.out.println("[탈퇴 회원 복구]");
		System.out.print("복구할 회원의 번호  : ");
		int memNo = sc.nextInt();
		sc.nextLine();

		try {
			int result = aService.recoverDeleteMem(memNo);

			if(result>0) {
				System.out.println("복구를 정상적으로 수행했습니다.");
			}else {
				System.out.println("복구 실패했습니다.");
			}
		}catch (Exception e) {
			System.out.println("복구 과정에서 오류가 발생했습니다.");
			e.printStackTrace();
		}

	}




	/**
	 * 게시글 Service View
	 */
	private void boardService() {
		int sel =0;
		do {
			try {
				System.out.println("[게시글 Service]");
				System.out.println("1. 모든 게시글 조회");
				System.out.println("2. 삭제된 게시글 조회");
				System.out.println("3. 삭제된 게시글 복구");
				System.out.println("0. 메인 메뉴로");
				System.out.print("메뉴 선택 >>");
				sel = sc.nextInt();
				sc.nextLine();
				System.out.println();

				switch(sel) {
				case 1 : selectAllBoard(); break;
				case 2 : selectDeleteBoard(); break;
				case 3 : recoverDeleteBoard(); break;
				case 0 : displayMain(); break;
				default : System.out.println("잘못 입력하셨습니다.");
				}

			}catch (InputMismatchException e) {
				System.out.println("잘못 입력하셨습니다. 다시 입력해주세요");
				sel =-1;
				sc.nextLine();
			}catch (Exception e) {
				System.out.println("에러가 발생했습니다.");
				e.printStackTrace();
			}

		}while(sel!=0);


	}





	/**
	 * 게시글 전체 조회
	 */
	private void selectAllBoard() {
		System.out.println("[전체 게시글 조회]");

		try {
			List<Board> list  = aService.selectAllBoard();

			for(Board board : list) {

				System.out.println("게시글 번호 : "+ board.getBoardNo() +", 제목 : "+board.getTitle()+
						",\n 내용 : " + board.getContent() + " 작성일 : " + board.getCreateDt() +
						", 조회수 : " + board.getReadCount() + ", 삭제여부 : " + board.getDeleteFl()
						+ ", 회원 번호 : " + board.getMemNo());
				System.out.println();
			}
		}catch (Exception e) {
			System.out.println("조회 과정에서 오류 발생");
			e.printStackTrace();
		}

	}


	/**
	 * 삭제된 게시글 조회
	 */
	private void selectDeleteBoard() {
		System.out.println("[삭제된 게시글 조회]");
		try {

			List<Board> list = aService.selectDeleteBoard();

			if(list.isEmpty()) {
				System.out.println("조회 결과가 없습니다.");
			}else {
				for(Board board : list) {

					System.out.println("게시글 번호 : "+ board.getBoardNo() +", 제목 : "+board.getTitle()+
							",\n 내용 : " + board.getContent() + " 작성일 : " + board.getCreateDt() +
							", 조회수 : " + board.getReadCount() + ", 삭제여부 : " + board.getDeleteFl()
							+ ", 회원 번호 : " + board.getMemNo());
					System.out.println();
				}
			}
		}catch (Exception e) {
			System.out.println("조회 과정에서 오류 발생");
			e.printStackTrace();
		}


	}
	
	





	/**
	 * 삭제된 게시글 복구
	 */
	private void recoverDeleteBoard() {
		System.out.println("[삭제된 게시글 복구]");
		System.out.print("복구하려는 게시글 번호 : ");
		int boardNo = sc.nextInt();
		sc.nextLine();
		
		try {
			int result = aService.recoverDeleteBoard(boardNo);
			
			if(result>0) {
				System.out.println("성공적으로 복구되었습니다.");
				System.out.println();
			}else {
				System.out.println("복구 실패했습니다.");
			}
			
		}catch (Exception e) {
			System.out.println("조회 과정에서 오류 발생");
			e.printStackTrace();
		}
		
	}



	



	/**
	 * 댓글 서비스
	 */
	private void commentService() {
		int sel =0;
		do {
			try {
				System.out.println("[댓글 Service]");
				System.out.println("1. 삭제된 댓글 조회");
				System.out.println("2. 삭제된 댓글 복구");
				System.out.println("0. 메인 메뉴로");
				System.out.print("메뉴 선택 >>");
				sel = sc.nextInt();
				sc.nextLine();
				System.out.println();

				switch(sel) {
				case 1 : selectDeleteComment(); break;
				case 2 : recoverDeleteComment(); break;
				case 0 : displayMain(); break;
				default : System.out.println("잘못 입력하셨습니다.");
				}

			}catch (InputMismatchException e) {
				System.out.println("잘못 입력하셨습니다. 다시 입력해주세요");
				sel =-1;
				sc.nextLine();
			}catch (Exception e) {
				System.out.println("에러가 발생했습니다.");
				e.printStackTrace();
			}

		}while(sel!=0);
		
	}




	/**
	 * 삭제된 댓글 조회
	 */
	private void selectDeleteComment() {
		System.out.println("[삭제된 댓글 조회]");
		try {
			List<Comment> list = aService.selectDeleteComment();
			
			for(Comment com : list) {
				System.out.println("댓글 번호 : "+ com.getCommentNo() +", 내용 : "+ com.getContent()+
						", 작성일 : " + com.getCreateDt() + ", 삭제 여부 : " + com.getDeleteFl() +
						", 회원 번호 : " + com.getMemNo() + ", 게시글 번호 : " + com.getBoardNo());
				System.out.println();
			}
			
		}catch (Exception e) {
			System.out.println("오류 발생");
			e.printStackTrace();
		}
		
	}
	
	


	/**
	 * 삭제된 댓글 복구
	 */
	private void recoverDeleteComment() {
		System.out.println("[삭제된 댓글 복구]");
		System.out.print("복구할 댓글 번호 : ");
		int commentNo = sc.nextInt();
		sc.nextLine();
		
		try {
			int result = aService.recoverDeleteComment(commentNo);
			
			if(result>0) {
				System.out.println("복구 성공");
			}else {
				System.out.println("복구 실패");
			}
			
		}catch (Exception e) {
			System.out.println("오류 발생");
			e.printStackTrace();
		}
	}




}


```
```java
//AdminService.java
package com.kh.admin.model.service;

import static com.kh.admin.common.JDBCTemplate.*;

import java.sql.Connection;
import java.util.List;

import com.kh.admin.model.dao.AdminDAO;
import com.kh.admin.model.vo.Board;
import com.kh.admin.model.vo.Comment;
import com.kh.admin.model.vo.Member;

public class AdminService {
	private AdminDAO aDAO = new AdminDAO();
	

	/** 모든 회원 조회 Service
	 * @return list
	 * @throws Exception
	 */
	public List<Member> selectAllMem() throws Exception {
		Connection conn = getConnection();
				
		List<Member> list = aDAO.selectAllMem(conn);
		
		close(conn);
		return list;
	}


	/**탈퇴 회원 조회 Service
	 * @return
	 * @throws Exception
	 */
	public List<Member> selectDeleteMem() throws Exception {
		Connection conn = getConnection();
		
		List<Member>list = aDAO.selectDeleteMem(conn);
				
		close(conn);
		
		return list;
	}


	/** 회원 복구 Service
	 * @param memNo
	 * @return result
	 * @throws Exception
	 */
	public int recoverDeleteMem(int memNo) throws Exception {
		Connection conn = getConnection();
		
		int result = aDAO.recoverDeleteMem(conn, memNo);
		
		if(result>0) commit(conn);
		else		rollback(conn);
		
		close(conn);
		return result;
	}


	/** 모든 게시글 조회
	 * @return list
	 * @throws Exception
	 */
	public List<Board> selectAllBoard() throws Exception {
		Connection conn = getConnection();
		
		List<Board> list = aDAO.selectAllBoard(conn);
		
		close(conn);
		return list;
	}


	/**삭제된 게시글 조회
	 * @return list
	 * @throws Exception
	 */
	public List<Board> selectDeleteBoard() throws Exception {
		Connection conn = getConnection();
		List<Board> list = aDAO.selectDeleteBoard(conn);
		
		close(conn);
		
		return list;
	}


	/** 삭제된 게시글 복구
	 * @param boardNo 
	 * @return result
	 * @throws Exception
	 */
	public int recoverDeleteBoard(int boardNo) throws Exception {
		Connection conn = getConnection();
		
		int result = aDAO.recoverDeleteBoard(conn, boardNo);
		
		if(result>0) commit(conn);
		else		rollback(conn);
		
		close(conn);
		return result;
	}


	/** 삭제된 댓글 조회
	 * @return list
	 * @throws Exception
	 */
	public List<Comment> selectDeleteComment() throws Exception {
		Connection conn = getConnection();
		List<Comment> list = aDAO.selectDeleteComment(conn);
		close(conn);
		return list;
	}


	/** 삭제된 댓글 복구
	 * @param commentNo
	 * @return result
	 * @throws Exception
	 */
	public int recoverDeleteComment(int commentNo) throws Exception {
		Connection conn = getConnection();
		
		int result = aDAO.recoverDeleteComment(conn, commentNo);
		
		if(result>0) commit(conn);
		else	rollback(conn);
		
		close(conn);
		return result;
	}

}

```

```java
//AdminDAO.java
package com.kh.admin.model.dao;

import static com.kh.admin.common.JDBCTemplate.*;

import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Properties;

import com.kh.admin.model.vo.Board;
import com.kh.admin.model.vo.Comment;
import com.kh.admin.model.vo.Member;

public class AdminDAO {
	
	private Statement stmt = null;
	private PreparedStatement pstmt = null;
	private ResultSet rset = null;
	private Properties prop = null;
	
	public AdminDAO() {
		try {
			prop = new Properties();
			prop.loadFromXML(new FileInputStream("Admin-query.xml"));
		}catch (Exception e) {
			e.printStackTrace();
		}
	}

	
	
	
	/**모든 회원 조회 DAO
	 * @param conn
	 * @return list
	 * @throws Exception
	 */
	public List<Member> selectAllMem(Connection conn)throws Exception {
		List<Member> list = null;
		try {
			String query = prop.getProperty("selectAll");
			stmt = conn.createStatement();
			rset = stmt.executeQuery(query);
			
			list = new ArrayList<Member>();
			
			while(rset.next()) {
				list.add(new Member(rset.getInt("MEM_NO"), 
						rset.getString("MEM_ID"), 
						rset.getString("MEM_NM"), 
						rset.getString("PHONE"), 
						rset.getString("GENDER").charAt(0), 
						rset.getDate("HIRE_DT"),
						rset.getString("SCSN_FL").charAt(0)));
			}
		}finally {
			if(rset!=null) close(rset);
			if(stmt!=null) close(stmt);
		}
		return list;
	}




	/** 탈퇴 회원 조회
	 * @param conn
	 * @return list
	 * @throws Exception
	 */
	public List<Member> selectDeleteMem(Connection conn) throws Exception{
		List<Member> list= null;
		try {
			String query = prop.getProperty("selectDelete");
			stmt = conn.createStatement();
			
			rset = stmt.executeQuery(query);
			
			list = new ArrayList<Member>();
			while(rset.next()) {
				list.add(new Member(rset.getInt("MEM_NO"), 
						rset.getString("MEM_ID"), 
						rset.getString("MEM_NM"), 
						rset.getString("PHONE"), 
						rset.getString("GENDER").charAt(0), 
						rset.getDate("HIRE_DT"),
						rset.getString("SCSN_FL").charAt(0)));
			}
			
		}finally {
			close(rset);
			close(stmt);
		}
		return list;
	}




	/** 회원 복구 DAO
	 * @param conn
	 * @param memNo
	 * @return result
	 * @throws Exception
	 */
	public int recoverDeleteMem(Connection conn, int memNo)throws Exception {
		int result = 0;
		
		try {
			String query = prop.getProperty("recoverDelete");
			pstmt = conn.prepareStatement(query);
			pstmt.setInt(1, memNo);
			
			result = pstmt.executeUpdate();
		
		}finally {
			close(pstmt);
		}
		
		return result;
	}




	/** 게시글 전체 조회
	 * @param conn
	 * @return list
	 * @throws Exception
	 */
	public List<Board> selectAllBoard(Connection conn) throws Exception {
		List<Board> list = null;
		try {
			String query = prop.getProperty("selectAllBoard");
			stmt = conn.createStatement();
			
			rset = stmt.executeQuery(query);
			
			list = new ArrayList<Board>();
			while(rset.next()) {
				list.add(new Board(rset.getInt("BOARD_NO"), rset.getString("TITLE"), rset.getString("CONTENT"), 
						rset.getDate("CREATE_DT"), rset.getInt("READ_COUNT"), rset.getString("DELETE_FL").charAt(0), 
						rset.getInt("MEM_NO")));
			}
		}finally {
			close(rset);
			close(stmt);
			
		}
		return list;
	}




	/** 삭제된 게시글 조회
	 * @param conn
	 * @return list
	 * @throws Exception
	 */
	public List<Board> selectDeleteBoard(Connection conn)throws Exception {
		List<Board> list = null;
		
		try {
			String query = prop.getProperty("selectDeleteBoard");
			stmt = conn.createStatement();
			
			rset = stmt.executeQuery(query);
			list = new ArrayList<Board>();
			
			while(rset.next()) {
				list.add(new Board(rset.getInt("BOARD_NO"), rset.getString("TITLE"), rset.getString("CONTENT"), 
						rset.getDate("CREATE_DT"), rset.getInt("READ_COUNT"), rset.getString("DELETE_FL").charAt(0), 
						rset.getInt("MEM_NO")));
			}
			
		}finally {
			close(rset);
			close(stmt);
		}
		
		return list;
	}




	/**삭제된 게시글 복구
	 * @param conn
	 * @param boardNo
	 * @return result
	 * @throws Exception
	 */
	public int recoverDeleteBoard(Connection conn, int boardNo) throws Exception {
		int result = 0;
		try {
			String query = prop.getProperty("recoverDeleteBoard");
			pstmt = conn.prepareStatement(query);
			pstmt.setInt(1, boardNo);
			
			result = pstmt.executeUpdate();
			
		}finally {
			close(pstmt);
		}
		return result;
	}




	/**삭제된 댓글 조회
	 * @param conn
	 * @return list
	 * @throws Exception
	 */
	public List<Comment> selectDeleteComment(Connection conn) throws Exception {
		List<Comment> list = null;
		try {
			String query = prop.getProperty("selectDeleteComment");
			
			stmt = conn.createStatement();
			rset = stmt.executeQuery(query);
			
			list = new ArrayList<Comment>();
			
			while(rset.next()) {
				list.add(new Comment(rset.getInt("COMMENT_NO"), rset.getString("CONTENT"), rset.getDate("CREATE_DT"), 
						rset.getString("DELETE_FL").charAt(0), rset.getInt("MEM_NO"), 
						rset.getInt("BOARD_NO")));
			}
		}finally {
			close(rset);
			close(stmt);
		}
		return list;
	}




	/** 삭제된 댓글 복구
	 * @param conn
	 * @param commentNo
	 * @return result
	 * @throws Exception
	 */
	public int recoverDeleteComment(Connection conn, int commentNo) throws Exception {
		int result =0;
		
		try {
			String query = prop.getProperty("recoverDeleteComment");
			pstmt = conn.prepareStatement(query);
			pstmt.setInt(1,commentNo);
			
			result = pstmt.executeUpdate();
			
		}finally {
			close(pstmt);
		}
		return result;
	}

}

```


```java
//사용한 xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>

<!-- Admin SQL 작성 -->


<entry key="selectAll">
SELECT MEM_NO, MEM_ID, MEM_NM, PHONE, GENDER, HIRE_DT,SCSN_FL
FROM TB_MEMBER
</entry>


<entry key="selectDelete">
SELECT MEM_NO, MEM_ID, MEM_NM, PHONE, GENDER, HIRE_DT,SCSN_FL
FROM TB_MEMBER
WHERE SCSN_FL = 'Y'
</entry>


<entry key="recoverDelete">
UPDATE TB_MEMBER
SET SCSN_FL = 'N'
WHERE MEM_NO = ?
</entry>


<entry key="selectAllBoard">
SELECT BOARD_NO, TITLE,CONTENT,CREATE_DT,READ_COUNT,DELETE_FL,MEM_NO
FROM TB_BOARD
</entry>

<entry key="selectDeleteBoard">
SELECT BOARD_NO, TITLE,CONTENT,CREATE_DT,READ_COUNT,DELETE_FL,MEM_NO
FROM TB_BOARD
WHERE DELETE_FL = 'Y'
</entry>

<entry key="recoverDeleteBoard">
UPDATE TB_BOARD
SET DELETE_FL = 'N'
WHERE BOARD_NO = ?
</entry>

<entry key="selectDeleteComment">
SELECT * FROM TB_COMMENT
WHERE DELETE_FL = 'Y'
</entry>

<entry key="recoverDeleteComment">
UPDATE TB_COMMENT
SET DELETE_FL = 'N'
WHERE COMMENT_NO = ?
</entry>


</properties>

```

<br/><br/>