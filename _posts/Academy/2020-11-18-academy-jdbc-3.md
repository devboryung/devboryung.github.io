---
title: "2020년 11월 18일"
excerpt: "JDBC-3"
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
//JDBCRun.java
package com.kh.jdbc.member.model.vo;

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


	// 회원 검색용 생성자
	public Member(String memId, String memNm, String phone, char gender, Date hireDt) {
		super();
		this.memId = memId;
		this.memNm = memNm;
		this.phone = phone;
		this.gender = gender;
		this.hireDt = hireDt;
	}
	
	
	// 회원 수정용 생성자
	public Member(int memNo, String memNm, String phone, char gender) {
		super();
		this.memNo = memNo;
		this.memNm = memNm;
		this.phone = phone;
		this.gender = gender;
	}

	// 성별 검색용 생성자
	public Member(String memId, String memNm, String phone, char gender) {
		super();
		this.memId = memId;
		this.memNm = memNm;
		this.phone = phone;
		this.gender = gender;
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
	
	public int getMemNo() {
		return memNo;
	}
	public void setMemNo(int memNo) {
		this.memNo = memNo;
	}
	public String getMemId() {
		return memId;
	}
	public void setMemId(String memId) {
		this.memId = memId;
	}
	public String getMemPw() {
		return memPw;
	}
	public void setMemPw(String memPw) {
		this.memPw = memPw;
	}
	public String getMemNm() {
		return memNm;
	}
	public void setMemNm(String memNm) {
		this.memNm = memNm;
	}
	public String getPhone() {
		return phone;
	}

	public void setPhone(String phone) {
		this.phone = phone;
	}

	public char getGender() {
		return gender;
	}

	public void setGender(char gender) {
		this.gender = gender;
	}
	public Date getHireDt() {
		return hireDt;
	}
	public void setHireDt(Date hireDt) {
		this.hireDt = hireDt;
	}
	public char getScsnFl() {
		return scsnFl;
	}
	public void setScsnFl(char scsnFl) {
		this.scsnFl = scsnFl;
	}

	@Override
	public String toString() {
		return "회원 번호 :" + memNo + ", 아이디 : " + memId + ", 비밀번호 :" + memPw + ", 이름 :" + memNm + ", 전화번호 :"
				+ phone + ", 성별 : " + gender + ", 가입일 : " + hireDt + ", 탈퇴 여부 : " + scsnFl;
	}

}



```


```java
//JDBCTemplate.java
package com.kh.jdbc.common;

import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.Properties;

public class JDBCTemplate {

	private static Connection conn =null; 
	
	private JDBCTemplate() {}

	public static Connection getConnection() {
		try {
			if(conn == null || conn.isClosed()) {

			Properties prop = new Properties();
		
			prop.loadFromXML(new FileInputStream("driver.xml"));

			Class.forName(prop.getProperty("driver"));
			conn = DriverManager.getConnection(prop.getProperty("url"), prop.getProperty("user"), prop.getProperty("password"));

			conn.setAutoCommit(false);
			}			
		}catch(Exception e) {
			e.printStackTrace();
		}
		return conn;
	}

	public static void commit(Connection conn) {
		try {
			if(conn != null && !conn.isClosed()) {
				conn.commit();
			}
		}catch(Exception e) {
			e.printStackTrace();
		}
		
	}

	public static void rollback(Connection conn) {
		try {
			if(conn != null && !conn.isClosed()) {
				conn.rollback();
			}
		}catch(Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void close(Connection conn) {
		try {
			if(conn != null && !conn.isClosed()) { 
				conn.close();
			}
		}catch(Exception e) {
			e.printStackTrace();
		}
		
	}
	
	public static void close(ResultSet rset) {
		try {
			if(rset != null && !rset.isClosed()) { 
				rset.close();
			}
		}catch(Exception e) {
			e.printStackTrace();
		}		
	}
	

	public static void close(Statement stmt) {
		try {
			if(stmt != null && !stmt.isClosed()) { 
				stmt.close();
			}
		}catch(Exception e) {
			e.printStackTrace();
		}		
	}	
}
```


```java
//TestProperties.java
package com.kh.jdbc;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.util.Properties;

public class TestProperties {
	public static void main(String[] args) {
	
		Properties prop = new Properties();

		prop.setProperty("driver", "oracle.jdbc.driver.OracleDriver");
		prop.setProperty("url","jdbc:oracle:thin:@localhost:1521:xe");
		prop.setProperty("user", "jdbc");
		prop.setProperty("password","jdbc");
				
		try { 
			prop.storeToXML(new FileOutputStream("driver.xml"), "driver information");
			
		}catch (Exception e) {
		}

		try {
			prop.loadFromXML(new FileInputStream("driver.xml"));
			
		System.out.println(prop.getProperty("driver")); 
		System.out.println(prop.getProperty("url")); 
		System.out.println(prop.getProperty("user")); 
		System.out.println(prop.getProperty("password")); 
			
		}catch (Exception e) {
			e.printStackTrace();
		}
	}
}


```

```java
//JDBCView.java
package com.kh.jdbc.View;

import java.util.InputMismatchException;
import java.util.List;
import java.util.Scanner;
import com.kh.jdbc.member.model.sevice.MemberService;
import com.kh.jdbc.member.model.vo.Member;

/** JDBCProject View
 * @author 강보령
 */
public class JDBCview {
	
	private Scanner sc = new Scanner(System.in);
	private MemberService mService = new MemberService();

	private Member loginMember = null; // 로그인 된 회원의 정보를 저장. 해당 클래스 내 모든 기능에서 사용 가능.
	
	public void displayMain() {
		int sel = 0;
				
		do {
			try { 
				if(loginMember == null) { 
					System.out.println("★☆★☆★☆<JDBC PROJECT>☆★☆★☆★");
					System.out.println("=============================");
					System.out.println("1. 로그인");
					System.out.println("2. 회원가입");
					System.out.println("0. 프로그램 종료");
					System.out.println("=============================");

					System.out.print("메뉴 선택 >>");
					sel = sc.nextInt();
					sc.nextLine();

					System.out.println();

					switch(sel) {
					case 1: login(); break;
					case 2: signUp(); break;
					case 0: System.out.println("프로그램 종료");; break;
					default : System.out.println("잘못 입력하셨습니다.");
					}
				}else { 
					System.out.println("=============================");
					System.out.println("[메인 메뉴]");
					System.out.println("1. 회원 기능");
					System.out.println("9. 로그아웃");
					System.out.println("0. 프로그램 종료");
					System.out.println("=============================");
					
					System.out.print("메뉴 선택 >>");
					sel = sc.nextInt();
					sc.nextLine();
					
				
					System.out.println();
					
					switch(sel) {
					case 1: memberMenu();break;
					case 9: loginMember = null; //loginMember = 로그인 된 회원 정보 저장되어있는것
							System.out.println("★로그아웃 되었습니다★\n");
							break;
					case 0: System.out.println("프로그램 종료");; break;
					default : System.out.println("잘못 입력하셨습니다.");
					}

				}
			}catch(InputMismatchException e) {
				System.out.println("숫자만 입력해 주세요.");
				sel = -1;
				sc.nextLine();
			}
		}while(sel != 0);
	}

	/**
	 *  회원 기능 메뉴 View
	 */
	private void memberMenu() {
		int sel = 0;
		do {
			try {
				if(loginMember == null) return;
				//회원 탈퇴로 인해 로그아웃 될 경우 메인 메뉴로 리턴(돌아가기)
				System.out.println("==============================");
				System.out.println("[회원 기능 메뉴]");
				System.out.println("1. 내 정보 조회");
				System.out.println("2. 검색어가 이름에 포함된 모든 회원 검색");
				System.out.println("3. 내 정보 수정");
				System.out.println("4. 비밀번호 변경");
				System.out.println("5. 회원 탈퇴");
				System.out.println("6. 성별 조회");
				System.out.println("0. 메인 메뉴로 돌아가기");
				System.out.println("==============================");
				
				System.out.print("메뉴 선택 >>");
				sel = sc.nextInt();
				sc.nextLine();
				
				System.out.println();
				switch(sel) {
				case 1 : selectMyInfo(); break;
				case 2 : selectMemberName(); break;
				case 3 : updateMyInfo(); break;
				case 4 : updatePw(); break;
				case 5 : updateSecessionMember(); break;
				case 6 : selectGender(); break;
				case 0 : System.out.println("메인 메뉴로 돌아갑니다."); break;
				default : System.out.println("잘못 입력하셨습니다.");
				}
				
			}catch(InputMismatchException e){
				System.out.println("숫자만 입력해주세요.");
				sel = -1;
				sc.nextLine();
			}
			
		}while(sel!=0);
	}
	

	/**
	 * 내 정보 조회 View
	 */
	private void selectMyInfo() {
		// DB에서 회원 정보를 조회할 필요 없이 
		// loginMember에 담겨있는 값을 이용해 출력
		System.out.println("[내 정보 조회]");
		System.out.println("회원번호 : " + loginMember.getMemNo());
		System.out.println("아이디 : " + loginMember.getMemId());
		System.out.println("이름 : " + loginMember.getMemNm());
		System.out.println("전화번호 : " + loginMember.getPhone());
		System.out.println("성별 : " + loginMember.getGender());
		System.out.println("가입일 : " + loginMember.getHireDt());
	}
	
	
	
	/**
	 * 검색어가 이름에 포함된 모든 회원 조회 View
	 */
	private void selectMemberName() {
//		1. 검색어를 입력 받기
		System.out.println("[검색어 포함 이름 검색]");
		System.out.print("검색어 입력 : ");
		String name = sc.nextLine();
		
//		2. Service에 알맞은 메소드 호출 후 결과 반환 받기
		try {
			List<Member> list = mService.selectMemberName(name);
			
//		3. 결과로 List<Member> 를 반환받고 모두 출력
			if(list.isEmpty()) { // 조회 결과가 없을 때 -> 검색에서 걸러지는 조건이 없으면 결과 : 0행
				System.out.println("조회 결과가 없습니다."); 
			} else {
				for(Member mem : list) {
//				System.out.println("회원 아이디 : " + mem.getMemId());
//				System.out.println("회원 이름 : " + mem.getMemNm());
//				System.out.println("전화번호 : " + mem.getPhone());
//				System.out.println("성별 : " + mem.getGender());
//				System.out.println("가입일 : " + mem.getHireDt());
		        System.out.printf("%s / %s / %s / %c / %s\n", 
			mem.getMemId(),mem.getMemNm(),mem.getPhone(),mem.getGender(),mem.getHireDt());
				}
			}
		}catch (Exception e) {
			System.out.println("검색어 포함 이름 검색 과정에서 오류 발생");
			e.printStackTrace();
		}
	}
	
	
	/**
	 * 내 정보 수정 View
	 */
	private void updateMyInfo() {
		System.out.println("[내 정보 수정]");
		
		System.out.print("이름 : ");
		String memNm = sc.nextLine();
		
		System.out.print("전화번호 : ");
		String phone = sc.nextLine();
		
		System.out.print("성별 : ");
		char gender = sc.nextLine().toUpperCase().charAt(0);
		
		// 입력받은 값 + 회원 번호를 저장할 멤버 객체 생성.
			
		Member upMember = new Member(loginMember.getMemNo(), memNm, phone, gender);
							      // 회원번호는 로그인 멤버에서 얻어옴 (Update where절에 사용)
		
		try {
		// 생성한 member객체를 매개변수로하여 Service 메소드 호출 및 결과 반환
			int result = mService.updateMyInfo(upMember);
		
			if(result>0) {
				System.out.println("내 정보 수정 성공"); 
				
				// loginMember는 수정 전 데이터를 가지고 있으므로
				// 수정 성공 시 데이터를 최신화 해야 함.
				loginMember.setMemNm(memNm);
				loginMember.setPhone(phone);
				loginMember.setGender(gender);
			}else {
				System.out.println("내 정보 수정 실패"); 
			}
			
		}catch (Exception e) {
			System.out.println("내 정보 수정 과정에서 오류 발생");
			e.printStackTrace();
		}
		
	}
	
	
	/**
	 * 비밀번호 변경 View
	 */
	    private void updatePw() {
		System.out.println("[비밀번호 변경]");
		System.out.print("현재 비밀번호 : ");
		String currPw = sc.nextLine();
		
		System.out.print("새 비밀번호 : ");
		String newPw = sc.nextLine();
		System.out.print("새 비밀번호 확인 : ");
		String newPw2 = sc.nextLine();
		
		// 새 비밀번호와 확인이 일치하는지 판별
		if(newPw.equals(newPw2)) { // 새 비밀번호가 같을 경우
		 // 현재 비밀번호와, 새 비밀번호 + 현재 로그인한 회원번호
			Member upMember = new Member();
			upMember.setMemNo(loginMember.getMemNo()); //회원번호
			upMember.setMemPw( currPw ); //입력받은 현재 비밀번호
			
			try {
				int result = mService.updatePw(upMember, newPw);
				
				if(result > 0) {
					System.out.println("비밀번호 변경 성공");
				}else {
					System.out.println("현재 비밀번호가 일치하지 않습니다.");
				}
				
			}catch (Exception e) {
				System.out.println("비밀번호 변경 과정에서 오류 발생");
				e.printStackTrace();
			}
		}else {
			System.out.println("변경하려는 비밀번호가 일치하지 않습니다.");
		}
		
	}
		
		
		
	/**
	 * 회원 탈퇴 View
	 */
	private void updateSecessionMember() {
		System.out.println("[회원 탈퇴]");
		
		System.out.print("비밀번호 입력 : ");
		String memPw = sc.nextLine(); // 비밀번호 일치 여부는 DB에서 확인
		
		System.out.print("**** 정말 삭제하시겠습니까??(Y/N) ****");
		char check = sc.nextLine().toUpperCase().charAt(0);
		
		if(check == 'Y') {
			// 삭제 진행
			try {
				Member upMember = new Member();
				upMember.setMemNo(loginMember.getMemNo()); // 로그인된 회원 번호
				upMember.setMemPw(memPw); // 비밀번호 셋팅
				
				int result = mService.updateSecessionMember(upMember);
	
				if(result>0) {
					System.out.println("탈퇴 되었습니다.....");
					loginMember = null;
				}else {
					System.out.println("비밀번호가 일치하지 않습니다.");
				}
				
			} catch (Exception e) {
				System.out.println("회원 탈퇴 과정에서 오류 발생");
				e.printStackTrace();
			}
		} else { 
			// 탈퇴 취소
			System.out.println("탈퇴가 취소되었습니다!^^");
		}
			
	}
		
	
	/**
	 * 성별 검색 View
	 */
	private void selectGender() {
		// 성별(M 또는 F, 대소문자 구분X)을 입력받아
		// DB에서 일치하는 회원을 모두 조회
		// 아이디, 이름, 전화번호 마지막자리 4개, 성별만 출력 
		System.out.println("[성별 검색]");
		System.out.print("성별(M 또는 F) 입력 >> ");
		char gender = sc.nextLine().toUpperCase().charAt(0);
		try {
			List<Member> list = mService.selectGender(gender);
		
			if(list.isEmpty()) {
				System.out.println("검색 결과가 없습니다.");
			}else {
				for(Member mem : list) {
					System.out.printf("%s / %s / %s / %c\n", 
							mem.getMemId(), mem.getMemNm(),
							mem.getPhone(), mem.getGender());
				}
			}
		
		}catch(Exception e) {
			System.out.println("검색 과정에서 오류 발생");
			e.printStackTrace();
		}
	
	}

}

```

```java
// MemberService.java
package com.kh.jdbc.member.model.sevice;

import static com.kh.jdbc.common.JDBCTemplate.*; 
// static import : 특정 static 필드, 메소드 호출 시 클래스명을 생략할 수 있게하는 구문.

import java.sql.Connection;
import java.util.List;

import com.kh.jdbc.common.JDBCTemplate;
import com.kh.jdbc.member.model.dao.MemberDAO;
import com.kh.jdbc.member.model.vo.Member;

public class MemberService {
	
	private MemberDAO mDAO = new MemberDAO();
		
	/** 검색어 포함 이름 검색 Service
	 * @param name
	 * @return list
	 * @throws Exception
	 */
	public List<Member> selectMemberName(String name) throws Exception {
//		1. Connection 생성
		Connection conn = getConnection();
		
//		2. DAO에서 알맞은 메소드 호출 후 결과 반환 받기
		List<Member> list = mDAO.selectMemberName(conn, name);
		
		// ** 전화번호 가운데를 ****로 치환하기
		// 010-1234-1234 --> 010-****-1234
		// split() 사용
		for(Member mem : list) {
			String[] arr = mem.getPhone().split("-");
			mem.setPhone(arr[0]+"-****-"+arr[2]);
		}
		
		
//		3. Connection 반환
		close(conn);
		
//		4. DAO 호출 결과를 그대로 View로 반환
		return list;
	}
	

	/**정보 수정용 Service
	 * @param upMember
	 * @return result
	 * @throws Exception
	 */
	public int updateMyInfo(Member upMember) throws Exception {
		Connection conn = getConnection();
		
		// 생성한 커넥션과 매개변수 upMember를 DAO메소드의 매개변수로 전달하고
		// 결과를 반환 받음
		int result = mDAO.updateMyInfo(conn, upMember);
		
		// UPDATE(DML)을 진행 하였으므로 트랜잭션 처리
		if(result>0) commit(conn);
		else		 rollback(conn);
		
		close(conn);
		
		return result;
	}


	// 비밀번호 변경용 Service
	public int updatePw(Member upMember, String newPw) throws Exception {
		Connection conn = getConnection();
		
		int result = mDAO.updatePw(conn, upMember, newPw);
		
		if(result>0) commit(conn);
		else		 rollback(conn);
		
		close(conn);
		
		return result;
		
	}


	/** 회원탈퇴 Service
	 * @param upMember
	 * @return result
	 * @throws Exception
	 */
	public int updateSecessionMember(Member upMember) throws Exception {
		Connection conn = getConnection();
		
		int result = mDAO.updateSecessionMember(conn, upMember);
		
		if(result>0) commit(conn);
		else		 rollback(conn);
		
		close(conn);
		
		return result;		
	}
	

	// 성별 검색용 Service
	public List<Member> selectGender(char gender) throws Exception {
		Connection conn = getConnection(); 
		
		List<Member> list = mDAO.selectGender(conn, gender);
		
		//SelectGender1 사용시
		
		//for(Member mem : list) {
		  // 1)split() 사용
		  //String[] arr = mem.getPhone().split("-");
		  //mem.setPhone(arr[2]);
		
		  // 2) subString() 사용
		  //String ph = mem.getPhone();
		  //mem.setPhone(ph.substring(ph.lastIndexOf("-")
		//}
		
		//SelectGender2 사용시 다른 로직 필요 없음
				
		close(conn);
		return list;
	}

}
```

```java
//MemberDAO.java
package com.kh.jdbc.member.model.dao;

import static com.kh.jdbc.common.JDBCTemplate.close;

import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Properties;

import com.kh.jdbc.member.model.vo.Member;

public class MemberDAO {

	// DAO에서 자주 사용하는 JDBC객체 참조변수를 멤버 변수로 선언 --> 변수 재활용
	private Statement stmt = null;
	private PreparedStatement pstmt = null;
	private ResultSet rset = null;
	
	// 외부 xml파일에 작성된 SQL구문을 읽어들일 Properties 변수 선언 
	private Properties prop = null;
	
	public MemberDAO() { // 생성자로 작성을 해 놓아서 클래스가 시작하자마자,,실행됨 ->member-query.xml의 내용이 prop에 저장됨
		// 기본 생성자를 통한 객체 생성 시 xml파일을 읽어오게 함.
		try {
			prop = new Properties();
			prop.loadFromXML(new FileInputStream("member-query.xml"));
		}catch(Exception e) {
			e.printStackTrace();
		}
	}
				
	/** 검색어 포함 이름 검색 DAO
	 * @param conn
	 * @param name
	 * @return list
	 * @throws Exception
	 */
	public List<Member> selectMemberName(Connection conn, String name) throws Exception {
//		1. 반환할 결과를 저장할 변수 List<Member> 선언
		List<Member> list = null;
		try{

//			2. SQL 작성
			String query = prop.getProperty("selectMemberName");
//			3. PreparedStatement 객체 생성
			pstmt = conn.prepareStatement(query);
//			4. 위치홀더(?)에 알맞은 값 배치
			pstmt.setString(1, name);
//		    5. SQL 수행 후 결과(ResultSet)를 반환 받음
			rset = pstmt.executeQuery();
//			6. 수행 결과를 모두 List에 담기
			list = new ArrayList<Member>();
			
			while(rset.next()) {
				list.add(new Member(rset.getString("MEM_ID"), 
									rset.getString("MEM_NM"), 
									rset.getString("PHONE"), 
									rset.getString("GENDER").charAt(0), 
									rset.getDate("HIRE_DT")) );
			}
		} finally {
//		7. 사용한 JDBC 객체 반환
			close(rset);
			close(pstmt);
		}
//		8. 조회 결과가 담긴 List 반환
		return list;
	}
	

	// 회원 정보 수정용 DAO
	public int updateMyInfo(Connection conn, Member upMember) throws Exception {
		int result =0; //DB 수행 결과를 저장할 변수 선언
		
		try {
			String query = prop.getProperty("updateMyInfo");
			
			// PreparedStatment 객체 생성 후 위치홀더에 알맞은 값 배치
			pstmt = conn.prepareStatement(query);
			
			pstmt.setString(1, upMember.getMemNm());
			pstmt.setString(2, upMember.getPhone());
			pstmt.setString(3, upMember.getGender()+"");
			pstmt.setInt(4, upMember.getMemNo());
			
			//SQL 수행 후 결과를 result에 저장
			
			result = pstmt.executeUpdate();
		}finally {
			close(pstmt);
		}
		return result;
	}

	
	
	/** 비밀번호 변경 DAO
	 * @param conn
	 * @param upMember
	 * @param newPw
	 * @return result
	 * @throws exception
	 */
	public int updatePw(Connection conn, Member upMember, String newPw) throws Exception {
		int result =0;
		
		try {
			String query = prop.getProperty("updatePw");
			
		    pstmt = conn.prepareStatement(query);
		    
		    pstmt.setString(1,newPw);
		    pstmt.setInt(2,upMember.getMemNo());
		    pstmt.setString(3,upMember.getMemPw());
		    
		    result = pstmt.executeUpdate();
		}finally {
			close(pstmt);
		}
		return result;
	}

	
	/**회원탈퇴 DAO
	 * @param conn
	 * @param upMember
	 * @return result
	 * @throws Exception
	 */
	public int updateSecessionMember(Connection conn, Member upMember) throws Exception {
		int result =0;

		try {
			String query = prop.getProperty("updateSecessionMember");

			pstmt = conn.prepareStatement(query);

			pstmt.setInt(1,upMember.getMemNo());
			pstmt.setString(2,upMember.getMemPw());

			result = pstmt.executeUpdate();
		}finally {
			close(pstmt);
		}
		return result;
	}
		

	
	// 성별 검색용 DAO
	public List<Member> selectGender(Connection conn, char gender) throws Exception {
		List<Member> list = null;
		try {
			
			//selectGender1 사용시
			//String query = prop.getProperty("selectGender1");
			
			//selectGender2 사용시
			String query = prop.getProperty("selectGender2");
			
			pstmt = conn.prepareStatement(query);
			
			pstmt.setString(1, gender+"");

			rset = pstmt.executeQuery();

			list = new ArrayList<Member>();

			while(rset.next()) {
				list.add(new Member(rset.getString(1), rset.getString(2), 
						rset.getString("SUBSTR(PHONE,10)"), rset.getString(4).charAt(0)) );
						// selectGender2를 SQL문으로 쓸 경우 PHONE은  SELECT문에서 잘려져있음..
						// 컬럼명 바뀌어져있는거임,, 컬럼 순서를 쓰던가 바뀐 이름 작성해야함
			}
		}finally {
			close(rset);
			close(pstmt);
		}
		return list;
	}


}


```
```java
// 사용한 xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">

<!--검색어 포함 이름 검색  -->
<entry key ="selectMemberName">
SELECT MEM_ID, MEM_NM, PHONE, GENDER, HIRE_DT  
FROM TB_MEMBER 
WHERE MEM_NM LIKE '%' || ? || '%' 
AND SCSN_FL = 'N'
</entry>

<!--내 정보 수정-->
<entry key ="updateMyInfo">
UPDATE TB_MEMBER
SET MEM_NM =?, PHONE = ?, GENDER = ?
WHERE MEM_NO = ?
</entry>

<!--비밀번호 변경-->
<entry key ="updatePw">
UPDATE TB_MEMBER
SET MEM_PW = ?
WHERE MEM_NO = ?
AND MEM_PW = ?
</entry>

<!--회원 탈퇴-->
<entry key ="updateSecessionMember">
UPDATE TB_MEMBER
SET SCSN_FL = 'Y'
WHERE MEM_NO = ?
AND MEM_PW = ?
</entry>

<!--성별 검색1 -->
<entry key ="selectGender1">
SELECT MEM_ID, MEM_NM, PHONE, GENDER
FROM TB_MEMBER 
WHERE GENDER = ? 
AND SCSN_FL = 'N'
</entry>

<!--성별 검색 2-->
<entry key ="selectGender2">
SELECT MEM_ID, MEM_NM, SUBSTR(PHONE,10), GENDER
FROM TB_MEMBER 
WHERE GENDER = ? 
AND SCSN_FL = 'N'
</entry>

</properties>
```


<br/><br/>