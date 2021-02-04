---
title: "2020년 02월 04일"
excerpt: "Spring 정리"
search: true
categories: 
  - Academy
tags: 
  - FrameWork
  - Java
  - Spring
toc: true
---

## 스프링 설정


서버구동! tomcat Start -> 

1. web.xml(배포서술자)을 읽는다 . 서버 배포에 필요한 설정 초기화해주는 파일. <br>
1-1. listener : web.xml에서 가장 먼저 생성되는 객체 <br>
리스너가  생성될 때 param-value에 작성된 것들(root-context.xml, spring-security.xml 파일)을 읽어들인다. <br>
1-2. Dispatcher Servlet을 지정한다.(servlet-context.xml파일)<br>
1-3. filter(문자 인코딩 필터)<br>
<br><br>

2. root-context.xml, spring-security.xml<br>
2-1. root-context.xml (다른 모든 웹 구성 요소에 표시되는 공유 자원을 정의)<br>
프로젝트에서 공통적으로 사용하는 요소들(DB연결,AOP..)를 정의하면서 bean 등록을 한다.<br><br>
2-2. spring-security.xml :  스프링 프로젝트 보안 요소 추가 설정 파일<br><br><br><br>
*  Bean이란? 스프링 컨테이너가 생명주기를 관리하는 객체,<br>
> 개발자가 아닌 스프링 컨테이너가 생성한 객체, 제어 권한이 스프링 컨테이너에게 있다.
> 공통적으로 사용하는 자원들을 bean으로 등록함으로써 스프링 컨테이너가 제어 권한을 가져  필요한 곳에 의존성주입(DI)가 가능해 진다.<br>
3. Dispatcher Servlet(servlet-context.xml파일)<br>
클라이언트의 요청을 최전방에서 받아들이고 요청에 맞는 컨트롤러를 찾아 연결 후 반환되는 응답을 적절한 View로 연결해 클라이언트에게 응답하는 역할을 한다.<br>
>3-1. annotation-driven : <br> 
>@RequestMapping, @ExceptionHandler 를 사용 가능하게 하는 태그.<br>
>component-scan : 지정된 패키지 내의 @Component,@Controller,@Service,@Repository 어노테이션이 붙은 클래스를 Bean으로 등록하는 태그. (Bean = 스프링 컨테이너가 제어, DI가 가능)<br><br>
>3-2. Handler Mapping : <br>
>요청 주소를 처리할 수 있는 컨트롤러 메소드를 찾는 기능.<br>
(요청이 오면  그것을 처리할 수 있는 컨트롤러가 어디 있는지 찾아주는 기능.)
-> @Controller, @RequestMapping 을 통해서 요청 처리 메소드를 검색한다.<br><br>
>3-3. View Resolver : <br>
>컨트롤러에서 반환된 뷰 이름을 토대로 응답화면 view를 검색함.<br>
		(prefix + 반환된 뷰 이름 + suffix)

<br><br><br>

## 마이바티스 관련 설정

1. root-context.xml에 작성된 내용<br>
1-1. DataSource : DBCP(커넥션 풀) 설정 객체 (DB연결 정보, 커넥션 풀 생성 정보, 오토커밋 설정을 작성 한다.)<br>
커넥션 풀 : 커넥션을 필요할 때 하나 씩 만드는게 아닌 커넥션 풀에 미리 커넥션을 만들어 둠.<br>
---이 때 까지는 단순 JDBC에서만 사용하는 용도임.<br>
단순한 JDBC를 조금 더 간단하고 효율성있게 사용하기 위해서 (DB연결을 쉽게 하고, 조금 더 SQL을 쉽게 사용할 수 있게)마이바티스를 사용한다.<br><br>
1-2. SqlSessionFactoryBean : 마이바티스 사용을 위한 SqlSession 생성 객체(Bean) 생성<br>
해당 객체 생성 시 DataSource가 필요하다. +  mybatis-config.xml 파일도 필요함.<br>
DB연결 정보 = DataSource,  추가적인 마이바티스 관련된 내용(별칭,매퍼) = mybatis-config.xml 가 있음.<br><br>
1-3. SqlSessionTemplate : SqlSession을 통한 DB연결 시 트랜잭션 처리를 관리할 수 있는 객체(Bean)<br><br><br>
2. mybatis-config.xml : 마이바티스 설정 파일<br>
마이바티스 관련 세팅, 별칭,mapper등록 등의 내용을 정의<br><br><br>
3. mapper-xml :  마이바티스 사용 시 sql 구문이 작성된 xml 파일<br>
`<mapper namespace="aaa">` : 해당 매퍼파일의 이름을 "aaa" 라고 지정 <br><br>
`<resultMap>` : SELECT구문에서만 사용되는,  SELECT 시 조회 결과인 ResultSet의 컬럼명과 과 결과를 담을 VO의 필드명이 일치하지 않는 경우 이를 매핑 시켜주는 태그.<br><br>
(속성) parameterType : DAO -> mapper로 전달받은 자료형을 작성.<br><br>
(속성) resultType : SELECT 조회 결과의 자료형을 작성. (기본자료형, string,필드명,==컬럼명인 vo) <br><br>
     (+ DML 구문은 resultType은 _int로 고정이기 때문에 생략 가능하다. <br> + INSERT 성공 1, 실패 0 / UPDATE,DELETE 성공한 행의 개수, 실패 0 )<br>
(속성)resultMap : 선언한 <resultMap>태그의 id 작성.<br>
**resultType과 resultMap 속성은 동시에 작성할 수 없다.<br><br>


<br><br><br><br>

## Controller에서 새롭게 사용된 어노테이션, 객체 정리

- Annotation이란?<br>
컴파일러, 스프링컨테이너가 해석하는 주석. <br>
컴파일러,스프링컨테이너에게 무언가를 알려주기 위한 주석. <br><br><br>


@Controller : 컨트롤러임을 알려줌 + Bean 등록 <br><br>
@RequestMapping : Handler Mapping 시 검색되는 주소를 작성. (매핑되는 주소)<br><br>
@SessionAttributes : Model에 추가된 데이터 중 key값이 일치하는 데이터의 scope를 session으로 이동 시킴.<br><br>
@ResponseBody : 메소드의 반환값이 View name이 아니라 값 자체임을 알려줌. AJAX에서 많이 사용됨.<br><br>
*컨트롤러에서 반환되는 것은 view resolver나 핸들러 매핑으로 가게되는데, 뷰 네임이 아닌  값이라고 알려주는 것임.<br><br>
@RequestParam : 요청 파라미터를 매개변수에 매핑시켜달라고 하는 것.<br>
ex) RequestParam("memberId") String id <br> -> 파라미터 중에 memberId라는 것의 값을 매개변수 id에 넣어달라고 하는 것임.<br><br>
@ModelAttribute : 요청 파라미터 중 지정된 객체의 필드와 name 속성 값이 일치하는 파라미터를 객체에 setting하여 커맨드 객체로 반환함을 지정한다.<br>
ex) @ModelAttribute Member signUpMember <br> -> 파라미터로 전달된 값 중 name 속성값이 Member 클래스 필드와 일치하는 것을  객체에 담아 signUpMember로 반환해 줌.<br>(==signUpMember가 참조하는 객체를 '커맨드 객체'라고 부른다.) <br><br>

@Autowired : Bean으로 등록된 객체 중 지정된 변수와 타입이 일치하는 Bean을 자동으로 찾아 의존성 주입을 수행한다.<br><br>

@ExceptionHandler : 지정된 컨트롤러 내에서 발생하는 예외를 처리하는 메소드임을 알려줌.<br><br><br><br>

Model 객체 : 데이터를 key:value 형태로 전달해주는 용도의 객체<br>
(기본 scope는 request scope, 
@SessionAttributes에 작성된 값과 key값이 일치하는 경우 session scope)<br><br>

RedirectAttributes 객체 : 리타이렉트 시 데이터를 request scope로 전달해주는 객체.<br>
원래 redirect 시 요청데이터가 폐기돼서 사용을 못하는데,  RedirectAttributes 를 사용하면  redirect 시 session scope로 잠시 올려둠으로  데이터를 전달할 수 있다.<br><br>

SessionStatus 객체 : @SessionAttributes로 인해 등록된 session을 관리하는 객체.<br>

<br><br><br><br>

## Service에서 새롭게 사용된 어노테이션, 객체 정리

@Service : 서비스임을 알려줌 + Bean 등록<br><br>
@Transactional : 지정된 서비스 메소드에서 예외(기본값 : RuntimeException)발생 시 트랜잭션을 rollback 시키고, 예외가 발생하지 않으면 commit 시킬 것을 알려주는 어노테이션<br>
+ rollbackFor 속성 : 발생하는 예외 기준을 변경하는 속성.<br><br><br>


bcrypt 암호화 : (입력값 + 랜덤값(salt)) --> 암호화 진행 하는 방법<br>
- 비밀번호 암호화에 특화됨. <br>
- BCryptPasswordEncoder.encode( 비밀번호 ) : 암호화 된 비밀번호 반환 <br>
- BCryptPasswordEncoder.matches(평문,암호화비밀번호) : 평문과 암호화 비밀번호가 <br>일치하면 True, 다르면 false를 반환 <br>
사용 하려면 spring-security-core 모듈이 필요하다. <br><br><br>

<br><br><br><br>

## DAO에서 새롭게 사용된 어노테이션, 객체 정리

@Repository : 저장소(DB)와 연결되는 객체임을 알려줌 + Bean 등록 <br><br>

SqlSessionTemplate : 트랜잭션 관리가 가능한 마이바티스용 DB 연결 객체 (root-context.xml 에서 정의 함) <br><br>
<br><br><br><br>


## 요청 주소 경로 (URL) 

- 최상위 요청 주소 경로(/) : contextPath <br>
contextPath는 기본적으로 프로젝트 명 또는 패키지 중 세번 째 레벨의 이름을 사용하는 경우가 많다. <br>

- ContextPath 변경방법 :  <br>
1. web.xml의 displayName 변경  <br>
2. pom.xml의 artifactId 변경(프로젝트 배포 이름)  <br>
3. 프로젝트 우클릭 후 properties -> webProjectSettings 의 context root 변경<br>
(1,2,3 영구적 변경) <br><br>
4. Servers 탭에서 서버 더블 클릭 -> Modules 탭에서 Edit 로 Path 변경.  <br>
(4 server가 유지되는 동안만 변경) <br><br>
** 프로젝트 우클릭 properties로 변경하는 방법을 가장 많이 사용한다. <br><br><br><br>

- html,jsp, js, css (화면과 관련된) 에서는 최상위 요청 주소 작성하는 법 : /spring , ${contextPath}
- Controller에서 최상위 요청 주소 작성하는 법 (/spring, request.getContextPath())<br>
--> 만약 Controller 에서 "/"만 작성하는 경우는 localhost:8080/  마지막에 있는 "/"를 나타냄.
- 단, RequestMapping과 같은 주소 매핑에서는 "/"가 최상위 주소 /spring 을 나타냄.
 
 <br><br><br><br>

- 파일 경로 (Path)
>최상위 폴더(/) == webapp 폴더를 나타냄 (배포되는 최상위 폴더) <br>
>(/webapp, /WebContent 사용하지 않음. 최상위 폴더는 /만 작성해주면 된다.)  <br>


- 절대 경로 : 절대 변하지 않는 한 위치를 기준으로 경로를 표시하는 방법
>보통 최상위 요청 주소(contextPath) 또는 최상위 폴더(webapp)를 기준으로 함. <br>
>ex) login.jsp 파일의 절대 경로 :  /WEB-INF/views/member/login.jsp <br>
>ex) signUpAction 요청의 절대 경로 : ${contextPath}/member2/signUpAction<br><br>


- 상대 경로 : 현재 위치를 기준으로 다른 요청이나 파일의 위치를 표시하는 방법. <br>
> [기호] <br>
>/ : 하위 디렉토리 <br>
>../ : 상위 디렉토리 <br>
>(공백) : 같은 디렉토리<br><br>
>ex) ${contextPath}/member2/signUp 상태에서<br>
>   ${contextPath}/member2/signUpAction 으로 상대 경로로 요청 지정하는 방법.<br>
>   ${contextPath}/member2/ 까지 같음 == 같은 디렉토리<br>
>   경로 :  href="signUpAction"<br><br>
>ex) ${contextPath}/member2/signUp 상태에서<br>
>    ${contextPath}/board/insert 로 상대 경로로 요청 지정하는 방법.<br>
>    경로 : href="../board/insert"<br><br>
>ex) webapp/WEB-INF/views/member/login.jsp에서<br>
>    webapp/index.jsp 까지 상대 경로 지정 방법<br>
>   경로 : href ="../../../index.jsp"<br>

<br><br><br><br>
 