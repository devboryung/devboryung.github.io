---
title: "[SQL] DDL,DML,DCL"
excerpt: "SQL"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---


## SQL 이란
구조화 질의어로 데이터 정의어(DDL)와 데이터 조작어(DML)를 포함한 데이터베이스용 질의언어(query language)의 일종이다.<br> 
SQL은 단순한 질의 기능뿐만 아니라 완전한 데이터 정의 기능과 조작 기능을 갖추고 있다.<br> 
또 온라인 단말기를 통해 대화식으로 사용할 수도 있고 코볼이나 PL/I, C 등의 호스트 언어로 된 프로그램에 삽입되어서 사용되기도 한다.<br> 
SQL은 장치 독립적이고 액세스 경로에 대해서는 어떠한 참조도 하지 않으며, 개개의 레코드보다는 레코드의 집합인 테이블을 단위로 연산을 수행하는 것이 특징이다.<br> <br> 

### DDL(Data Definition Language)
데이터베이스를 정의하는 언어를 말하며, 객체를 생성하거나 수정하거나 삭제하는 등 데이터의 전체 골격을 결정하는 역할의 언어를 말합니다.<br> 
- CREATE: 데이터베이스, 테이블 등을 생성하는 역할을 한다.
- ALTER: 테이블을 수정하는 역할을 한다. 
- DROP: 데이터베이스, 테이블을 삭제하는 역할을 한다. 
- TRUNCATE: 테이블을 초기화 시키는 역할을 한다.

<br> 


### DML(Data Manipulation Language)
정의된 데이터베이스에 입력된 레코드를 조회하거나 수정하거나 삭제하는 등의 역할을 하는 언어를 말한다.<br> 
- INSERT: 데이터를 삽입하는 역할을 한다.
- UPDATE: 데이터를 수정하는 역할을 한다.
- DELETE: 데이터를 삭제하는 역할을 한다.

<br> <br> 

### DCL(Data Control Language)
데이터베이스에 접근하거나 객체에 권한을 주는등의 역할을 하는 언어를 말한다.<br> 
- GRANT: DB 객체에 권한 부여한다.
- REVOKE: 부여한 권한을 취소, 철회한다.

<br> <br> 