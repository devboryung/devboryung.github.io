---
title: "2020년 11월 17일"
excerpt: "JDBC-2"
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
### Template
1. 반복되는 Connection 객체의 생성을 간소화하기 위해 사용한다.
2. 트랜잭션 처리, close() 처리의 간소화하기 위해 사용한다.
{: .notice--success}
<br/>

### Properties 클래스
키와 값이 모두 String인 컬렉션
외부 파일 입출력이 간단하게 구현되어 있음.
{: .notice--success}
<br/>


```java
//JDBCRun.java
package com.kh.jdbc.run;

import com.kh.jdbc.View.JDBCView;

public class JDBCRun {
	
	public static void main(String[] args) {
	
		new JDBCView().displayMain();
		
	}
}

```

```java
//Membervo.java
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
	
	/**
	 * 회원가입용 생성자
	 */
	public Member(String memId, String memPw, String memNm, String phone, char gender) {
		super();
		this.memId = memId;
		this.memPw = memPw;
		this.memNm = memNm;
		this.phone = phone;
		this.gender = gender;
	}

	/**
	 * 로그인용 생성자
	 */
	public Member(int memNo, String memId, String memNm, String phone, char gender, Date hireDt) {
		super();
		this.memNo = memNo;
		this.memId = memId;
		this.memNm = memNm;
		this.phone = phone;
		this.gender = gender;
		this.hireDt = hireDt;
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
	// 하나의 공용 커넥션 참조변수 선언
	private static Connection conn =null; 
	
	// 기본 생성자
	private JDBCTemplate() {}

	// DB 연결을 위한 Connection 객체를 간접적으로 얻어가는 메소드를 생성
	public static Connection getConnection() {
		try {
			if(conn == null || conn.isClosed()) {

			Properties prop = new Properties();
			// driver.xml 파일에 작성된 entry를 prop에 저장(entry = 키,값 한 쌍)
			prop.loadFromXML(new FileInputStream("driver.xml"));
			
			// JDBC 드라이버 등록 및 커넥션 얻어오기
			Class.forName(prop.getProperty("driver"));
			conn = DriverManager.getConnection(prop.getProperty("url"), prop.getProperty("user"), prop.getProperty("password"));
			
			// 자동  commit 비활성 화
			conn.setAutoCommit(false);
			}
			
		}catch(Exception e) {
			e.printStackTrace();
		}
		return conn;
	}
	
	
	// 트랜잭션 처리(commit, rollback)도 공통적으로 사용함
	// static으로 미리 선언 코드길이 감소 효과 + 재사용성
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
	
	// DB 연결 자원 반환 구문도 static으로 작성
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
		
//		 값 세팅
		prop.setProperty("driver", "oracle.jdbc.driver.OracleDriver");
		prop.setProperty("url","jdbc:oracle:thin:@localhost:1521:xe");
		prop.setProperty("user", "jdbc");
		prop.setProperty("password","jdbc");
		
		// XML의 장점 : 모든 프로그래밍 언어에서 입출력 가능한 파일
		try { //storeToXML -> XML에 자료 쌓는다
			prop.storeToXML(new FileOutputStream("driver.xml"), "driver information");
			
		}catch (Exception e) {
		}

		// 값 꺼내기
		// 외부 XML 파일 읽어오기
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
import java.util.Scanner;
import com.kh.jdbc.member.model.sevice.MemberService;
import com.kh.jdbc.member.model.vo.Member;

/** JDBCProject View
 * @author 강보령
 */
public class JDBCView {
	
	private Scanner sc = new Scanner(System.in);
	private MemberService mService = new MemberService();
	
	private Member loginMember = null; // 로그인 된 회원의 정보를 저장.
	

	/**
	 * 메인 메뉴  View
	 */
	public void displayMain() {
		int sel = 0;
				
		do {
			try { 
				if(loginMember == null) { // 로그인이 되어있지 않은 경우.
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
				}else { //로그인이 된 경우
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
					case 1:  break;
					case 9: loginMember = null; //loginMember = 로그인 된 회원 정보 저장되어있는것
							System.out.println("★로그아웃 되었습니다★\n");
							break;
					case 0: System.out.println("프로그램 종료"); break;
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
	 *  회원 가입용  View
	 */
	private void signUp() {
		System.out.println("[회원 가입]");
		
		System.out.print("아이디 : ");
		String memId = sc.nextLine();
		
		System.out.print("비밀번호 : ");
		String memPw = sc.nextLine();
		
		System.out.print("이름 : ");
		String memNm = sc.nextLine();
		
		System.out.print("전화번호 : ");
		String phone = sc.nextLine();
		
		System.out.print("성별 : ");
		char gender = sc.nextLine().toUpperCase().charAt(0); // 대문자로 입력 받음
		
		// 입력 받은 값을 하나의 Member 객체에 저장
		Member newMember = new Member(memId, memPw, memNm, phone, gender);
		
		// 입력 받은 회원 정보를 DB에 삽입하기 위한 service 호출
		try {
			int result = mService.sighUp(newMember);
			if (result > 0) {
				System.out.println("회원 가입 성공!!");
				System.out.println(); 
			}else {
				System.out.println("회원 가입 실패!!");
				System.out.println();
			}
		}catch(Exception e) {
			System.out.println("회원 가입 과정에서 오류가 발생했습니다.");
			e.printStackTrace();
		}
		
	}
	
	/**
	 * 로그인용 View
	 */
	private void login() {
		System.out.println("===[로그인]===");
		
		System.out.print("아이디 : ");
		String memId = sc.nextLine();
		
		System.out.print("비밀번호 : ");
		String memPw = sc.nextLine();
		
		// 아이디, 비밀번호를 하나의 Member 객체에 저장
		Member member = new Member();
		member.setMemId(memId);
		member.setMemPw(memPw);
	
		try {
			// 입력받은 ID, PW를 이용하여 로그인 서비스 수행
			//  -> 반환 받은 결과를 loginMember에 저장
			loginMember = mService.login(member);

			// 로그인 성공
			if(loginMember != null) {
				System.out.println("*****" + loginMember.getMemNm()+ "님 환영합니다*****\n");
			} else {
				System.out.println("아이디 또는 비밀번호가 일치하지 않거나, 탈퇴한 회원입니다.");
			}
		
		} catch (Exception e) {
			System.out.println("로그인 과정에서 오류 발생"); 
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

import com.kh.jdbc.common.JDBCTemplate;
import com.kh.jdbc.member.model.dao.MemberDAO;
import com.kh.jdbc.member.model.vo.Member;

public class MemberService {
	
	private MemberDAO mDAO = new MemberDAO();
	
	/**
	 * 회원가입용 Service
	 */
	public int sighUp(Member newMember) throws Exception {
		
		// Connection 생성 <- 서비스의 첫번째~! -> JDBCTemplate에서 커넥션 얻어오기
		Connection conn = getConnection();
		
		// 요청을 처리할 수 있는 알맞은 DAO메소드를 호출하여 
		// 커넥션과 메개변수를 전달하고 결과를 반환 받음.
		
		int result = mDAO.signUp(conn, newMember);
		
		//  result 값에 따른 트랜잭션 처리  --> DAO에서 insert 함 (DML)
		
		if (result>0) commit(conn);
		else 		  rollback(conn);
		
		// conn 반환
		close(conn); // <-여기서 에러난거면 위에 import구문에 *써있는지 확인해보기.
		
		// DB 삽입 결과를 반환.
		return result;
	}
	
	
	/**
	 * 로그인용 service
	 */
	public Member login(Member member) throws Exception {
		// 커넥션 객체 얻어오기
		Connection conn = getConnection();
		
		// 얻어온 커넥션과 매개변수를 DAO로 전달한다.
		Member loginMember = mDAO.login(conn,member);
		
		// select는 트랜잭션 처리가 필요 없으므로 바로 커넥션 반환
		close(conn);
		
		return loginMember;
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
	
	/** 회원 가입용 DAO 
	 * @param conn
	 * @param newMember
	 * @return  result
	 * @throws Exception
	 */
	public int signUp(Connection conn, Member newMember) throws Exception {
		
		int result =0; // DML 수행 결과 저장용 변수
		
		try {
			String query = prop.getProperty("signUp");
			
			// SQL 구문을 DB 전달할 준비
			pstmt = conn.prepareStatement(query);
			pstmt.setString(1, newMember.getMemId());
			pstmt.setString(2, newMember.getMemPw());
			pstmt.setString(3, newMember.getMemNm());
			pstmt.setString(4, newMember.getPhone());
			pstmt.setString(5, newMember.getGender()+""); // gender는 char 자료형 -> char데이터에 빈 문자열을 더하여 String 형태로 형변환
			
			// SQL 수행 후 결과를 반환받아 result에 저장
			result = pstmt.executeUpdate();
		}finally {
			// catch가 없어도 finally 작성 가능함
			// DB 자원 반환
			close(pstmt);
		}
		return result;
	}


	
	public Member login(Connection conn, Member member) throws Exception {
		
		Member loginMember = null; // 로그인된 회원 정보를 저장할 임시 변수
		
		try {
			// member-query.xml 파일에서 키값이 "login"인 태그의 겂을 얻어옴.
			String query = prop.getProperty("login");
		
			// DB 전달을 위한 pstmt 생성
			pstmt = conn.prepareStatement(query);
			
			// SQL 구문의 위치홀더에 알맞는 값을 배치
			pstmt.setString(1, member.getMemId());
			pstmt.setString(2, member.getMemPw());
			
			// SQL 수행 후 결과를 ResultSet으로 반환 받는다.
			
			rset = pstmt.executeQuery();
		
			// 조회 결과가 있는지 확인하는 if문 작성
			if(rset.next()) {
				// 조회 결과가 있을 경우 Member 객체를 만들어 로그인 정보를 저장
				loginMember = new Member(rset.getInt("MEM_NO"), rset.getString("MEM_ID"), rset.getString("MEM_NM"), 
										rset.getString("PHONE"), rset.getString("GENDER").charAt(0), rset.getDate("HIRE_DT") );
			}
		}finally {
			// DB 자원 반환
			close(rset);
			close(pstmt);
		}
		return loginMember;
	}

}

```

<br/><br/>