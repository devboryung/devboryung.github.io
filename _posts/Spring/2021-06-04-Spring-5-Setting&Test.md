---
title: "[Spring] 데이터베이스 관련 설정 및 테스트"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 데이터베이스 관련 설정 및 테스트

root-context.xml에는 mybatis-spring 네임스페이스를 추가하고, DataSource의 설정과 MyBatis의 설정을 추가한다.<br>

> root-context.xml

![image](https://user-images.githubusercontent.com/73421820/120820230-4dba9200-c58f-11eb-8a16-ac152c67ea12.png)
<br>


```xml
<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
    <property name="driverClassName" value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy"></property>
    <property name="jdbcUrl" value="jdbc:oracle:thin:@localhost:1521:XE"/>
    <property name="username" value="book_ex"/>
    <property name="password" value="book_ex"/>
</bean>

<bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource" destroy-method="close">
    <constructor-arg ref="hikariConfig"/>
</bean>

<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"></property>
</bean>

<mybatis-spring:scan base-package="org.zerock.mapper"/>
```

<br>

root-context.xml은 내부적으로 Log4jdbc를 이용하는 방식으로 구성되어 있어서 이전 프로젝트에 생성한 log4jdbc.log4j2.properties 파일을 추가해 준다.

![image](https://user-images.githubusercontent.com/73421820/120821098-1f898200-c590-11eb-9bf4-c87f4bc79b72.png)

<br><br>

프로젝트가 정상적으로 실행되려면 DataSource와 MyBatis의 연결이 반드시 필요하기 때문에  이전 프로젝트에 생성한 DataSourceTests 클래스와 JDBCTests 클래스를 테스트 패키지에 추가한다.<br>

![image](https://user-images.githubusercontent.com/73421820/120821616-99ba0680-c590-11eb-9172-f1af65d169a2.png)<br>




<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
