---
title: "2020년 11월 21일"
excerpt: "JDBC 과제 : 단군마켓 "
search: true
categories: 
  - Parctice
  - Academy
tags: 
  - JAVA
  - JDBC
  - TIL
  - 단군마켓
  - 보령, 혜윤, 만희 팀플
toc: true
---

## JDBC
### 단군마켓 Project
> 중고거래를 하는 마켓.<br/>
  {: .notice--success}


```java
//JurrTemplate
package manat.jurr.common;

import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.Properties;

public class JurrTemplate {
	
		private static Connection conn =null; 
	
		private JurrTemplate() {}
	
		public static Connection getConnection() {
			try {
				if(conn == null || conn.isClosed()) {
		
				Properties prop = new Properties();
				
				
				prop.loadFromXML(new FileInputStream("driver.xml"));
				
			
				Class.forName(prop.getProperty("driver"));
				conn = DriverManager.getConnection(prop.getProperty("url"), prop.   getProperty("user"), prop.getProperty("password"));
				
				
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
//JurrProperties.java
package manat.jurr.common;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.util.Properties;

public class JurrProperties {
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

}
```

```java
// 사용한 xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>

<!-- 쿼리 작성하는 xml ~~!!!!!-->


<!--  만희 -->
<entry key="signUp">
INSERT INTO MEMBER
VALUES(SEQ_MNO.NEXTVAL,? ,? , ? , ? ,? ,DEFAULT,DEFAULT,DEFAULT,DEFAULT)
</entry>

<entry key="login">
SELECT MEM_NO, MEM_ID, MEM_NM, PHONE, DANKUN_LV, POINT
FROM MEMBER
WHERE MEM_ID = ?
AND MEM_PW = ?
AND SCSN_FL = 'N'
</entry>

<entry key="registerProduct">
INSERT INTO PRODUCT(ITEM_NO, ITEM_NM, PRICE, KIND, ADDRESS, CONTENT, MEM_NO, DELETE_FL)
VALUES(SEQ_INO.NEXTVAL, ?, ?, ?, ?, ?, ?, DEFAULT)
</entry>


<!-- 혜윤 -->
<!-- 모든 판매글 조회 -->
<entry key="selectAllItem">
SELECT * FROM V_PRODUCT
WHERE DELETE_FL = 'N'
ORDER BY ITEM_NO DESC
</entry>

<!-- 검색으로 판매글 조회 -->
<entry key="selectSearchItem">
SELECT * FROM V_PRODUCT
WHERE ITEM_NO = ?
AND DELETE_FL = 'N'
</entry>

<!-- 상품번호로 상품 구매 -->
<entry key="purchaseItem">
UPDATE PRODUCT
SET DELETE_FL = 'Y'
WHERE ITEM_NO = ?
</entry>

<!-- 포인트 적립 -->
<entry key="earnPoints">
UPDATE MEMBER
SET POINT = POINT + 5
WHERE MEM_NO IN 
        (SELECT MEM_NO
        FROM PRODUCT
        WHERE ITEM_NO = ?)

</entry>



<!-- 보령  -->

<!-- 내가 쓴 글 전체 조회 -->
<entry key="selectMyBoard">
SELECT ITEM_NO, MEM_ID, ITEM_NM, PRICE, KIND 
FROM V_PRODUCT
WHERE MEM_ID = ?
AND DELETE_FL = 'N'
</entry>


<!--  내가 쓴 글 수정 -->
<entry key="updateMyBoard">
UPDATE PRODUCT
SET ITEM_NM = ?, PRICE = ?, CONTENT =?
WHERE ITEM_NO= ?
</entry>
 
 
 <!-- 내가 쓴 글 삭제 -->
 <entry key="deleteMyBoard">
 UPDATE PRODUCT
 SET DELETE_FL = 'Y'
 WHERE ITEM_NO = ?
 </entry>

 <!--내 게시글인지 확인용-->
<entry key="checkMyBoard">
SELECT COUNT(*)
FROM PRODUCT
WHERE MEM_NO = ?
AND ITEM_NO =?
AND DELETE_FL = 'N'
</entry>

<!--나의 단군_LV-->
<entry key="myDanKunLV">
SELECT MEM_NM, POINT, DANKUN_LV
FROM MEMBER
WHERE MEM_NO = ?
</entry>


<!--나의 단군_LV 업데이트-->
<entry key="updateDanKunLV">
UPDATE MEMBER
SET DANKUN_LV = ?
WHERE MEM_NO = ?
</entry>



<!-- 회원 정보 수정 -->
<entry key="updateMyInfo">
UPDATE MEMBER SET
MEM_NM = ?,
PHONE = ?,
ADDRESS = ?
WHERE MEM_NO =?
</entry>



<!-- 회원 탈퇴 -->
<entry key="updateSecessionMember">
UPDATE MEMBER SET
SCSN_FL = 'Y'
WHERE MEM_NO = ?
AND MEM_PW = ?
</entry>



</properties>
```


```java
//Member.java
package manat.jurr.model.vo;

import java.sql.Date;

public class Member {
	   private int memNo;
	   private String memId;
	   private String memPw;
	   private String memNm;
	   private String phone;
	   private String address;
	   private Date signUp;
	   private char scsn;
	   private String dankunLv;
	   private int point;
	 

	   public Member() {}
	   
	   
	   
	   
	   
	   //로그인용 생성자 
	   public Member(int memNo, String memId, String memNm, String phone, String dankunLv, int point) {
		   super();
		   this.memNo = memNo;
		   this.memId = memId;
		   this.memNm = memNm;
		   this.phone = phone;
		   this.dankunLv = dankunLv;
		   this.point = point;
	   }
	   
	   // 
	   public Member(String memId, String memPw, String memNm, String phone, String address) {
			super();
			this.memId = memId;
			this.memPw = memPw;
			this.memNm = memNm;
			this.phone = phone;
			this.address = address;
		}

	   
	   // 단군 레벨 조회용 생성자
	   public Member(String memNm,int point, String dankunLv) {
		super();
		this.memNm = memNm;
		this.point = point;
		this.dankunLv = dankunLv;
	}



	// 회원 정보 수정용 생성자
	   public Member(int memNo, String memNm, String phone, String address) {
		super();
		this.memNo = memNo;
		this.memNm = memNm;
		this.phone = phone;
		this.address = address;
	}




	public Member(int memNo, String memId, String memPw, String memNm, String phone, String address, Date signUp,
			char scsn, String dankunLv, int point) {
		super();
		this.memNo = memNo;
		this.memId = memId;
		this.memPw = memPw;
		this.memNm = memNm;
		this.phone = phone;
		this.address = address;
		this.signUp = signUp;
		this.scsn = scsn;
		this.dankunLv = dankunLv;
		this.point = point;
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


	public String getAddress() {
		return address;
	}


	public void setAddress(String address) {
		this.address = address;
	}


	public Date getSignUp() {
		return signUp;
	}


	public void setSignUp(Date signUp) {
		this.signUp = signUp;
	}


	public char getScsn() {
		return scsn;
	}


	public void setScsn(char scsn) {
		this.scsn = scsn;
	}


	public String getDankunLv() {
		return dankunLv;
	}


	public void setDankunLv(String dankunLv) {
		this.dankunLv = dankunLv;
	}


	public int getPoint() {
		return point;
	}


	public void setPoint(int point) {
		this.point = point;
	}

	@Override
	public String toString() {
		return "Member [memNo=" + memNo + ", memId=" + memId + ", memPw=" + memPw + ", memNm=" + memNm + ", phone="
				+ phone + ", address=" + address + ", signUp=" + signUp + ", scsn=" + scsn + ", dankunLv=" + dankunLv
				+ ", point=" + point + "]";
	}
	   
	   
}

```
```java
//Product.java
package manat.jurr.model.vo;

public class Product {
	
	private int itemNo;
	private String itemNm;
	private int price;
	private String kind;
	private String address;
	private String content;
	private int memNo;
	private char deleteFl;	
	
	public Product() {}
	
	  // 김마리
	 //상품 등록용 생성자
	public Product(String itemNm, int price, String kind, String address, String content, int memNo) {
		super();
		this.itemNm = itemNm;
		this.price = price;
		this.kind = kind;
		this.address = address;
		this.content = content;
		this.memNo = memNo;
	}



	
	// 게시글 수정용,,
	public Product(String itemNm, int price, String content, int itemNo) {
		super();
		this.itemNm = itemNm;
		this.price = price;
		this.content = content;
		this.itemNo = itemNo;
	}


	public Product(int itemNo, String itemNm, int price, String kind, String address, String content, int memNo,
			char deleteFl) {
		super();
		this.itemNo = itemNo;
		this.itemNm = itemNm;
		this.price = price;
		this.kind = kind;
		this.address = address;
		this.content = content;
		this.memNo = memNo;
		this.deleteFl = deleteFl;
	}


	public int getItemNo() {
		return itemNo;
	}


	public void setItemNo(int itemNo) {
		this.itemNo = itemNo;
	}


	public String getItemNm() {
		return itemNm;
	}


	public void setItemNm(String itemNm) {
		this.itemNm = itemNm;
	}


	public int getPrice() {
		return price;
	}


	public void setPrice(int price) {
		this.price = price;
	}


	public String getKind() {
		return kind;
	}


	public void setKind(String kind) {
		this.kind = kind;
	}


	public String getAddress() {
		return address;
	}


	public void setAddress(String address) {
		this.address = address;
	}


	public String getContent() {
		return content;
	}


	public void setContent(String content) {
		this.content = content;
	}


	public int getMemNo() {
		return memNo;
	}


	public void setMemNo(int memNo) {
		this.memNo = memNo;
	}


	public char getDeleteFl() {
		return deleteFl;
	}


	public void setDeleteFl(char deleteFl) {
		this.deleteFl = deleteFl;
	}


	@Override
	public String toString() {
		return "Product [itemNo=" + itemNo + ", itemNm=" + itemNm + ", price=" + price + ", kind=" + kind + ", address="
				+ address + ", content=" + content + ", memNo=" + memNo + ", deleteFl=" + deleteFl + "]";
	}

	

}

```
```java
//VProduct.java
package manat.jurr.model.vo;

public class VProduct {

	private String memId;
	private String itemNm;
	private int price;
	private String kind;
	private String address;
	private int itemNo;
	private String content;
	private char deleteFl;
	
	public VProduct() {
		// TODO Auto-generated constructor stub
	}
	
	public VProduct(String memId, String itemNm, int price, String kind, String address, int itemNo, String content,
			char deleteFl) {
		super();
		this.memId = memId;
		this.itemNm = itemNm;
		this.price = price;
		this.kind = kind;
		this.address = address;
		this.itemNo = itemNo;
		this.content = content;
		this.deleteFl = deleteFl;
	}
	
	//게시글 조회용 생성자
		public VProduct(int itemNo, String memId, String itemNm, int price, String kind) {
			super();
			this.itemNo = itemNo;
			this.memId = memId;
			this.itemNm = itemNm;
			this.price = price;
			this.kind = kind;
		}

	//상품 전체 조회용 생성자
	public VProduct(String memId, String itemNm, int price, String kind, int itemNo) {
		super();
		this.memId = memId;
		this.itemNm = itemNm;
		this.price = price;
		this.kind = kind;
		this.itemNo = itemNo;
	}

	//상세 상품 조회용 생성자
	public VProduct(String memId, String itemNm, int price, String kind, String address, int itemNo, String content) {
		super();
		this.memId = memId;
		this.itemNm = itemNm;
		this.price = price;
		this.kind = kind;
		this.address = address;
		this.itemNo = itemNo;
		this.content = content;
		
	}
	
	public String getMemId() {
		return memId;
	}
	
	public void setMemId(String memId) {
		this.memId = memId;
	}

	public String getItemNm() {
		return itemNm;
	}

	public void setItemNm(String itemNm) {
		this.itemNm = itemNm;
	}

	public int getPrice() {
		return price;
	}

	public void setPrice(int price) {
		this.price = price;
	}

	public String getKind() {
		return kind;
	}

	public void setKind(String kind) {
		this.kind = kind;
	}

	public String getAddress() {
		return address;
	}

	public void setAddress(String address) {
		this.address = address;
	}

	public int getItemNo() {
		return itemNo;
	}

	public void setItemNo(int itemNo) {
		this.itemNo = itemNo;
	}

	public String getContent() {
		return content;
	}

	public void setContent(String content) {
		this.content = content;
	}

	public char getDeleteFl() {
		return deleteFl;
	}

	public void setDeleteFl(char deleteFl) {
		this.deleteFl = deleteFl;
	}

	@Override
	public String toString() {
		return "VProduct [memId=" + memId + ", itemNm=" + itemNm + ", price=" + price + ", kind=" + kind + ", address="
				+ address + ", itemNo=" + itemNo + ", content=" + content + ", deleteFl=" + deleteFl + "]";
	}

	
}
```

<br/><br/>