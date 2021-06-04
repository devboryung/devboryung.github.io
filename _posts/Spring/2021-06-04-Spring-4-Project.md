---
title: "[Spring] 예제 프로젝트 구성"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 예제 프로젝트 구성


예제를 위한 프로젝트로 Spring Legecy Project로 새로 생성 후, pom.xml 수정, 데이터베이스 관련 처리, 스프링  MVC 처리와 같은 순서로 진행한다.<br>

> pom.xml

스프링 , 자바 버전 수정

```xml
<java-version>1.8</java-version>
<org.springframework-version>5.0.7.RELEASE</org.springframework-version>
```

<br>

spring-tx, spring-jdbc, spring-test 라이브러리 추가

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>${org.springframework-version}</version>
</dependency>

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>${org.springframework-version}</version>
</dependency>

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>${org.springframework-version}</version>
</dependency>
```

<br>

MyBatis 이용을 위한 HikariCP, Mybatis, mybatis-spring, Log4jdbc 라이브러리 추가

```xml
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>2.7.8</version>
</dependency>
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
    <groupId>org.bgee.log4jdbc-log4j2</groupId>
    <artifactId>log4jdbc-log4j2-jdbc4</artifactId>
    <version>1.16</version>
</dependency>
```
<br>

테스트와 Lombok을 위해서 jUnit 버전 변경, Lombok 추가

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>   
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.0</version>
    <scope>provided</scope>
</dependency>
```
<br>

Servlet 3.1을 제대로 사용하기 위해 Servlet 버전 수정

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
```
<br>

Servlet 3.1버전과, JDK8버전 기능 활용을 위해 Maven 버전 수정

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>2.5.1</version>
    <configuration>
        <source>1.8</source>
        <target>1.8</target>
        <compilerArgument>-Xlint:all</compilerArgument>
        <showWarnings>true</showWarnings>
        <showDeprecation>true</showDeprecation>
    </configuration>
</plugin>
```

<br>

수정 완료 후 프로젝트 선택 후 Maven > Update Project를 실행한다.<br>
<br>

마지막으로 Oracle JDBC Driver를 프로젝트의 Build Path에 추가하고, Deployment Assembly에도 추가한다.<br>


<br><br>


### 테이블 생성과 Dummy(더미) 데이터 생성

게시물은 각 게시물마다 고유의 번호가 필요하며, 오라클의 경우 시퀀스(sequence)를 이용해서 처리한다.<br>

```sql
CREATE SEQUENCE seq_board;

CREATE TABLE tbl_board(
    bno number(10,0),
    title varchar2(200) not null,
    content varchar2(2000) not null,
    writer varchar2(50) not null,
    regdate date default sysdate,
    updatedate date default sysdate
);

alter table tbl_board add constraint pk_board primary key (bno);
```

<br>

시퀀스를 생성할 때는 데이터베이스의 다른 오브젝트들과 구분하기 위해서 `seq_`와 같이 시작하는 것이 일반적이다.<br>
테이블을 생성할 때는 `tbl_`로 시작하거나 `t_`와 같이 구분이 가능한 단어를 앞에 붙여주는 것이 좋다.<br>
<br>
'tbl_board' 테이블은 고유의 번호를 가지기 위해서 bno 칼럼을 지정했고, 제목(title), 내용(content), 작성자(writer)를 칼럼으로 지정했다.<br>
테이블을 설계할 때는 가능하면 레코드의 생성 시간과 최종 수정 시간을 같이 기록하는 것이 좋기 때문에 생성 시간(regdate)과 레코드의 최종 수정 시간(updatedate) 칼럼을 작성한다.<br>
이때 기본값으로 sysdate를 지정해서 레코드가 생성된 시간은 자동으로 기록될 수 있게 한다.<br><br>

테이블의 생성 이후에는 'alter table'을 이용해서 테이블에 PK를 지정해 준다.<br>
제약조건의 이름은 중요하게 사용되므로 반드시 의미를 구분할 수 있게 생성해 주는 것이 좋다.<br>
<br>

### 더미 데이터의 추가

테이블을 생성하고 나면 여러 개의 데이터를 추가해 주는데 이런 의미 없는 데이터를 흔히 '토이 데이터(toy data)' 혹은 '더미 데이터(dummy data)'라고 한다.<br>
게시물이 많을 수록 유용할 수 있다.<br>

```sql
insert into tbl_board(bno, title, content, writer)
values (seq_board.nextval, '테스트 제목', '테스트 내용', 'user00');
```

<br>

tbl_board의 bno 컬럼은 매번 새로운 값이 들어가야 하므로 seq_board.nextval을 이용해서 매번 새로운 번호를 얻는다.<br>
regdate와 updatedate 칼럼은 기본적으로 현재 시간이 들어가므로, 별도의 작업이 필요하지 않다.<br>

오라클 데이터베이스의 경우 데이터를 insert 한 후 주의해야 하는 점이 commit이다. 오라클의 경우에는 데이터에 대한 가공 작업 후 반드시 commit을 `수동`으로 처리 해야 한다.<br>




<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>