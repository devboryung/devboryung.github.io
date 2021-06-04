---
title: "[Spring] Mybatis 연동"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## Mybatis란

Mybatis는 'SQL 매핑 프레임워크'로 분류된다.<br>
개발자들은 JDBC 코드의 복잡하고 지루한 작업을 피하는 용도로 많이 사용한다.<br><br>

> 전통적인 JDBC 프로그램

1. 직접 Connection을 맺고 마지막에 close()  
2. PreparedStatement 직접 생성 및 처리
3. PreparedStatement 의 setXX() 등에 대한 모든 작업을 개발자가 처리
4. SELECT의 경우 직접 ResultSet 처리

<br>

> MyBatis

1. 자동으로 Connection close() 가능
2. Mybatis 내부적으로 PreparedStatement 처리
3. #{prop}와 같이 속성을 지정하면 내부적으로 자동으로 처리
4. 리턴 타입을 지정하는 경우 자동으로 객체 생성 및 ResultSet 처리



MyBatis는 기존의 SQL을 그대로 활용할 수 있다는 장점이 있고, 진입장벽이 낮아 JDBC의 대안으로 많이 사용된다.<br><br><br>


### MyBatis 관련 라이브러리 추가

pom.xml에 라이브러리 설정

```xml
<dependency>
  	<groupId>org.mybatis</groupId>
 	<artifactId>mybatis</artifactId>
 	<version>3.4.6</version>
</dependency>
<dependency>
  	<groupId>org.mybatis</groupId>
  	<artifactId>mybatis-spring</artifactId>
  	<version>1.3.2</version>
</dependency>
<dependency>
  	<groupId>org.springframework</groupId>
  	<artifactId>spring-tx</artifactId>
  	<version>${org.springframework-version}</version>
</dependency>
<dependency>
  	<groupId>org.springframework</groupId>
  	<artifactId>spring-jdbc</artifactId>
  	<version>${org.springframework-version}</version>
</dependency>
```

mybatis/mybatis-spring : MyBatis 와 스프링 연동용 라이브러리<br>
spring-jdbc/spring-tx : 스프링에서 데이터베이스 처리와 트랜잭션 처리<br>

<br>
<br>

### SQLSessionFactory


SQLSessionFactory는 내부적으로 SQLSession을 만들어내는 존재이다.<br>
개발에서는 SQLSession을 통해서 Connection을 생성하거나 원하는 SQL을 전달하고, 결과를 리턴 받는 구조로 작성한다.<br><br>

root-context.xml에 Bean태그를 작성한다.

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"></property>
</bean>
```

<br><br>

### 스프링과의 연동 처리

SQLSessionFactory를 이용해서 코드를 작성해도 직접 Connection을 얻어서 JDBC코딩이 가능하지만,<br> 
좀 더 편하게 작업하기 위해서 SQL을 어떻게 처리할 것인지 별도의 설정을 분리해 주고, 자동으로 처리되는 방식을 이용하는 것이 좋다.<br>
이를 위해서는 MyBatis의 Mapper를 작성해 줘야 한다.<br>
Mapper는 쉽게 말해 SQL과 그에 대한 처리를 지정하는 역할을 한다.<br><br>


> Mapper인터페이스

Mapper를 작성한느 작업은 XML을 이용할 수도 있지만, 최소한의 코드를 작성하는 Mapper 인터페이스를 사용해 실습한다.<br>
인터페이스에 MyBatis의 어노테이션을 이용해서 SQL을 메서드에 추가한다.<br>

```java
package org.zerock.mapper;

import org.apache.ibatis.annotations.Select;

public interface TimeMapper {
	@Select("SELECT sysdate FROM dual")
	public String getTime();
}
```

Mapper를 작성한 후 MyBatis가 동작할 때 Mapper를 인식할 수 있도록 root-context.xml에 추가적인 설정을 진행한다.<br>
root-context.xml 파일을 열고, 하단의 Namespaces항목에서 'mybatis-spring'탭을 선택한다.<br>

```xml
<mybatis-spring:scan base-package="org.zerock.mapper"/>
```

'mybatis-spring:scan' 태그의 base-package 속성은 지정된 패키지의 모든 MyBatis 관련 어노테이션을 찾아서 처리한다.<br>
Mapper를 설정하는 작업은 각각의 XML이나 Mapper 인터페이스를 설정할 수도 있지만, 매번 너무 번잡하기 때문에 예제는 자동으로 org.zerock.mapper 패키지를 인식하는 방식으로 하는 것이 가장 편리하다.<br>

<br><br><br>


### Mapper 테스트
MyBatis-Spring은 Mapper 인터페이스를 이용해서 실제 SQL 처리가 되는 클래스를 자동으로 생성한다.<br>
개발자들은 인터페이스와 SQL만을 작성하는 방식으로도 모든 JDBC 처리를 끝낼 수 있다.<br>

```java
package org.zerock.persistence;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.zerock.mapper.TimeMapper;

import lombok.Setter;
import lombok.extern.log4j.Log4j;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml")
@Log4j
public class TimeMapperTests {
	
	@Setter(onMethod_ = @Autowired)
	private TimeMapper timeMapper;
	
	@Test
	public void testGetTime() {
		log.info(timeMapper.getClass().getName());
		log.info(timeMapper.getTime());
	}
}
```

TimeMapperTests 클래스는 TimeMapper가 정상적으로 사용이 가능한지를 알아보기위한 테스트코드이다.<br>
코드가 정상적으로 동작한다면 스프링 내부에는 TimeMapper 타입으로 만들어진 스프링 객체가 존재한다는 뜻이다.<br><br>

timeMapper.getClass().getName()은 실제 동작하는 클래스의 이름을 확인해 주는데 실행 결과를 보면 개발 시 인터페이스만 만들어 주었는데 내부적으로 적당한 클래스가 만들어진 것을 확인할 수 있다.<br>

<br><br>

### XML 매퍼와 같이 쓰기

MyBatis를 이용해서 SQL을 처리할 때 어노테이션을 이용하는 방식이 압도적으로 편리하지만, SQL이 복잡하거나 길어지는 경우에는 어노테이션 보다는 XML을 이용하는 방식을 더 선호한다.<br><br>
XML을 작성해서 사용할 때는 XMl 파일의 위치와 XML 파일에 지정하는 namespace가 중요하다.<br>

[XML에 대한 설명](https://mybatis.org/mybatis-3/ko/sqlmap-xml.html)<br><br>

Mapper 인터페이스와 XML을 같이 이용하기 위해 기존의 TimeMapper 인터페이스에 추가적인 메서드를 선언한다.<br>

```java
package org.zerock.mapper;

import org.apache.ibatis.annotations.Select;

public interface TimeMapper {
	@Select("SELECT sysdate FROM dual")
	public String getTime();
	
	public String getTime2();
}
```
<br>
추가된 getTime2 메서드에는 @Select와 MyBatis의 어노테이션, SQL이 존재하지 않는다.<br>
실제 SQL은 XML을 이용해서 처리하며, 새로 생성한 TimeMapper.xml을 이용한다.<br>


```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  
  <mapper namespace="org.zerock.mapper.TimeMapper">
  	<select id="getTime2" resultType="string">
  		SELECT sysdate FROM dual
  	</select>
  </mapper>
```

MyBatis는 Mapper 인터페이스와 XML을 인터페이스의 이름과 namespace 속성값을 가지고 판단한다.<br>
XML의 namespace와 동일한 이름이 존재하면 이를 병합해서 처리한다.<br>
이 경우 메서드 선언은 인터페이스에 존재하고 SQL에 대한 처리는 XML을 이용하는 방식이다.
<br>

select 태그의 id속성의 값은 메서드의 이름과 동일하게 맞춰야 한다.<br>
resultType 속성은 인터페이스에 선언된 메서드의 리턴 타입과 동일하게 작성한다.<br>

<br><br>

### log4jdbc-log4j2 설정

MyBatis는 내부적으로 JDBC의 PreparedStatement를 이용해서 SQL을 처리한다.<br>
SQL에 전달되는 파라미터의 JDBC에서와 같이 '?'로 치환되어서 처리된다.<br>
복잡한 SQL의 경우 '?'로 나오는 값이 제대로 되었는지 확인하기가 쉽지 않고, 실행된 SQL의 내용을 정확히 확인하기 어렵다.<br>
이런 문제를 해결하기 위해서 SQL을 변환해서 PreparedStatement에 사용된 '?'가 어떤 값으로 처리되었는지 확인하는 기능을 추가하도록 합니다.<br>
SQL 로그를 제대로 보기 위해서 log4jdbc-log4j2 라이브러리를 사용한다.<br>


pom.xml에 라이브러리를 추가한다.

```xml
<dependency>
	<groupId>org.bgee.log4jdbc-log4j2</groupId>
	<artifactId>log4jdbc-log4j2-jdbc4</artifactId>
	<version>1.16</version>
</dependency>
```

로그 설정 파일을 추가한다.

```java
log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
```

root-context.xml에서 JDBC의 연결 정보를 수정한다.<br>
수정이 제대로 되지 않으면 데이터베이스의 로그가 정상적으로 기록되지 않는다.<br>


```xml
<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
<!-- 		<property name="driverClassName" value="oracle.jdbc.driver.OracleDriver"/>
	<property name="jdbcUrl" value="jdbc:oracle:thin:@localhost:1521:XE"/> -->
	<property name="driverClassName" value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy"></property>
	<property name="jdbcUrl" value="jdbc:log4jdbc:oracle:thin:@localhost:1521:XE"></property>
	<property name="username" value="book_ex"/>
	<property name="password" value="book_ex"/>
</bean>
```


<br><br>

### 로그의 레벨 설정

테스트 코드를 실행하면 상당히 많은 양의 로그가 출력되기 때문에 시간이 지나면 불편해지게 되기 때문에 log4j.xml에서 로그의 레벨을 수정해 주면 좋다.<br><br>
기본 설정의 로그는 info 레벨이기 때문에 warn과 같이 좀 더 높은 레벨의 로그만 기록하게 수정하면 테스트 코드를 실행할 때 이전에 비해 로그의 양이 줄어드는 것을 확인할 수 있다.<br>

```xml
<logger name="jdbc.audit">
	<level value="warn" />
</logger>
<logger name="jdbc.resultset">
	<level value="warn" />
</logger>
<logger name="jdbc.connection">
	<level value="warn" />
</logger>
```

[로그 레벨에 대한 설명](https://logging.apache.org/log4j/2.x/manual/customloglevels.html)

<br><br>




관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>

