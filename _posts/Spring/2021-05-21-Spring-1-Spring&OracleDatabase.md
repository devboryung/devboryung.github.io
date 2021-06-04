---
title: "[Spring] Oracle Database 연동"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 프로젝트의 JDBC 연결

JDBC연결을 위해 JDBC Driver가 필요하다.<br>
다른 라이브러리는 Maven을 이용하지만 Oracle 데이터베이스의 JDBC Driver는 11g까지 공식적으로 Maven으로는 지원이 되지 않기 때문에 직접 jar 파일을 프로젝트에 추가한다.<br><br><br>

### JDBC 테스트 코드

데이터베이스 연결이 가능하다면 정상적으로 데이터베이스가 연결된 Connection 객체가 출력된다.<br>
만일 데이터베이스에 문제가 있거나, JDBC 드라이버에 문제가 있다면 안 되기 때문에 반드시 확인해 봐야 한다.<br>
<br>

```java
package org.zerock.persistence;

import static org.junit.Assert.fail;

import java.sql.Connection;
import java.sql.DriverManager;

import org.junit.Test;

import lombok.extern.log4j.Log4j;

@Log4j
public class JDBCTests {
	static {
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
		}catch(Exception e) {
			e.printStackTrace();
		}
	}
	
	@Test
	public void testConnection() {
		
		try(Connection con=
		DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:XE","book_ex","book_ex")){
			
			log.info(con);
		} catch (Exception e) {
			fail(e.getMessage());
		}
	}
}
```

<br><br>

### 커넥션 풀 설정

java에서는 DataSource라는 인터페이스를 통해서 커넥션 풀을 사용한다.<br>
DataSource를 통해 매번 데이터베이스와 연결하는 방식이 아닌, 미리 연결을 맺어주고 반환하는 구조를 이용하여 성능 향상을 한다.<br>
<br><br>



pom.xml에  HikariCP(커넥션 풀 중 하나) 라이브러리를 추가한다.<br>

```xml
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>2.7.4</version>
</dependency>
```
<br>

root-context.xml 안에 HikariCP에 대한 bean태그를 정의한다.<br>

```xml
<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
    <property name="driverClassName" value="oracle.jdbc.driver.OracleDriver"/>
    <property name="jdbcUrl" value="jdbc:oracle:thin:@localhost:1521:XE"/>
    <property name="username" value="book_ex"/>
    <property name="password" value="book_ex"/>
</bean>

<bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource" destroy-method="close">
    <constructor-arg ref="hikariConfig"/>
</bean>
```
<br>


> root-context.xml

스프링에서 root-context.xml은 스프링이 로딩되면서 읽어 들이는 문서이다.<br>
이미 만들어진 클래스들을 이용해서 스프링의 빈으로 등록할 때 사용된다.<br>
일반적인 상황이라면 프로젝트에 직접 작성하는 클래스들은 어노테이션을 이용하는 경우가 많고,<br>
외부 jar 파일 등으로 사용하는 클래스들은 <bean> 태그를 이용해서 작성하는 경우가 대부분이다.<br>

<br><br>


관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
