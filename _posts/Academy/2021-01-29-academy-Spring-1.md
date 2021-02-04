---
title: "2020년 01월 29일"
excerpt: "Spring 세팅"
search: true
categories: 
  - Academy
tags: 
  - FrameWork
  - Java
  - Spring
toc: true
---

## Spring Framework란?
> 자바 플랫폼을 위한 오픈 소스 애플리케이션 프레임워크로 간단하게 스프링(Spring)이라고도 불린다.
동적인 웹 사이트를 개발하기 위한 여러 가지 서비스를 제공하고 있으며
대한민국 공공기관의 웹 서비스 개발 시 사용을 권장하고 있는
전자정부 표준 프레임워크(Spring MVC Project 기반 프레임워크)의 기반 기술로서 쓰이고 있다. 

<br><br>

## Spring Framework의 특징 
1. IOC(Inversion of control) 제어 반전
> 컨트롤의 제어권을 프레임워크가 가지고 있다는 뜻으로, 객체의 생성부터 모든 생명주기의 관리까지 프레임워크가 주도하고 있다.<br>
객체를 생성하고, 직접 호출하는 프로그램이 아니라, 만들어둔 자원을 호출해서
사용한다. 
2. DI(Dependency Injection) 의존성 주입
> 설정 파일이나 어노테이션을 통해 객체간의 의존 관계를 설정하여 개발자가 직접 의존하는 객체를 생성할 필요가 없다.
3. POJO 기반 프레임워크(Plain Old Java Object)
> 특정 기술이나 라이브러리 없이 유용한 기능을 그대로 사용할 수 있고, 순수한 자바 객체를 사용하듯이 자바의 객체지향적 설계가 가능하다. <br>
코드가 간결해져 개발이 쉽고, 기존 Java API, 라이브러리 지원에 용이하다.
4. Spring AOP (Aspect Oriented Programming) 관점 지향 프로그래밍
> 트랜잭션, 로깅, 보안 등 여러 모듈, 여러 계층에서 공통으로 필요로 하는 기능의 경우 해당 기능들을 분리하여 관리한다. 
5. Spring JDBC 
> Mybatis나 Hibernate 등의 데이터베이스를 처리하는 영속성 프레임워크와 연결할 수 있는 인터페이스를 제공한다.
6. Spring MVC
> MVC 디자인 패턴을 통해 웹 어플리케이션의 Model, View, Controller 사이의 의
존 관계를 DI 컨테이너에서 관리하여 개발자가 아닌 서버가 객체들을  관리하는 웹 애플리케이션을 구축 할 수 있다. 
<br><br>

## Spring 개발 환경 구축
<br>

### 문자 인코딩 설정
새로운 이클립스로 작업을 진행 할 경우 문자의 인코딩을 UTF-8로 변경해준다.<br>
- Window -> Preperences -> enco 검색 -> 전부 UTF-8로 변경해주기,<br>
- json -> jsonFiles -> UTF-8로 변경해주기,<br>
- General->Editors->TextEditors->Spelling ->UTF-8로 변경해주기. <br>

![인코딩1](https://user-images.githubusercontent.com/73421820/106374678-b965d600-63c8-11eb-9d5c-cc7b8f91b0f7.png)
![인코딩2](https://user-images.githubusercontent.com/73421820/106374679-ba970300-63c8-11eb-8594-258ef4d0bc83.png)
<br><br>

### 이클립스와 톰캣 연결
Window -> Preperences -> Runtime Envorionments -> Add -> Apache Tomcat v8.5 선택 -> Browse -> 톰캣이 설치되어있는 폴더 선택 -> Finish <br>
![서버연결](https://user-images.githubusercontent.com/73421820/106374795-81ab5e00-63c9-11eb-8eae-ec2d6dc1c317.png)
<br><br>

### 서버 실행 단축키 설정 & 탭 키 사이즈 지정 & 글자수 지정
- 서버 실행 단축키 설정 : Window -> Preperences -> keys -> run on server 검색 -> Binding = F10 <br>
- 한 줄에 보여지는 글자 수 지정 : Window -> Preperences -> html검색 -> Editor -> line width :999<br>
- 탭 키 사이즈 지정 :   Window -> Preperences ->general -> editors -> textEditors ->displayed tab width : 2 <br>
![설정1](https://user-images.githubusercontent.com/73421820/106374915-af44d700-63ca-11eb-8d16-87645c1e7c37.png)
![탭키](https://user-images.githubusercontent.com/73421820/106374916-b0760400-63ca-11eb-9d37-e675c2bef607.png)

<br><br>

### Maven 다운로드
이클립스는 기본적으로 설치되어있으나 이클립스 내장 maven을 사용하면  아주 깊숙한 곳에 있는 폴더에 저장되기 때문에 따로 관리도 힘들고, 수정 삭제도 힘들다.<br>
[![메이븐](https://user-images.githubusercontent.com/73421820/106374951-16628b80-63cb-11eb-9806-e0ab9295303d.png)](https://maven.apache.org/index.html "Maven 공식홈페이지") <br>
👆🏼 이미지 클릭 시 공식 홈페이지로 이동
<br>

Download 클릭 -> Binary zip archive 다운로드(윈도우 환경 용) -> 관리하기 편한 폴더(한글X, 띄어쓰기X)에 저장 -> 압축 풀기 -> 압축을 푼 폴더 안에 라이브러리 관리 폴더(repository) 생성 -> repository폴더의 경로를 복사한다. 
![1](https://user-images.githubusercontent.com/73421820/106375053-229b1880-63cc-11eb-821c-92c9f56862a8.png)
![2](https://user-images.githubusercontent.com/73421820/106375056-23cc4580-63cc-11eb-85ec-573399f90174.png)
![폴더생성](https://user-images.githubusercontent.com/73421820/106375057-24fd7280-63cc-11eb-9834-88088a0c50fa.png)
![경로복사](https://user-images.githubusercontent.com/73421820/106375059-25960900-63cc-11eb-8a47-9459900c92b3.png)
<br><br>
<br><br>
conf폴더 -> setting파일 우클릭 -> 연결프로그램 -> VSCode -> localRepository 태그에 방금 복사한 repository폴더의 경로 붙여넣기.
![`](https://user-images.githubusercontent.com/73421820/106375093-8a516380-63cc-11eb-9d44-9852e2c08ab3.png)
![경로 붙여넣기](https://user-images.githubusercontent.com/73421820/106375094-8c1b2700-63cc-11eb-865c-25088a04e2c7.png)
<br>
<br>

### 이클립스와 메이븐 연동
preferences -> maven -> User Settings -> 
UserSettigs: Browse -> dev 폴더에 압축 푼 메이븐 폴더로 이동 -> conf-> settings.xml 선택<br>
![1](https://user-images.githubusercontent.com/73421820/106375126-e1efcf00-63cc-11eb-8e15-92e4eac74b5e.png)

<br><br>

### Spring 설치
Eclipse -> help > eclipse Marketplace -> sts3 검색 -> (Standalone Edition)  Installed -> confirm -> agree -> Finish<br>
![마켓](https://user-images.githubusercontent.com/73421820/106375173-5296eb80-63cd-11eb-87d9-e19170c1c683.png)
![sts3](https://user-images.githubusercontent.com/73421820/106375175-53c81880-63cd-11eb-9485-f0b3168604eb.png)

<br><br>

### sts 구성 설정 추가

1. 이클립스 설치 폴더 ->  eclipse.ini 를 메모장으로 옮겨 놓고, -vmargs 위에 -vm 추가 후 그 아래에 jdk.bin 경로 추가  + \javaw.exe
저장 후 이클립스 재 시작. <br>
![eclipse ini](https://user-images.githubusercontent.com/73421820/106375229-d9e45f00-63cd-11eb-9ba6-dc4946bab2a2.png)
![메모장](https://user-images.githubusercontent.com/73421820/106375230-db158c00-63cd-11eb-91d0-d8e3b28e6bde.png)<br><br>
2. 이클립스 -> New -> Spring 검색 -> Spring legacy Project 선택 -> 프로젝트 이름 지정, Spring MVC Project 선택 -> 최상위 3레벨 지정 -> Finish <br>
![spring legacy](https://user-images.githubusercontent.com/73421820/106375311-86bedc00-63ce-11eb-8712-2056c1f71851.png)
![프로젝트이름지정](https://user-images.githubusercontent.com/73421820/106375312-87577280-63ce-11eb-90dd-916434df82c3.png)
![최상위3레벨](https://user-images.githubusercontent.com/73421820/106375313-87f00900-63ce-11eb-8701-8df657ac11fe.png)<br><br>
3. pom.xml 파일 클릭 -> springframework버전 5.2.8.RELEASE로 변경 -> spectJ버전  1.9.4로 변경 -> slf4j 버전  1.7.25로 변경 <br>
![pom](https://user-images.githubusercontent.com/73421820/106375379-ceddfe80-63ce-11eb-9b23-8fee854d13fb.png)
![버전](https://user-images.githubusercontent.com/73421820/106375393-f0d78100-63ce-11eb-8701-4f0d8c3e4ac9.png)<br><br>
4. https://mvnrepository.com/ 접속 -> gson 검색 -> 2.8.6버전 클릭 -> maven textarea 에있는것 클릭(복사) -> form.xml에 추가<br>
![gson 검색](https://user-images.githubusercontent.com/73421820/106375433-4c097380-63cf-11eb-9913-8117b5650d7d.png)
![2 8 6](https://user-images.githubusercontent.com/73421820/106375434-4ca20a00-63cf-11eb-9800-2988854412e8.png)
![복사할내용](https://user-images.githubusercontent.com/73421820/106375436-4d3aa080-63cf-11eb-9285-b10f96199c5d.png)
![pom에 추가](https://user-images.githubusercontent.com/73421820/106375437-4e6bcd80-63cf-11eb-9440-91308b9f9cb3.png)<br><br>
5. pom.xml -> dependency 태그의 groupId 가 javax.servlet 인 항목(Maven에서 제공하는 JSTL, 정상 적용이 되지 않음) 지우기.<br>
![삭제](https://user-images.githubusercontent.com/73421820/106375456-8246f300-63cf-11eb-9b9e-9ed97b1c276c.png)<br><br>
6. build 태그 안의 groupId : org.apache.maven.plugins 의 버전, 소스 변경 <br>
버전 2.5.1-> 3.1로 변경 / 소스,타겟 -> 1.8로 변경 <br>
![버전소스](https://user-images.githubusercontent.com/73421820/106375490-d0f48d00-63cf-11eb-892c-bc4bb3704a45.png)<br><br>

<br>

### 서버 생성
1. WEB-INF 에 lib 폴더 생성 -> jstl jar파일 lib폴더에 넣기 -> home.jsp에 태그 라이브러리 작성 <br>
![JSTL JAR](https://user-images.githubusercontent.com/73421820/106375543-54ae7980-63d0-11eb-9e8e-98e4bf256256.png)
![태그라이브러리](https://user-images.githubusercontent.com/73421820/106375546-55471000-63d0-11eb-9407-2127451c41a0.png)<br><br>
2. 프로젝트 우클릭 -> properties -> project Facets ->다이나믹 웹 모듈 3.1로 변경 -> 자바 1.8로 변경 -> rumtimes에서 아파치 톰캣 8.5 체크 <br>
![3 1](https://user-images.githubusercontent.com/73421820/106375581-9c350580-63d0-11eb-98b3-ac40c5d76f24.png)
![아파치](https://user-images.githubusercontent.com/73421820/106375583-9ccd9c00-63d0-11eb-87c5-3495d6fef6b0.png) <br><br>


<br><br>