---
title: "2020년 02월 02일"
excerpt: "Spring 2"
search: true
categories: 
  - Academy
tags: 
  - FrameWork
  - Java
  - Spring
toc: true
---

## DB연결
Connection은 DB와 연결하기 위한 정보들을 담고있음(url, userName, password...) <br><br>

자바와 DB를 연결하기 위해 <br>
1. JDBC 기술이 필요 (ojdbc6.jar 라이브러리가 필요하다.)
2. 마이바티스를 사용하기 위해 마이바티스도 필요하다.
3. 스프링에서 JDBC를 쓰기 위한 라이브러리
4. 스프링에서 마이바티스를 쓰기 위한 라이브러리
DB와 연결을 위해서 총 4개의 라이브러리가 필요함.<br><br>

Maven을 이용해서 라이브러리를 쉽게 받아올 수 있음.
<br>

[![메이븐](https://user-images.githubusercontent.com/73421820/106374951-16628b80-63cb-11eb-9806-e0ab9295303d.png)](https://maven.apache.org/index.html "Maven 공식홈페이지") <br>
👆🏼 이미지 클릭 시 공식 홈페이지로 이동

<br><br>

 https://mvnrepository.com/ 에 접속
1. spring-jdbc 검색 -> Spring JDBC 클릭 -> (버전은 스프링 버전에 맞추는게 좋다)5.2.8.RELEASE 클릭 -> 
Maven안에 있는 코드 복사해서 pom.xml의 dependencies태그 안에  붙여넣기

2. mybatis spring 검색 -> MyBatis Spring 클릭 -> 2.0.5 클릭 -> Maven안에 있는 코드 복사해서 pom.xml의 dependencies태그 안에  붙여넣기 

3. Mybatis 검색 -> Mybatis 클릭 -> 3.5.6 클릭 -> Maven안에 있는 코드 복사해서 pom.xml의 dependencies태그 안에  붙여넣기 

4. ojdbc6 검색 -> 3번째에 있는 Ojdbc6 클릭 -> 11.2.0.3 클릭 ->  Maven안에 있는 코드 복사해서 pom.xml의 dependencies태그 안에  붙여넣기 <br>
-> dependency에 에러 발생 -> maven에 보면 Note: this artifact is located at Datanucleus repository (http://www.datanucleus.org/downloads/maven2/) 라고 써져있음 ->
저장소가 옮겨짐.. -> 이 저장소에소 다운로드 받아올 수 있게 pom.xml에 외부 저장소를 추가해야됨(http://www.datanucleus.org/downloads/maven2/ 복사하기) -> 
dependencies 태그 바깥에 repositories 태그 생성해서 복사해둔 url 넣어주기

5. 커넥션 풀 라이브러리 다운로드 <br>
따로 Server 폴더의 context.xml에 db정보를 적지 않아도 된다.
maven 홈페이지 -> commons dbcp2 -> Apache Commons DBCP 클릭 -> 2.7.0 클릭 ->Maven안에 있는 코드 복사해서 pom.xml의 dependencies태그 안에  붙여넣기 

<br><br>

## pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.kh</groupId>
	<artifactId>spring</artifactId>
	<name>springProject</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>
	
	
	<!-- properties : 메이븐이 적용된 프로젝트에서 공종턱으로 사용할 버전 또는 설정값을 작성하는 태그  -->
	<properties>
		<java-version>1.6</java-version>
		<org.springframework-version>5.2.8.RELEASE</org.springframework-version>
		<org.aspectj-version>1.9.4</org.aspectj-version>
		<org.slf4j-version>1.7.25</org.slf4j-version>
	</properties>
	
	<!-- 외부 저장소 추가 태그  -->
	<repositories>
		<repository>
			<id>OJDBC6 Repository</id> <!-- 식별자(자유롭게 작성 가능)  -->
			<url>http://www.datanucleus.org/downloads/maven2/</url>
		</repository>
	</repositories>
	
	
	
	<!-- dependencies : Maven 프로젝트는 외부 저장소와 의존 관계를 맺고 있어 
			프로젝트에 필요한 파일을(라이브러리) 사용자가 직접 받을 필요 없이 해당 태그 내에 지정된 형식으로 작성하면
			네트워크를 통해 외부 저장소에서 자동으로 얻어와 세팅함.   -->
	<dependencies>
		<!-- GSON 라이브러리  -->
		<!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
		<dependency>
		    <groupId>com.google.code.gson</groupId>
		    <artifactId>gson</artifactId>
		    <version>2.8.6</version>
		</dependency>
		
		
		<!-- Spring - jdbc 연결 모듈  -->
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
		<dependency>
		    <groupId>org.springframework</groupId>
		    <artifactId>spring-jdbc</artifactId>
		    <version>5.2.8.RELEASE</version>
		</dependency>
		
		
		<!-- Spring - Mybatis 연결 모듈 -->
		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
		<dependency>
		    <groupId>org.mybatis</groupId>
		    <artifactId>mybatis-spring</artifactId>
		    <version>2.0.5</version>
		</dependency>
				
				
		<!-- Mybatis 라이브러리  -->		
		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
		<dependency>
		    <groupId>org.mybatis</groupId>
		    <artifactId>mybatis</artifactId>
		    <version>3.5.6</version>
		</dependency>
				
		<!-- oracle jdbc 라이브러리  -->		
		<!-- https://mvnrepository.com/artifact/oracle/ojdbc6 -->
		<dependency>
		    <groupId>oracle</groupId>
		    <artifactId>ojdbc6</artifactId>
		    <version>11.2.0.3</version>
		</dependency>
				
				
		<!-- 커넥션 풀 라이브러리  -->		
        <!-- 커넥션 풀 : 웅덩이에 커넥션을 일정 개수 만큼 만들어 놓고, 사용자가 요청하면 줬다가 다시 반환받고
	    요청이 올 때마다 커넥션을 새로 만드는 번거로운 과정 없어짐.
	    오류로 인해 반환을 못 받는 경우가 생길 수 가 있음, 그럴 경우를 대비해서 세션에 문제가 생기면 자동으로 반환을 함. -->
		<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-dbcp2 -->
		<dependency>
		    <groupId>org.apache.commons</groupId>
		    <artifactId>commons-dbcp2</artifactId>
		    <version>2.7.0</version>
		</dependency>
		
		
	
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${org.springframework-version}</version>
			<exclusions>
				<!-- Exclude Commons Logging in favor of SLF4j -->
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
				 </exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
				
		<!-- AspectJ -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${org.aspectj-version}</version>
		</dependency>	
		
		<!-- Logging -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${org.slf4j-version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jcl-over-slf4j</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.15</version>
			<exclusions>
				<exclusion>
					<groupId>javax.mail</groupId>
					<artifactId>mail</artifactId>
				</exclusion>
				<exclusion>
					<groupId>javax.jms</groupId>
					<artifactId>jms</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jdmk</groupId>
					<artifactId>jmxtools</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jmx</groupId>
					<artifactId>jmxri</artifactId>
				</exclusion>
			</exclusions>
			<scope>runtime</scope>
		</dependency>

		<!-- @Inject -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>
				
		<!-- Servlet -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1</version>
			<scope>provided</scope>
		</dependency>
		<!-- maven에서 제공하는 JSTL이 정상 적용이 되지 않아 삭제  -->
<!-- 		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency> -->
	
		<!-- Test -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.7</version>
			<scope>test</scope>
		</dependency>        
	</dependencies>
	
	<!-- build: 프로젝트 빌드 시 사용되는 플러그인 추가 및 버전 정보 설정  -->
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-eclipse-plugin</artifactId>
                <version>2.9</version>
                <configuration>
                    <additionalProjectnatures>
                        <projectnature>org.springframework.ide.eclipse.core.springnature</projectnature>
                    </additionalProjectnatures>
                    <additionalBuildcommands>
                        <buildcommand>org.springframework.ide.eclipse.core.springbuilder</buildcommand>
                    </additionalBuildcommands>
                    <downloadSources>true</downloadSources>
                    <downloadJavadocs>true</downloadJavadocs>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <compilerArgument>-Xlint:all</compilerArgument>
                    <showWarnings>true</showWarnings>
                    <showDeprecation>true</showDeprecation>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.2.1</version>
                <configuration>
                    <mainClass>org.test.int1.Main</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```

<br><br>

## root-context.xml

root.context.xml  = 모든 웹을 구성할 때 공유되는 자원,소스들을 작성하는 곳 .. <br>
프로그램에 전체적으로 사용할 내용들을 여기다가 작성하면 됨.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<!-- Root Context: defines shared resources visible to all other web components -->
		
	<!-- 
		root-context.xml 파일은 서버 구동 시 web.xml에서 가장 먼저 읽어들이는 설정 파일이다.
		DB 연결 정보(JDBC, Mybatis, DBCP), AOP, 트랜잭션 처리 관련 설정(언제 트랜잭션 처리를 할 것인지?), 파일 업로드 관련 설정등과 같은
		프로젝트 전반적으로 사용될 설정을 작성함.
	-->
		
		<!-- DBCP 사용을 위한 Datasource를 Bean으로 등록  -->
		<!-- destroy-method="close" : 주어진 세션을 자동으로 반환(close)하라는 설정  -->
		<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close">
      
      <property name="driverClassName" value="oracle.jdbc.driver.OracleDriver"/>
      <property name="url" value="jdbc:oracle:thin:@localhost:1521:xe"/>
      <property name="username" value="spring"/>
      <property name="password" value="spring"/>
      <!-- defaultAutoCommit: 풀에서 생성 된 연결의 기본 자동 커밋 상태입니다. 설정하지 않으면 setAutoCommit 메서드가 호출되지 않습니다. -->
      <property name="defaultAutoCommit" value="false"/>
      <!-- 자동 커밋을 방지해서 사용자가 원하는 시점에 커밋할 수 있게 함.  -->
      
      <!-- 커넥션 풀 설정 -->
      <property name="initialSize" value="10"/> <!-- 초기 커넥션 수, 기본 0 -->
      <property name="maxTotal" value="500"/> <!-- 최대 커넥션 수, 기본 8 -->
      <property name="maxIdle" value="100"/> <!-- 유휴 상태로 존재할 수 있는 커넥션 최대 수, 기본 8-->
      <property name="minIdle" value="10"/> <!-- 유휴 상태로 존재할 수 있는 커넥션 최소 수, 기본 0 -->
      <property name="maxWaitMillis" value="60000"/> <!-- 예외 발생 전 커넥션이 반환 될 떄 까지 대기하는 최대 시간(ms), 기본 -1(무기한) -->
      
   </bean>
   
   
   
   <!-- 
      SqlSession : sql구문을 DB에 전달, 실행하는 객체
      SqlSessionFactory : SqlSession을 만드는 객체
      sqlSessionFactoryBean : mybatis 설정 파일(mybatis-config.xml)과 Connection Pool 정보를 이용하여 SqlSessionFactory를 만드는 객체
      sqlSessionTemplate : SqlSession 객체에 트랜잭션 처리 역할이 가능하도록 하는 객체
    -->
   
   <!-- 마이바티스 SqlSession 등록하기 (xml 방식으로 bean 등록)-->
   <!-- SqlSessionFactoryBean: MyBatis 설정파일을 바탕으로 SqlSessionFactory를 생성한다. Spring Bean으로 등록해야 한다.
      
    -->
   <bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
      <!-- mybatis-config.xml 설정 불러오기 -->   
      <property name="configLocation" value="classpath:mybatis-config.xml"/>  
      <property name="dataSource" ref="dataSource"/> 
      <!-- dataSource : 위에 생성한 Bean 을 참조 한다.  -->
   </bean>
   
   
   
   <!-- SqlSessionTemplate : 기본 SQL 실행  + 트랜잭션 관리 역할을 하는 SqlSession을 생성할 수 있게 하는 객체(Spring bean으로 등록해야함.)-->
   <bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
      <constructor-arg ref="sqlSessionFactoryBean"/>
   </bean>
		
</beans>
```

<br><br>


## mybatis-config.xml


src/main/resources에  mybatis-config.xml을 만들어서 
마이바티스 홈페이지에서 xml설정파일 복사해온것을 붙여넣음. <br>
DB연결은 root-context.xml에서 했기 때문에  DB연결 설정부분 필요 없음. <br>
NULL값 세팅해줌.
<br>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<settings>
		<!-- 마이바티스로 JDBC 수행 시  NULL값이 있으면 에러 발생함 , DB로 전달되는 값 중 NULL이 있으면 에러가 아닌 NULL 자체로 인식  -->
		<setting name="jdbcTypeForNull" value="NULL"/>
	</settings>
	
	
	<!-- 별칭 지정  -->
	<typeAliases>
		<typeAlias type="com.kh.spring.member.model.vo.Member" alias="Member"/>
	</typeAliases>
	
	
	<mappers>
		<mapper resource="/mappers/member-mapper.xml"></mapper>
	</mappers>
	
</configuration>
```

<br><br>


## 로그인

### member.vo

```java
package com.kh.spring.member.model.vo;

import java.sql.Date;

public class Member {
	private int memberNo;
	private String memberId;
	private String memberPwd;
	private String memberName;
	private String memberPhone;
	private String memberEmail;
	private String memberAddress;
	private String memberInterest;
	private Date memberEnrollDate;
	private String memberStatus;
	private String memberGrade;
	
	public Member() {}

	public int getMemberNo() {
		return memberNo;
	}

	public void setMemberNo(int memberNo) {
		this.memberNo = memberNo;
	}

	public String getMemberId() {
		return memberId;
	}

	public void setMemberId(String memberId) {
		this.memberId = memberId;
	}

	public String getMemberPwd() {
		return memberPwd;
	}

	public void setMemberPwd(String memberPwd) {
		this.memberPwd = memberPwd;
	}

	public String getMemberName() {
		return memberName;
	}

	public void setMemberName(String memberName) {
		this.memberName = memberName;
	}

	public String getMemberPhone() {
		return memberPhone;
	}

	public void setMemberPhone(String memberPhone) {
		this.memberPhone = memberPhone;
	}

	public String getMemberEmail() {
		return memberEmail;
	}

	public void setMemberEmail(String memberEmail) {
		this.memberEmail = memberEmail;
	}

	public String getMemberAddress() {
		return memberAddress;
	}

	public void setMemberAddress(String memberAddress) {
		this.memberAddress = memberAddress;
	}

	public String getMemberInterest() {
		return memberInterest;
	}

	public void setMemberInterest(String memberInterest) {
		this.memberInterest = memberInterest;
	}

	public Date getMemberEnrollDate() {
		return memberEnrollDate;
	}

	public void setMemberEnrollDate(Date memberEnrollDate) {
		this.memberEnrollDate = memberEnrollDate;
	}

	public String getMemberStatus() {
		return memberStatus;
	}

	public void setMemberStatus(String memberStatus) {
		this.memberStatus = memberStatus;
	}

	public String getMemberGrade() {
		return memberGrade;
	}

	public void setMemberGrade(String memberGrade) {
		this.memberGrade = memberGrade;
	}

	@Override
	public String toString() {
		return "Member [memberNo=" + memberNo + ", memberId=" + memberId + ", memberPwd=" + memberPwd + ", memberName="
				+ memberName + ", memberPhone=" + memberPhone + ", memberEmail=" + memberEmail + ", memberAddress="
				+ memberAddress + ", memberInterest=" + memberInterest + ", memberEnrollDate=" + memberEnrollDate
				+ ", memberStatus=" + memberStatus + ", memberGrade=" + memberGrade + "]";
	}
}
```

<br>

### memberService.java


```java
package com.kh.spring.member.model.service;

import com.kh.spring.member.model.vo.Member;

public interface MemberService {
	
	/* Service Interface를 사용하는 이유
	 * 1. 프로젝트에 규칙성을 부여하기 위하여
	 * 2. 결합도 약화를 위하여 --> 유지보수성 향상
	 * 3. 구버전 Spring의 AOP 호환을 위해서
	 */
	
	
	
	/** 로그인 Service
	 * @param inputMember
	 * @return loginMember
	 */
	public abstract Member loginAction(Member inputMember);
	
}

```
<br>

### memberServiceImpl.java

```java
package com.kh.spring.member.model.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.kh.spring.member.model.dao.MemberDAO;
import com.kh.spring.member.model.vo.Member;

@Service // Service를 담당하는 계층이다라고, Service layer, 비즈니스로직(데이터 가공, 트랜잭션처리)을 가지는 클래스임을 명시하면서 bean으로 등록함.
public class MemberServiceImpl implements MemberService{

	@Autowired // 자동으로 MemberDAO 객체(bean)이 의존성 주입됨(DI).
	private MemberDAO dao;
	
	// 로그인 Service
	@Override
	public Member loginAction(Member inputMember) {
		// 이전에는 트랜잭션 처리를 위해서 Service에서 Connection, SqlSession 객체를 만든 것.
		// 하지만 Spring에서는 AOP를 이용한 트랜잭션 처리 기술을 활용할 예정이라, 필요가 없어짐.
		
		// DAO를 수행하고 결과를 반환 받음.
		Member loginMember = dao.loginAction(inputMember);
		
		return loginMember;
	}
}
```

<br>

### MemberDAO.java

```java
package com.kh.spring.member.model.dao;

import org.mybatis.spring.SqlSessionTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import com.kh.spring.member.model.vo.Member;

@Repository
public class MemberDAO {

	// DI를 이용하여 SqlSession을 주입 받음.
	@Autowired
	private SqlSessionTemplate sqlSession;
	// SqlSessionTemplate :  mybatis-config.xml에 작성되어 있음.
	
	/** 로그인 DAO
	 * @param inputMember
	 * @return loginMember
	 */
	public Member loginAction(Member inputMember) {
		return sqlSession.selectOne("memberMapper.loginAction", inputMember);
	}

}
```

<br>

### member-mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="memberMapper">
  
  <!-- resultMap : 조회된 result set의 컬럼명과 VO의 필드명이 같지 않을 때 매핑 시키는 역할  -->
  <resultMap type="Member" id="member_rm">
		 <!-- DB의 기본 키 -->
     <id property="memberNo" column="MEMBER_NO"/>
     
     <!-- DB의 일반 컬럼들 -->
     <result property="memberId" column="MEMBER_ID"/>
     <result property="memberPwd" column="MEMBER_PWD"/>
     <result property="memberName" column="MEMBER_NM"/>
     <result property="memberPhone" column="MEMBER_PHONE"/>
     <result property="memberEmail" column="MEMBER_EMAIL"/>
     <result property="memberAddress" column="MEMBER_ADDR"/>
     <result property="memberInterest" column="MEMBER_INTEREST"/>
     <result property="memberEnrollDate" column="MEMBER_ENROLL_DATE"/>
     <result property="memberStatus" column="MEMBER_STATUS"/>
     <result property="memberGrade" column="MEMBER_GRADE"/>
  </resultMap>
  
  
  <!-- 로그인  -->
  <select id="loginAction" parameterType="Member" resultMap="member_rm">
  	SELECT * FROM MEMBER
  	WHERE MEMBER_ID = #{memberId} 
  	AND MEMBER_PWD = #{memberPwd}
  	AND MEMBER_STATUS ='Y'
  
  </select>
  
  
</mapper>
```
<br>


### MemberController.java


- 스프링에는 request, session 을 통합한 model 이 이 있음 <br>
  HttpSession session 을 매개변수로 사용 안 함 <br>
  Model을 사용 한 후 @SessionAttributes 을 사용해서 session scope로 이동시킨다. <br>

```java
package com.kh.spring.member.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.SessionAttributes;

import com.kh.spring.member.model.service.MemberService;
import com.kh.spring.member.model.service.MemberServiceImpl;
import com.kh.spring.member.model.vo.Member;

// 1) @Component  // 객체(컴포넌트)를 나타내는 일반적인 타입으로 bean 등록 역할을 함.

@Controller  // @Component 보다 구체적인 의미를 가지고 있음. (MVC를 확실하게 구분하기 위해서)
			 // 프레젠테이션 레이어, 가장 앞에서  웹 애플리케이션에서 잔달된 요청 응답을 처리하는 클래스 + bean 등록
@RequestMapping("/member/*")
@SessionAttributes({"loginMember"}) // Model에 추가된 데이터 중  key 값이 해당 어노테이션에 적혀있는 값과 일치하는 데이터를 session scope로 이동 시키는 기능. 값이 여러개일 경우 , 로 구분함.
public class MemberController {
	
	// Spring 이전에는 service를 컨트롤러 내에서 공용으로 사용하기 위하여
	// 필드 또는 최상단 부분에 service 객체를 생성했지만,
	//private MemberService service= new MemberServiceImpl();
	
	// Spring에서는 객체의 생명주기를 Spring Container가 관리할 수 있도록  함.
	// == bean으로 등록하여 IOC를 통해 제어를 한다.
	
	@Autowired
	private MemberService service;
	// @Autowired : 자동으로 연결해주는 어노테이션
	// 빈 스캐닝(component-scan)을 통해 등록된 bean 중 알맞은 bean을 해당 변수에 의존성 주입(DI)를 진행한다.
	
	
	/** 로그인 화면 전환용 Controller
	 * @return viewName
	 */
	@RequestMapping("login")
	public String loginView() {
		return "member/login";
	}
	
	
	// --------------------------------------------------------------------------------
	// 로그인 동작 Controller

	// 1. HttpServletRequest를 이용한 파라미터 전달 받기.
 /*	@RequestMapping("loginAction")
	public String loginAction(HttpServletRequest request) {
		// 매개변수에 HttpServletRequest를 작성한 경우 해당 객체를 스프링 컨테이너가 자동으로 주입해 줌
		String memberId = request.getParameter("memberId");
		String memberPwd = request.getParameter("memberPwd");
		
		System.out.println("memberId : " + memberId);
		System.out.println("memberPwd : " + memberPwd);
		
		return "redirect:/";
	}
 */
	
	// 2. @RequestParam을 이용한 파라미터 전달 방법.
	// - request객체를 이용한 파라미터 전달 어노테이션
	// 매개변수 앞에 해당 어노테이션을 작성하면, 매개변수에 값이 주입됨.
	
	// [속성]
	// value : 전달받은 input 태그의 name 속성값 (하나의 속성만 있을 때 속성명을 안 쓸 경우 value로 인식함, 하나 이상 사용 시 무조건 속성명을 적어줘야 된다.)
	// required : 필수적으로 필요한 파라미터, 입력된 name 속성값 파라미터 필수 여부 지정 (기본값 true)
	//  -> required = true 인 파라미터가 존재하지 않는다면 400 Bad Request 에러가 발생함.
	// defaultValue : 파라미터 중 일치하는 name 속성 값이 없을 경우 대입할 값 지정.
	// -> required = false인 경우에 주로 사용한다.
/*	@RequestMapping(value="loginAction", method=RequestMethod.POST)
	public String loginAction( @RequestParam("memberId") String memberId, @RequestParam("memberPwd") String memberPwd,
							   @RequestParam(value="cp", required=false, defaultValue="1") int cp){
		
		System.out.println("memberId : " + memberId);
		System.out.println("memberPwd : " + memberPwd);
		System.out.println("cp : " + cp);
		
		return "redirect:/";
	}
*/
	
	// 3. @RequestParam 어노테이션 생략
	// - 매개변수명을 전달되는 파라미터 name 속성값과 일치시키면 자동으로 주입됨.
	// ** 어노테이션 코드를 생략할 경우 가독성이 떨어져서, 협업 시 좋지 않아 잘 사용하지 않음. 
/*	@RequestMapping("loginAction")
	public String loginAction(String memberId, String memberPwd) {
		
		System.out.println("memberId : " + memberId);
		System.out.println("memberPwd : " + memberPwd);
		
		return "redirect:/";
	}
*/	
	
	// 4. @ModelAttribute를 이용한 파라미터 전달
	// 요청 페이지에서 여러 파라미터가 전달 될 때
	// 해당 파라미터가 한 객체의 필드명과 같다면 일치하는 객체를 하나 생성하여 자동으로 세팅 후 반환
	
	// (주의사항) 전달 받아 값을 세팅할 VO 내부에는 반드시 기본생성자, setter가 작성되어 있어야 한다.
	//  + name 속성값과 필드명이 같아야 함.
	
	// 파라미터가 객체안에 들어와서 매핑된것을  커맨드 객체라고 한다.
/*	@RequestMapping("loginAction")
	public String loginAction(@ModelAttribute Member inputMember) {
		System.out.println("memberId : " + inputMember.getMemberId());
		System.out.println("memberPwd : " + inputMember.getMemberPwd());
		
		return "redirect:/";
	}
*/
	
	// 5. @ModelAttribute 어노테이션 생략
	@RequestMapping("loginAction")
	public String loginAction(Member inputMember, Model model ) {
		// Model = 전달하고자 하는 데이터를 Map 형식(K:V)으로 담아서 전달하는 용도의 객체
		// Model 객체는 기본적으로 request scope 이지만, 클래스 위쪽에 작성된 @SessionAttributes를 이용하면 session scope로 변경됨.
		
		// System.out.println(inputMember);
		
		// 비즈니스 로직 수행 후 결과 반환받기
		Member loginMember = service.loginAction(inputMember);
		System.out.println(loginMember);
		
		
		if(loginMember !=null) {
		 model.addAttribute("loginMember", loginMember);
			
		}
		
		return "redirect:/";
	}
	
	
	// 상황에 따라 쓰이는게 다르지만 보통 @ModelAttribute를 많이 사용함. 매개변수 4개 이상 넣지 않는다는 개발자들만의 암묵적인 규칙?이 있음.
	// @ModelAttribute 와 @RequestParam 동시에 사용할 수 있지만  데이터가 중복될 수 있다. 데이터가 겹치면 비효율적이라 사용 시 주의해야 된다.
}
```