---
title: "[Spring 실습] Mybatis 연동"
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
