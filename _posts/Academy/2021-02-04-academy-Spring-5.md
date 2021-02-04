---
title: "2020년 02월 04일"
excerpt: "Spring 4 (회원가입, 비밀번호 암호화)"
search: true
categories: 
  - Academy
tags: 
  - FrameWork
  - Java
  - Spring
toc: true
---

## 회원가입 

<br>

![가입하기 버튼](https://user-images.githubusercontent.com/73421820/106834267-73668600-66d8-11eb-95df-a58f29564008.png)
<br><br>

- signupView.jsp  가입하기 버튼<br>
가입하기 버튼 :  submit <br>
폼 태그에서 제출 : action주소로  폼 태그 내부의 input 태그 값을 제출 하는 것.<br>
input 태그의 name 속성을 잘 기억하자.<br>
@ModelAttribute 를 이용해 커맨드객체로 만들기 위해 현재 name 속성의 값들이 필드값과 동일하게 되어있다 (번호,주소 제외). <br><br>

- signUpView.jsp 하단에  전화번호와 주소를 조합해주는 script 가 있다.<br>

![조합](https://user-images.githubusercontent.com/73421820/106834340-95600880-66d8-11eb-9c0e-420237a7204b.png)
<br><br>

- 관심분야 같은 경우 원래는 배열로 넘어왔었지만  스프링에서 동일한 name 속성을 가진 input태그 값을  <br>
String[] 에 저장할 경우, 배열 요소로 저장되며<br> 
String으로 저장할 경우  자동으로 ","로 구분된 한 줄의 문자열이 된다.<br>
<br><br>


- 스프링에서 트랜잭션 처리를 하는 방법
1. 코드 기반 처리 방법 -> 기존 방법
2. 선언적 트랜잭션 처리 방법
> 작성법 : 
> 1) <tx:advice> XML 작성 방법<br>
> 2) @Transactional 어노테이션 작성 방법 (트랜잭션 처리를 해야 되는 메소드 위에 어노테이션을 추가해주면 된다.) <br>
>트랜잭션 처리를 위한 매니저가 bean으로 등록되어 있어야 함. <br>
>(root-context.xml에서 sqlSessionTemplate 빈 생성 구문에서 이미 처리 함)
      
- @Transactional 이 commit/rollback을 하는 기준
>해당 메소드 내에서 RuntimeException이 발생하면 rollback, 발생하지 않으면  실패했을 때 <br>-> result가 0이 나온다(삽입이 안 됨/ DB에는 변화가 없음) <br>commit,rollback 할 필요가 없음. <br>      
> 하지만 발생하는 예외의 기준이 RuntimeException이다. 
> (RuntimeException-> 대표적인 unchecked Exception으로  try,catch문이 없어도 예외를 처리할 수 있음.(ex.NullPointerException) <br>
>DB를 운영할 때 SQLException이 가장 많이 발생하는데, RuntimeException과 자식 관계가 아닌 형제 관계임,, 관계가 없다. <br>
> 발생하는 예외의 기준을 변경해 줄 수 있다. 변경 방법 : rollbackFor 속성을 사용.(언제 rollback을 진행할지 기준을 정해 줌)
<br>
<br><br>

memberController2.java

```java
// 회원가입 Controller
@RequestMapping("signUpAction")
public String signUp(@ModelAttribute Member signUpMember , RedirectAttributes ra // redirect시 데이터 전달용 객체
                    /*String[] memberInterest, String memberInterest*/ ) {

//  signUpMember : 회원가입 시 입력한 값들이 저장된 커맨드 객체
    System.out.println(signUpMember);  // 넘어오는 값 확인 용

// 동일한 name 속성을 가진 input태그 값은
// String[] 에 저장할 경우, 배열 요소로 저장되며
// String으로 저장할 경우 ","로 구분된 한 줄의 문자열이 된다.
// System.out.println(Arrays.toString(memberInterest));
// System.out.println(memberInterest);


// 입력한 값을 DB까지 전달하기.
// 마이바티스에서는  insert 성공하면 1, 실패하면 0 을 반환해준다.
// 회원 가입 서비스 호출
int result  = service.signUp(signUpMember);

// 회원 가입 성공 여부에 따른 메세지 지정
if(result>0) { // 성공
    swalIcon ="success";
    swalTitle = "회원 가입 성공";
    swalText = "환영합니다.";
}else { // 실패
    swalIcon ="error";
    swalTitle = "회원 가입 실패";
    swalText = "회원 가입 과정에서 문제가 발생했습니다.";
}

ra.addFlashAttribute("swalIcon",swalIcon);
ra.addFlashAttribute("swalTitle",swalTitle);
ra.addFlashAttribute("swalText",swalText);

return "redirect:/"; // 메인 화면 재요청

}
```
<br><br>

MemberService2.java

```java
/** 회원가입 Service
* @param signUpMember
* @return result
*/
int signUp(Member signUpMember);
```

MemberService2Impl.java
```java
// 회원가입 Service 구현
@Transactional(rollbackFor = Exception.class)
@Override
public int signUp(Member signUpMember) {
    return dao.signUp(signUpMember);
    // 해당 서비스 메소드에서 예외가 발생하지 않으면 마지막에 commit이 수행됨.
}
```
<br><br>

MemberDAO2.java

```java
/** 회원 가입 DAO
* @param signUpMember
* @return result
*/
public int signUp(Member signUpMember) {
return sqlSession.insert("memberMapper2.signUp", signUpMember);
}
```
<br><br>

member-mapper2.xml

```sql
  <!-- 회원가입  -->
  <insert id="signUp" parameterType="Member">
  	INSERT INTO MEMBER 
  	VALUES(SEQ_MNO.NEXTVAL, #{memberId}, #{memberPwd} , 
  	#{memberName}, #{memberPhone}, 
  	#{memberEmail}, #{memberAddress}, 
  	#{memberInterest}, DEFAULT, DEFAULT, DEFAULT )
  </insert>
```

<br><br><br><br>



## 암호화


- 비밀번호를 저장하는 방법
1. 평문 형태 그대로 저장 -> 범죄 행위
2. SHA-512 같은 단방향 암호화(단방향 해쉬함수)를 사용 (암호화 된 비밀번호를 다이제스트라고 한다.) <br>
> 문제점 : 같은 비밀번호를 암호화 하면 똑같은 다이제스트가 반환된다. == 해킹에 취약함.
3. bcrypt 방식의 암호화 -> 비밀번호 암호화에 특화된 암호화 방식
> 입력된 문자열과 임의의 문자열(salt)을 첨부하여  암호화를 진행해서  같은 비밀번호를 입력해도 서로 다른 다이제스트가 반환된다.
> Spring-security-core 모듈 추가.


- Spring-security-core 모듈 추가 하는 법
* pom.xml 에 라이브러리 추가 <br>
메이븐 레파지토리에 Spring-security-core 검색 ->
Spring Security Core 클릭 ->  5.2.8.RELEASE 클릭 (보안요소는 최신버전을 사용하는게 좋지만, 이클립스와 호환을 위해 사용) ->  maven 코드 복사, pom.xml에 붙여넣기
<br>

pom.xml <br>

![pom xml](https://user-images.githubusercontent.com/73421820/106842672-ceec4000-66e7-11eb-901b-1f754801549a.png)<br><br>
 

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
    <!-- https://mvnrepository.com/artifact/org.apache.commons/commons-dbcp2 -->
    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-dbcp2</artifactId>
        <version>2.7.0</version>
    </dependency>
    
    <!-- Spring-security-core -->
    <!-- https://mvnrepository.com/artifact/org.springframework.security/spring-security-core -->
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-core</artifactId>
        <version>5.2.8.RELEASE</version>
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


* Spring-security-core bcrypt암호화를 진행해주는 코드를 bean으로 등록하기 위해 xml 생성. <br>
(보안요소들은 따로 관리하는게 좋다.) <br>
Spring 폴더에 spring-security.xml파일 생성 <br>
xml 에서 빈을 등록할 때 시큐리티를 추가할 수 있는 설정을 해준다. <br>


spring-security.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:security="http://www.springframework.org/schema/security"
   xsi:schemaLocation="http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security-5.3.xsd
      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
   <!-- 스프링 프로젝트에서 보안 관련 요소를 설정하고 관리하는 xml 파일  -->

   <!-- bcrypt 암호화 관련 클래스를 bean 등록  -->
   <bean id="bcryptPasswordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>
</beans>
```
<br><br>

경로 이미지 <br>
![bcrypt 경로](https://user-images.githubusercontent.com/73421820/106842494-7d43b580-66e7-11eb-84b5-17fd5bab6a8f.png) <br><br><br>



* 그 후 bean 등록을 위해 암호화 관련 클래스를 등록해주고,
bean으로 읽어올 수 있게  web.xml param-value에  추가해준다.<br><br>

web.xml

![파람](https://user-images.githubusercontent.com/73421820/106842821-11158180-66e8-11eb-841a-15324f7f6772.png)<br><br>


```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.1" xmlns="http://java.sun.com/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee https://java.sun.com/xml/ns/javaee/web-app_3_1.xsd">

    <!-- web.xml 파일 (배포 서술자)
        - WAS(Web Application Server :  요청에 따라 응답화면을 만들어주는 서버 == tomcat) 실행 시 제일 처음으로 읽어들이는 파일이다.
        - 서버 실행 시 초기에 설정할 내용(filter, DB접속 관련, aop, scheduling...) 또는 설정이 작성된 파일을 읽고 수행하도록 web.xml 파일에 등록한다.
        - 서블릿 실행 이전에 서블릿 초기화 용도로도 사용된다.
        
        + 프로젝트의 웹모듈 버전과 web.xml의 버전은 같아야 한다.(같지 않으면 에러는 안나지면 태그 오류가 발생할 수 있다.)
    -->

<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
<!-- context-param : 설정에 사용할 파라미터 설정 -->
<context-param>
    <!-- contextConfigLocation : 설정 파일 경로 (변수명)  -->
    <param-name>contextConfigLocation</param-name>
    <!-- 실제 root-context.xml의 경로 
                classpath : 프로젝트의 소스 폴더로 등록된 폴더의 경로 ( Java Build Path -> source 에서 확인 가능 )
        -->
    <param-value>
        classpath:spring/root-context.xml
        classpath:spring/spring-security.xml
    </param-value>
</context-param>

<!-- Creates the Spring Container shared by all Servlets and Filters -->
<!-- 모든 서블릿과 필터가 공유하는 , 스프링 컨테이너를 만드는 역할.
            listener : context-param에 작성된 설정 파일을 읽어 스프링 컨테이너를 생성하는 리스너 객체.  
            - 서버 실행 시 가장 먼저 로딩(pre-loading) 되어야 하는 xml 설정 문서를 읽어들이는 역할. -> 이를 이용해 스프링 컨테이너 생성
-->
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>


<!-- dispatcher Servlet을 지정 -->
<!-- Processes application requests -->
<servlet>
    <servlet-name>appServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring/appServlet/servlet-context.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
    
<servlet-mapping>
    <servlet-name>appServlet</servlet-name>
    <!-- / : 모든 주소에 대해서 요청이 들어올 경우 servlet-context.xml 이 제어해줄거다 (dispatcherServlet)  -->
    <url-pattern>/</url-pattern>
</servlet-mapping>


<!-- 한글 깨짐을 방지하기 위한 인코딩 필터 추가  -->
<filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>
        org.springframework.web.filter.CharacterEncodingFilter		
    </filter-class>
    <!-- 초기값 지정  -->
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>

<filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
    <!-- /* : 모든 요청, 응답에 대해서 필터처리 진행  -->
</filter-mapping>

</web-app>
```

<br><br>


- Memberservice2Imple 에서 암호화를 위한 객체를 의존성 주입

![객체](https://user-images.githubusercontent.com/73421820/106842495-7d43b580-66e7-11eb-91c3-775b3ea5823d.png)

<br><br>


- Memberservice2Imple 에서 DAO2에 전달하기 전에 비밀번호 암호화 진행.

![서비스암호화](https://user-images.githubusercontent.com/73421820/106842573-a06e6500-66e7-11eb-918e-6eb7556d437a.png)


<br><br>

- 로그인 시 비밀번호와 DB에 암호화 된 비밀번호 비교하기
bcrypt 암호화를 사용하는 경우 
같은 비밀번호를 입력해도 암호화된 문자열이 계속 다르므로 DB에서 비밀번호 일치를 이용한 조건식 사용이 불가능 하다. <br>
대신 이를 비교할 수 있는 별도의 메소드를 BCryptPasswordEncoder가 제공해 줌 (matches() ) <br>

MemberServiceImpl 에서 암호화를 위한 객체를 의존성 주입

![객체](https://user-images.githubusercontent.com/73421820/106842495-7d43b580-66e7-11eb-91c3-775b3ea5823d.png)
<br><br>


MemberServiceImpl.java

```java
// 로그인 Service
@Override
public Member loginAction(Member inputMember) {
// 이전에는 트랜잭션 처리를 위해서 Service에서 Connection, SqlSession 객체를 만든 것.
// 하지만 Spring에서는 AOP를 이용한 트랜잭션 처리 기술을 활용할 예정이라, 필요가 없어짐.

// DAO를 수행하고 결과를 반환 받음.
Member loginMember = dao.loginAction(inputMember);

// bcrypt 암호화를 사용하는 경우
// 같은 비밀번호를 입력해도 암호화된 문자열이 계속 다르므로 DB에서 비밀번호 일치를 이용한 조건식 사용이 불가능 하다.
// 대신 이를 비교할 수 있는 별도의 메소드를 BCryptPasswordEncoder가 제공해줌 (matches() )

// inputMember에 저장된 비밀번호 : pass05
// DB에 저장된 비밀번호 : $2a$10$A.cFdfTj06zkM2mCmmOf3eqtdYJyGB.Tiar6zCeNEAR9JBwg8PfFW

if(enc.matches(inputMember.getMemberPwd(), loginMember.getMemberPwd() )) { // 비밀번호가 같을 떄
    // getMemberPwd() : 입력받은 평문 비밀번호 
    // loginMember.getMemberPwd() : DB에 저장된 암호화 비밀번호 ,dao에서 조회해 와서 loginMember에 담겨있는 비밀번호.
    // 이 둘을 비교해서  True, False 를 반환해준다.
    
    // DB에서 조회된 회원 정보를 반환하면 되지만 
    // 비밀번혼느  null값으로 변경해서 내보냄.
    loginMember.setMemberPwd(null);
    
}else { // 비밀번호가 다를 때
    
    // 로그인이 실패한 모양을 만들어 줌.
    loginMember = null;
}
return loginMember;
}
	
```


<br><br>

member-mapper.xml <br>

- bcrypt 암호화를 사용하는 경우 <br>
같은 비밀번호를 입력해도 암호화된 문자열이 계속 다르므로 DB에서 비밀번호 일치를 이용한 조건식 사용이 불가능 하다. <br>


```sql
<!-- 로그인  -->
<select id="loginAction" parameterType="Member" resultMap="member_rm">
SELECT * FROM MEMBER
WHERE MEMBER_ID = #{memberId} 
<!-- AND MEMBER_PWD = #{memberPwd} -->
AND MEMBER_STATUS ='Y'
</select>
```


<br><br>

