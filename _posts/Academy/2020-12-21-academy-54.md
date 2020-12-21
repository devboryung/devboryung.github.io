---
title: "2020년 12월 21일"
excerpt: "Servlet - 1"
search: true
categories: 
  - Academy
tags: 
  - Servlet
  - Java
  - HTML
  - CSS
toc: true
---

## Servlet이란?
>자바 서블릿(Java Servlet)은 자바를 사용하여 웹페이지를 `동적`으로 생성하는 서버측 프로그램 혹은 그 사양을 말한다. 자바 서블릿은 웹 서버의 성능을 향상시키기 위해 사용되는 자바 `클래스`의 일종이다.[^w1]
>> 더 자세히 말해보자면 자바를 사용하여 웹을 만들기 위해 필요한 기술이다. 웹 프로그래밍에서 클라이언트의 `요청(Request)`을 처리하고 그 결과를 다시 클라이언트에게 `전송(응답, Response)`하는 Servlet 클래스의 구현 규칙을 지킨 자바 프로그래밍 기술이다.[^b1]

<br>

### Servlet 특징
- 클라이언트의 요청에 대해 `동적으로 작동`하는 웹 애플리케이션 컴포넌트
- HTML을 사용하여 요청에 응답함
- java thread를 이용하여 동작함
- MVC 패턴에서 Controller로 이용함
- http 프로토콜 서비스를 지원하는 javax.servlet.http.HttpServlet 클래스를 상속 받음

<br>

### Servlet 단점
- Servlet에 작성한 HTML 코드 변경 시 재컴파일을 해야 한다.

<br>

### Servlet 상속 관계
- javax.servlet.Servlet 인터페이스
    - javax.servlet.GenericServlet 추상클래스
        - javax.servlet.http.HttpServlet 추상클래스

<br>

### Servlet 동작 방식
![20201221_01](https://user-images.githubusercontent.com/70805241/102755576-fd20f500-43b1-11eb-9116-b6562c9f007f.JPG)
<br>

1. 사용자(클라이언트)가 URL(Uniform Resource Locator)을 클릭하면 HTTP Request를 Servlet Container로 전송한다.
2. Http Request를 전송 받은 Servlet Container는 HttpServletRequest, HttpServletResponse 두 객체 생성한다.
3. DD(배포서술자, Deployment Descriptor) = web.xml은 사용자가 요청한 URL을 분석하여 어느 서블릿에 요청할 것인지 찾는다.
4. 해당 서블릿에서 init() 메소드를 먼저 호출한 후 service() 메소드를 호출하여 클라이언트로부터 전송 받은 방식인 GET, POST 여부에 따라 해당 메소드(doXXX())를 호출한다.
5. doGet() / doPost() 메소드는 동적 페이지를 생성 후 HttpServletResponse 객체에 응답을 보낸다.
6. 응답이 끝나면 destory() 메소드를 호출하여 HttpServletRequest, HttpServletResponse 객체가 소멸된다.

<br>

### Servlet Container
>배포를 위한 포트 연결, 웹 서버 통신을 위한 소켓, 입/출력 스트림을 생성하는 역할을 한다. 대표적으로 Tomcat이 있다. 클라이언트의 요청을 받을 때마다 새로운 자바 스레드(Thread)를 만들어 요청을 처리하고 응답을 해준다.

### Servlet Container 역할
1. **웹 서버와의 통신 지원** : 서블릿과 웹 서버가 손쉽게 통신할 수 있도록 한다. 일반적으로 소켓을 만들고 listen, accept 등을 해야 하지만 서블릿 컨테이너는 이러한 기능을 API로 제공하여 복잡한 과정을 생략하게 한다.

2. **서블릿 생명주기(Life Cycle)관리** : 서블릿 클래스를 로딩하여 인스턴스화 하고, 초기화 메소드를 호출하고, 요청이 들어오면 적절한 서블릿 메소드를 호출한다. 서블릿이 생명을 다 한 순간에는 적절하게 가비지 컬렉션을 진행하여 편의를 제공한다.

3. **멀티쓰레드 지원 및 관리** : 서블릿 컨테이너는 요청이 올 때마다 새로운 자바 스레드를 생성하는데 http 서비스 메소드를 실행하고 나면 스레드는 자동으로 사라진다.

4. **선언적인 보안 관리** : 서블릿 컨테이너를 사용하면 개발자는 보안에 관련된 내용을 서블릿 또는 자바 클래스에 구현하지 않아도 된다.

<br><br>

## 아파치 톰캣(Apache Tomacat)
>아파치 소프트웨어 재단에서 개발한 서블릿 컨테이너(또는 웹 컨테이너)만 있는 웹 애플리케이션 서버이다. 웹 서버와 연동하여 실행할 수 있는 자바 환경을 제공하여 JSP와 자바 서블릿이 실행될 수 있는 환경을 제공한다.[^w2]

<br>

## 톰캣(Tomcat)
- 동적 웹(Dynamic Web)을 만들기 위한 웹 컨테이너(== 서블릿 컨테이너)
- 정적 페이지를 제외한 요청(Servlet, JSP)에 대한 수신, 응답을 하는 웹 응용 서버(WAS)이다.
- 톰캣 사용 시 동적 데이터 처리가 가능하므로 DB 연결 및 데이터 조작, 다른 응용 프로그램과의 상호작용이 가능하다.

<br>

### 톰캣(Tomcat) 다운로드
[![톰캣 다운로드](https://user-images.githubusercontent.com/70805241/102759423-71aa6280-43b7-11eb-8f73-bb4aff078360.png "클릭 시 공식홈페이지로 이동")](http://tomcat.apache.org/)<br>
👆🏼 이미지 클릭 시 공식홈페이지로 이동 <br><br>

1. **zip 파일 다운** <br> ![20201221_03](https://user-images.githubusercontent.com/70805241/102760201-84716700-43b8-11eb-9763-15a5459d200e.JPG)<br><br>
- Windows 환경에서는 zip 파일을, Mac 환경에서는 tar.gz 파일을 다운한다.
- 8 버전을 사용하는 이유는 10 버전은 나온지 얼마 안됐고, 9 버전도 아직까지 많이 사용하지 않아 제일 많이 쓰이는 8 버전으로 실습을 진행한다.<br><br>
2. **원하는 경로에 압축 풀기** <br> ![20201221_04](https://user-images.githubusercontent.com/70805241/102760776-62c4af80-43b9-11eb-9129-d0e1019118a4.JPG)<br><br>
3. **오라클 포트 변경하기** <br> 오라클이 설치되어 있는 경우 오라클에서 사용하는 포트와 톰캣에서 사용하는 포트 번호(8080)가 같아 이클립스에서 실행하면 충돌이 발생한다. <br><br> ![20201221_05](https://user-images.githubusercontent.com/70805241/102761891-e501a380-43ba-11eb-958d-4cf1c8d51fcc.png)<br><br>
이를 해결하기 위해 다음과 같이 설정한다(Windows 기준). <br>
 - CMD 실행
 - sqlplus 입력 후 관리자 계정으로 접속 <br> ![20201221_06](https://user-images.githubusercontent.com/70805241/102762419-9b658880-43bb-11eb-9aa9-51972661d6e6.JPG)
 - 톰캣에서 현재 사용하는 포트 번호 확인 <br> ![20201221_07](https://user-images.githubusercontent.com/70805241/102762659-ef706d00-43bb-11eb-9a1f-1ee14a08307c.JPG)
 - 톰캣 포트 변경 <br> ![20201221_08](https://user-images.githubusercontent.com/70805241/102762660-f0090380-43bb-11eb-9e4c-3ee15021ed1f.JPG) <br><br>


## 이클립스 설정
1. **Encoding 설정**
    - 이클립스 상단 Window - Preferences 클릭
    - Content Types 설정 <br> ![20201221_09](https://user-images.githubusercontent.com/70805241/102763701-65290880-43bd-11eb-85ac-78e99afc89d8.JPG)
    - Workspace 설정 <br> ![20201221_10](https://user-images.githubusercontent.com/70805241/102764030-dcf73300-43bd-11eb-8458-6f528d12819e.JPG)
    -  CSS Files / HTML Files / JSP Files / XML Files 설정 <br> 각 탭에 들어가서 <br> ![20201221_11](https://user-images.githubusercontent.com/70805241/102764376-63ac1000-43be-11eb-9ffb-e219714aa7ce.JPG) <br> Encoding 부분을 사진과 같이 설정한다. <br> ![20201221_12](https://user-images.githubusercontent.com/70805241/102764379-6444a680-43be-11eb-9965-a7165ee45497.JPG)
    - Spelling 설정 <br> ![20201221_13](https://user-images.githubusercontent.com/70805241/102766131-036a9d80-43c1-11eb-83bc-7168c92f646d.JPG) <br><br><br>
2. **톰캣 경로 설정**
    - 이클립스 상단 Window - Preferences 클릭
    - Runtime Environments 설정 <br> ![20201221_14](https://user-images.githubusercontent.com/70805241/102766874-221d6400-43c2-11eb-97f8-91e00f607696.JPG) <br>
    - 톰캣 다운 한 버전에 맞춰서 선택 <br> ![20201221_15](https://user-images.githubusercontent.com/70805241/102766877-234e9100-43c2-11eb-8471-b5219de07fd0.JPG) <br>
    - 톰캣 압축 푼 파일 경로 선택 <br> ![20201221_16](https://user-images.githubusercontent.com/70805241/102766878-234e9100-43c2-11eb-9705-23299ed4aa34.JPG) <br><br><br>
3. **톰캣 연동**
    - 이클립스 하단 Servers 선택 <br> ![20201221_17](https://user-images.githubusercontent.com/70805241/102767859-91e01e80-43c3-11eb-873d-4c2b0ac947fc.JPG)
    - 서버 이름 변경 <br> ![20201221_18](https://user-images.githubusercontent.com/70805241/102767865-9278b500-43c3-11eb-8e02-5f49d375d40c.JPG)
    - 연동 성공 시 좌측 Project Explorer에 Servers 생성, 하단 Servers에 설정한 이름으로 서버 생성 <br> ![20201221_19](https://user-images.githubusercontent.com/70805241/102767868-93114b80-43c3-11eb-90b0-f9b862216850.JPG)  ![20201221_20](https://user-images.githubusercontent.com/70805241/102768256-37938d80-43c4-11eb-91f0-0933d5be2922.JPG)<br><br><br>
4. **서버 Overview 설정** 
    - 이클립스 하단 Servers 탭 내 서버 더블클릭 <br> ![20201221_20-1](https://user-images.githubusercontent.com/70805241/102769633-467b3f80-43c6-11eb-9fa7-ab83cafd2379.JPG)

<br><br>

## 실습
### Dynamic Web Project 생성
1. Project Explorer에서 `ctrl + n` 누른 후 Dynamic Web Project 클릭 <br> ![20201221_21](https://user-images.githubusercontent.com/70805241/102770471-8f7fc380-43c7-11eb-80c1-8f44137bcb44.JPG) <br><br>
2. 프로젝트 이름 지정 후 Next <br> ![20201221_22](https://user-images.githubusercontent.com/70805241/102770446-868ef200-43c7-11eb-97f4-b7be4c99f3f0.JPG) <br><br>
3. `Default output folder` 안에 내용을 `WebContent\WEB-INF\classes` 로 변경 <br> ![20201221_23](https://user-images.githubusercontent.com/70805241/102770449-87c01f00-43c7-11eb-8dd9-3f69f8bba8c5.JPG) <br><br>
4. 빨간색 네모 박스 체크 <br> ![20201221_24](https://user-images.githubusercontent.com/70805241/102770451-87c01f00-43c7-11eb-98b2-77287df8f396.JPG)


### 테스트 index.html 생성
1. ServletProject 내 `WebContent` 폴더 클릭 후 `ctrl + n`을 누른다. <br> ![20201221_25](https://user-images.githubusercontent.com/70805241/102771494-56e0e980-43c9-11eb-9677-6a58430805eb.JPG) <br><br>
2. Web - HTML File 선택 <br> ![20201221_26](https://user-images.githubusercontent.com/70805241/102771499-57798000-43c9-11eb-8180-ca4b9b5222d0.JPG) <br><br>
3. 파일 이름 지정 <br> ![20201221_27](https://user-images.githubusercontent.com/70805241/102771502-58121680-43c9-11eb-9675-43bafe8a58b8.JPG) <br><br>
4. html 5 확인 <br> ![20201221_28](https://user-images.githubusercontent.com/70805241/102771504-58121680-43c9-11eb-812a-0ef762779f9f.JPG) <br><br>
5. index.html 파일 작성 <br> ![20201221_29](https://user-images.githubusercontent.com/70805241/102773388-81807180-43cc-11eb-851a-33fdb46a8ff2.JPG) <br><br>
6. ServletProject 우클릭 - Run As - Run on Server <br> ![20201221_30](https://user-images.githubusercontent.com/70805241/102773391-82190800-43cc-11eb-89df-6cee19e1a507.JPG) <br><br>
7. 서버 실행 <br> ![20201221_31](https://user-images.githubusercontent.com/70805241/102773392-82190800-43cc-11eb-8dd4-e3b242356683.JPG) <br><br>
8. 웹페이지 확인 <br> ![20201221_32](https://user-images.githubusercontent.com/70805241/102773393-82b19e80-43cc-11eb-9c39-c8dba83237b9.JPG)

<br><br>

### 테스트 웹브라우저 변경하기
1. Window - Web Browser - Chrome 클릭 <br> ![20201221_33](https://user-images.githubusercontent.com/70805241/102773864-45014580-43cd-11eb-9ddb-12eec3301d60.JPG) <br><br>
2. 서버 종료 후 재시작하면 크롬으로 열림 <br> ![20201221_34](https://user-images.githubusercontent.com/70805241/102773869-4599dc00-43cd-11eb-9316-8a269c797cbe.JPG)

<br><br>


### index.html

```html
<!DOCTYPE html>
<html lang="ko">
    <head>
    <meta charset="UTF-8">
	    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css" 
        integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
        
	    <title>Servlet 예제</title>
       	<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
       <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
       <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
  
    </head>
    <body>
        <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
            <a class="navbar-brand" href="#">Navbar</a>
    
            <div class="collapse navbar-collapse" id="navbarsExampleDefault">
                <ul class="navbar-nav mr-auto">
                    <li class="nav-item active">
                        <a class="nav-link" href="#">Home</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#">Link</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link disabled" href="#">Disabled</a>
                    </li>
                    <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle" href="#" id="dropdown01" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown</a>
                        <div class="dropdown-menu" aria-labelledby="dropdown01">
                            <a class="dropdown-item" href="#">Action</a>
                            <a class="dropdown-item" href="#">Another action</a>
                            <a class="dropdown-item" href="#">Something else here</a>
                        </div>
                    </li>
                </ul>
                <form class="form-inline my-2 my-lg-0">
                    <input class="form-control mr-sm-2" type="text" placeholder="Search"
                        aria-label="Search">
                    <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
                </form>
            </div>
        </nav>
        
        <div class="jumbotron">
            <div class="container">
                <h1 class="display-3">Hello, Servlet!</h1>
                <p>
				서블릿이란? 웹 서비스를 위한 자바 클래스를 말하며<br>
				자바를 사용해 웹을 만들기 위한 기술로 
				javax.servlet.http.HttpServlet 클래스를 상속받는다.<br>
					
				다시 말해 기존의 java 파일에 웹 페이지를 구현하기 위한 html 코드가 들어간 구조이다.<br>
					
				클라이언트의 요청을 처리하고, 그 결과를 HTML을 이용하여 응답할 수 있는 페이지를 만들어
				클라이언트에게 다시 전달하는 구현 규칙이 있다.<br>
					
                단, Java 코드에 작성된 HTML 코드 변경 시 재 컴파일을 해야되는 단점이 있다.
                </p>
                <p>
                        <a class="btn btn-primary btn-lg" href="https://www.google.co.kr/search?q=Servlet&rlz=1C1QJDB_enKR792KR792&oq=Servlet&aqs=chrome..69i57j69i65j69i60j69i59j69i61l2.2395j0j7&sourceid=chrome&ie=UTF-8" role="button">Learn more &raquo;</a>
                </p>
            </div>
        </div>
    
        <div class="container">
            <!-- Example row of columns -->
            <div class="row">
            
                    <h2>Servlet - 1(GET)</h2>
                    <p>
                  GET 방식은 URL에 변수(데이터)를 포함시켜 요청하는 방법 중 하나로<br>
                                        요청에 사용되는 데이터가 주소에 노출되어 보안 유지가 불가능하다.
                  ex) 로그인 시 아이디가, 비밀번호가 주소창에 노출됨.<br><br>
                  
                  GET방식에서 데이터를 Header에 포함하여 전송하는데
                  Body는 보통 빈 상태로 전송 되며 Header내용 중 Body의 데이터를 설명하는 Content-Type헤더필드도 들어가지 않는다.
                  Header에 데이터가 들어가기 때문에 전송하는 길이에 제한이 있으며 초과 데이터는 절단된다. 캐싱이 가능하다.
               </p>
                        <a class="btn btn-secondary" href="views/testServlet1.html" role="button">View
                            details &raquo;</a>
                    <!-- </p> -->
			</div>                
            <div class="row">
                    <h2>Servlet - 2(POST)</h2>
                    <p>
						POST방식은 URL에 변수(데이터)를 노출하지 않고 요청하는 것으로 보안 유지가 가능하다.
						데이터를 Body에 포함하여 전송하므로 Body의 데이터를 설명하는 Content-Type헤더필드가 들어가고 어떤 타입인지 명시해줘야 한다.
						Body에 데이터가 들어가 전송 길이는 제한이 없지만 최대 요청 받는 시간(Time Out)이 존재해 페이지 요청, 기다리는 시간이 있다.
						URL에 데이터가 노출되지 않아 즐겨찾기나 캐싱이 불가능하다.
                    </p>
                    <p>
                        <a class="btn btn-secondary" href="views/testServlet2.html" role="button">View
                            details &raquo;</a>
                    </p>
            </div>                
            <div class="row">
                    <h2>Servlet - 3(비즈니스 로직)</h2>
                    <p>			

					</p>
                    <p>
                        <a class="btn btn-secondary" href="#l" role="button">View details &raquo;</a>
                    </p>
            </div>                
            <div class="row">
                
					<h2>Servlet - 4(Servlet + JSP)</h2>
					<p>

					</p>
					<p>
						<a class="btn btn-secondary" href="" role="button">View details &raquo;</a>
					</p>
            </div>
            <hr>
        </div>
        <footer class="container">
            <p>&copy; Company 2017-2018</p>
        </footer>
        
  </body>
</html>
```

<br><br>

### views 생성
1. WebContent 클릭 후 `ctrl + n` 
2. General - Folder - Next - 폴더 이름 views로 지정 후 Finish 클릭
3. views 폴더 안에 두 개의 html 생성

- GET 방식 html

```html
<!DOCTYPE html>
<html lang="ko">
    <head>
    <meta charset="UTF-8">
        <title>개인정보 입력 - GET</title>
        <style>
            ul{ list-style-type: none; }

            li{  padding-bottom: 20px; }
        </style>
    </head>
    <body>
        <h2>개인 정보 입력 - GET</h2>
        당신의 개인 정보를 입력하겠습니다.<br>
        데이터 입력 후 확인 버튼을 눌러주세요.<br>

        <form action="/ServletProject/testServlet1.do" method="get" name="testFrm">
            <!-- /ServletProject/testServlet1.do을 앞에 넣는 이유는 현재 Context root 값을 수정하지 않고
                 프로젝트 이름으로 그대로 들어가있음 -->
            <!-- 이렇게 해주면 뒤에 Servlet에서 매핑할 URL을 등록할 때 
                testServlet1.do라고만 해도 앞에 Context root가 붙은 것으로 인식하여
                 우리는 직접적으로 이렇게 붙여준다. -->
                <ul>
                    <li>이름 : <input type="text" name="name" size="10" /></li>
                    <li>성별 : 
                        남자<input type="radio" name="gender" value="남자" />
                        여자<input type="radio" name="gender" value="여자" /> 
                    </li>
                    <li>나이 : 
                        10대 미만<input type="radio" name="age" value="10대 미만" />
                        10대<input type="radio" name="age" value="10대" />
                        20대<input type="radio" name="age" value="20대" />
                        30대<input type="radio" name="age" value="30대" />
                        40대<input type="radio" name="age" value="40대" />
                        50대<input type="radio" name="age" value="50대" />				 
                    </li>
                    <li>사는 도시 : 
                        <select name="city">
                            <option value="서울">서울</option>
                            <option value="경기">경기</option>
                            <option value="강원">강원</option>
                            <option value="충북">충북</option>
                            <option value="충남">충남</option>
                            <option value="경북">경북</option>
                            <option value="경남">경남</option>
                            <option value="전북">전북</option>
                            <option value="전남">전남</option>
                            <option value="제주">제주</option>
                        </select>
                    </li>
                    <li>키 :
                        <input type="range" name="height" min="140" max="200" step="1">
                        <input type="text" id="heightValue">
                    </li>
                    <li>좋아하는 음식 (모두 고르세요) : 
                        <label for="food1">한식</label><input type="checkbox" name="food" id="korea" value="한식" />
                        <label for="food2">중식</label><input type="checkbox" name="food" id="china" value="중식" />
                        <label for="food3">일식</label><input type="checkbox" name="food" id="japan" value="일식" />
                        <label for="food4">양식</label><input type="checkbox" name="food" id="western" value="양식" />
                        <label for="food5">분식</label><input type="checkbox" name="food" id="dduk" value="분식" /> 
                    </li>
                    <li>
                        <br/>
                        <input type="submit" name="btnOK" id="btnOK" value="확인"  />&nbsp;&nbsp; 
                        <input type="reset" value="취소" />
                    </li>
                 </ul>
            </form>

            <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
            <script>
                $("input[type=range]").on("input",function(){
                    $(this).next().val($(this).val());
                });

                $("#heightValue").on("input",function(){
                    $(this).prev().val($(this).val());
                });
            </script>
    </body>
</html>
```

<br>

- POST 방식 html

```html
<!DOCTYPE html>
<html lang="ko">
    <head>
    <meta charset="UTF-8">
        <title>개인정보 입력 - GET</title>
        <style>
            ul{ list-style-type: none; }

            li{  padding-bottom: 20px; }
        </style>
    </head>
    <body>
        <h2>개인 정보 입력 - GET</h2>
        당신의 개인 정보를 입력하겠습니다.<br>
        데이터 입력 후 확인 버튼을 눌러주세요.<br>

        <form action="/ServletProject/testServlet1.do" method="get" name="testFrm">
            <!-- /ServletProject/testServlet1.do을 앞에 넣는 이유는 현재 Context root 값을 수정하지 않고
                 프로젝트 이름으로 그대로 들어가있음 -->
            <!-- 이렇게 해주면 뒤에 Servlet에서 매핑할 URL을 등록할 때 
                testServlet1.do라고만 해도 앞에 Context root가 붙은 것으로 인식하여
                 우리는 직접적으로 이렇게 붙여준다. -->
                <ul>
                    <li>이름 : <input type="text" name="name" size="10" /></li>
                    <li>성별 : 
                        남자<input type="radio" name="gender" value="남자" />
                        여자<input type="radio" name="gender" value="여자" /> 
                    </li>
                    <li>나이 : 
                        10대 미만<input type="radio" name="age" value="10대 미만" />
                        10대<input type="radio" name="age" value="10대" />
                        20대<input type="radio" name="age" value="20대" />
                        30대<input type="radio" name="age" value="30대" />
                        40대<input type="radio" name="age" value="40대" />
                        50대<input type="radio" name="age" value="50대" />				 
                    </li>
                    <li>사는 도시 : 
                        <select name="city">
                            <option value="서울">서울</option>
                            <option value="경기">경기</option>
                            <option value="강원">강원</option>
                            <option value="충북">충북</option>
                            <option value="충남">충남</option>
                            <option value="경북">경북</option>
                            <option value="경남">경남</option>
                            <option value="전북">전북</option>
                            <option value="전남">전남</option>
                            <option value="제주">제주</option>
                        </select>
                    </li>
                    <li>키 :
                        <input type="range" name="height" min="140" max="200" step="1">
                        <input type="text" id="heightValue">
                    </li>
                    <li>좋아하는 음식 (모두 고르세요) : 
                        <label for="food1">한식</label><input type="checkbox" name="food" id="korea" value="한식" />
                        <label for="food2">중식</label><input type="checkbox" name="food" id="china" value="중식" />
                        <label for="food3">일식</label><input type="checkbox" name="food" id="japan" value="일식" />
                        <label for="food4">양식</label><input type="checkbox" name="food" id="western" value="양식" />
                        <label for="food5">분식</label><input type="checkbox" name="food" id="dduk" value="분식" /> 
                    </li>
                    <li>
                        <br/>
                        <input type="submit" name="btnOK" id="btnOK" value="확인"  />&nbsp;&nbsp; 
                        <input type="reset" value="취소" />
                    </li>
                 </ul>
            </form>

            <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
            <script>
                $("input[type=range]").on("input",function(){
                    $(this).next().val($(this).val());
                });

                $("#heightValue").on("input",function(){
                    $(this).prev().val($(this).val());
                });
            </script>
    </body>
</html>
```

### java Resources 생성
1. Java Resources - src 클릭 후 `ctrl + n`
2. Servlet 선택 <br> ![20201221_35](https://user-images.githubusercontent.com/70805241/102775056-7aa72e00-43cf-11eb-81ed-c62091dc4c53.JPG) <br><br>
3. Java package, Class name 지정 <br> ![20201221_36](https://user-images.githubusercontent.com/70805241/102775058-7b3fc480-43cf-11eb-9c58-be2d7685d3da.JPG) <br><br> 
4. Next 클릭 <br> ![20201221_37](https://user-images.githubusercontent.com/70805241/102775060-7bd85b00-43cf-11eb-8260-eddfd832ac08.JPG) <br><br>
5. init, destory, service, doGet, doPost 클릭 후 Finish <br> ![20201221_38](https://user-images.githubusercontent.com/70805241/102775061-7bd85b00-43cf-11eb-81c0-42707aa49283.JPG) 

<br><br>

- GET 방식 java 파일

```java
package com.kh.servlet.controller;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;



public class TestServlet1 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       

    public TestServlet1() {
        super();
        
    }


	public void init(ServletConfig config) throws ServletException {
		System.out.println("init() 메소드 호출(servlet 객체 생성됨)");
		// init() 메소드가 없을 경우 자동으로 service() 메소드가 호출 된다.
		// init() 메소드가 없을 경우 자동으로 service() 메소드가 호출된다.
	}


	public void destroy() {
		System.out.println("destroy() 호출됨.");
	}


//	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
//		
//	}
	// service() 메소드가 없을 경우
	// 자동으로 요청 방식이 get/post 임을 확인하여
	// get일 경우 doGet(), post일 경우 doPost()를 호출한다.
	

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// MVC 역할
		
		// Model : 비즈니스 로직 처리 역할(Service, DAO, VO ...)
		// View : 입력(요청)을 받아서 결과(응답)을 보여주는 역할
		// Controller : 요청에 따라 알맞은 Model을 연결하고
						// Model 처리 결과를 보여줄 View를 선택하는 역할
						// (Servlet)
		// HttpServletRequest : 웹 브라우저에서 사용자가 요청한 내용과 관련 정보를 받아주는 객체
		// HttpServletResponse : 요청 처리 결과를 요청한 클라이언트에게 전달하는 역할을 하는 객체
		
		// form 태그에서 제출된 값(Parameter) 얻어오기
		// tip. 파라미터의 자료형은 모두 String이다.
		
		//request.getParameter("name속성값")
		// -> 해당 name 속성으로 전달된 input 태그의 value를 얻어옴
		
		String name = request.getParameter("name");
		String gender = request.getParameter("gender");
		String age = request.getParameter("age");
		String city = request.getParameter("city");
		String height = request.getParameter("height");
		
		// 체크박스 또는 name 속성이 같은 여러 input 태그 값을 얻어올 경우
		// String[] 형태로 얻어와야 함.
		String[] foodArr = request.getParameterValues("food");
		
		// 파라미터 전달 확인
		System.out.println("name : " + name);
		System.out.println("gender : " + gender);
		System.out.println("age : " + age);
		System.out.println("city : " + city);
		System.out.println("height : " + height);
		
//		for(String food : foodArr) {
//			System.out.println("food : " + food);
//		}
		
		for(int i=0; i<foodArr.length; i++) {
			System.out.println("foodArr [" + i + "] : " + foodArr[i]);
		}
		// 여기까지 요청!!
		
		
		// 응답(response) 화면 준비
		 response.setContentType("text/html; charset=UTF-8");
		 
		 // 서버에서 작성된 문자열을 출력할 스트림
		 // HttpServletResponse 객체를 이용해서 얻어와
		 // 클라이언트 응답 화면과 연결
		 PrintWriter out = response.getWriter(); // 출력 스트림을 얻어와서 연결
		 
		 // 고정적으로 출력되는 부분
		 out.println("<!DOCTYPE html>\r\n" + 
		 		"<html lang=\"ko\">\r\n" + 
		 		"<head>\r\n" + 
		 		"    <meta charset=\"UTF-8\">\r\n" + 
		 		"    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\r\n" + 
		 		"    <title>TestServlet1 응답 페이지</title>\r\n" + 
		 		"    <style>\r\n" + 
		 		"        h1{ color : red; }\r\n" + 
		 		"        span.name { color : orange; }\r\n" + 
		 		"        span.gender { color : gray; }\r\n" + 
		 		"        span.age { color : goldenrod; }\r\n" + 
		 		"        span.city { color : bisque; }\r\n" + 
		 		"        span.height { color : pink; }\r\n" + 
		 		"        span.food { color : violet; }\r\n" + 
		 		"        span{ font-weight:  bold;}\r\n" + 
		 		"    </style>\r\n" + 
		 		"</head>\r\n" + 
		 		"<body>\r\n" + 
		 		"    <h1>개인정보 입력 결과(GET)</h1>");
		 
		 // 동적으로 완성되는 부분은 printf로 작성
		 out.printf("    <span class='name'>%s</span>님은\r\n" + 
		 		"    <span class='age'>%s</span>세이며,\r\n" + 
		 		"    <span class='city'>%s</span>에 사는\r\n" + 
		 		"    키 <span class='height'>%s</span>cm 인\r\n" + 
		 		"    <span class='gender'>%s</span> 입니다. <br>\r\n" + 
		 		"    좋아하는 음식은\r\n" + 
		 		"    <span class='food'>%s</span> 입니다.\r\n" + 
		 		"\r\n" + 
		 		"</body>\r\n" + 
		 		"</html>", name, age, city, height, gender, 
		 			String.join(", ", foodArr));
		 
	}


	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
		
		//post로 요청이 와도 doGet() 메소드로 처리하겠다.
		// == get 방식이든 post 방식이든 같은 방법으로 처리하겠다.
		
	}

}
```

<br>

- POST 방식 java 파일

```java
package com.kh.servlet.controller;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


public class TestServlet2 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       

    public TestServlet2() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		request.setCharacterEncoding("UTF-8");
		// POST 방식으로 데이터 전송 시 문자 인코딩이 정해져 있지 않아
		// 아스키 코드 범위(영어, 숫자, 특수문제 몇 개)를 제외한 문자는
		// 모두 깨진다.
		
		// POST 방식으로 데이터 전송 시 문자 인코딩이 지정되지 않아
		// 전달되는 데이터의 문자 인코딩이 브라우저의 기본값(ISO-8859-1)을 따름
		// UTF-8(이클립스 세팅)과 일치하지 않으므로
		// 파라미터를 얻어오기 전에 request의 문자 인코딩을 UTF-8로 변경.
		
		// 요청 데이터(파라미터)를 모두 얻어와 각 변수에 저장
		String name = request.getParameter("name");
		String gender = request.getParameter("gender");
		String age = request.getParameter("age");
		String city = request.getParameter("city");
		String height = request.getParameter("height");
		
		String[] foodArr = request.getParameterValues("food");
		
		// 파라미터 전달 확인
		System.out.println("name : " + name);
		System.out.println("gender : " + gender);
		System.out.println("age : " + age);
		System.out.println("city : " + city);
		System.out.println("height : " + height);
		
		for(int i=0; i<foodArr.length; i++) {
			System.out.println("foodArr [" + i + "] : " + foodArr[i]);
		}
		
		// 응답 화면 준비
		response.setContentType("text/html; charset=UTF-8");
		
		// 응답 화면을 내보낼 스트림 연결
		PrintWriter out = response.getWriter();
		
		
		 out.println("<!DOCTYPE html>\r\n" + 
			 		"<html lang=\"ko\">\r\n" + 
			 		"<head>\r\n" + 
			 		"    <meta charset=\"UTF-8\">\r\n" + 
			 		"    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\r\n" + 
			 		"    <title>TestServlet2 응답 페이지</title>\r\n" + 
			 		"    <style>\r\n" + 
			 		"        h1{ color : red; }\r\n" + 
			 		"        span.name { color : orange; }\r\n" + 
			 		"        span.gender { color : gray; }\r\n" + 
			 		"        span.age { color : goldenrod; }\r\n" + 
			 		"        span.city { color : bisque; }\r\n" + 
			 		"        span.height { color : pink; }\r\n" + 
			 		"        span.food { color : violet; }\r\n" + 
			 		"        span{ font-weight:  bold;}\r\n" + 
			 		"    </style>\r\n" + 
			 		"</head>\r\n" + 
			 		"<body>\r\n" + 
			 		"    <h1>개인정보 입력 결과(POST)</h1>");
			 
			 // 동적으로 완성되는 부분은 printf로 작성
			 out.printf("    <span class='name'>%s</span>님은\r\n" + 
			 		"    <span class='age'>%s</span>세이며,\r\n" + 
			 		"    <span class='city'>%s</span>에 사는\r\n" + 
			 		"    키 <span class='height'>%s</span>cm 인\r\n" + 
			 		"    <span class='gender'>%s</span> 입니다. <br>\r\n" + 
			 		"    좋아하는 음식은\r\n" + 
			 		"    <span class='food'>%s</span> 입니다.\r\n" + 
			 		"\r\n" + 
			 		"</body>\r\n" + 
			 		"</html>", name, age, city, height, gender, 
			 			String.join(", ", foodArr));
		
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}
```

<br><br>

### web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <!-- 컨트롤 + 쉬프트 + 슬래시 -->
  <!-- web.xml(배포 서술자) : 프로젝트 배포와 관련된 설정을 하는 파일 -->
  <!-- display-name : 배포 시 프로젝트 시작 이름 
  	   http://localhost:8080/ServletProject
  	   						 (display-name)
  -->
  <display-name>ServletProject</display-name>

  <!-- welcome-file-list : 
 		배포 시작 주소로 요청이 오는 경우 연결할 파일 목록 작성하는 부분
 		  ex) http://localhost:8080/ServletProject -->  
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    
  </welcome-file-list>
  <!-- Servlet 생성 및 url 연결 -->
  <servlet>
  	<servlet-name>Test1</servlet-name>
  	<servlet-class>
  		com.kh.servlet.controller.TestServlet1
  	</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>Test1</servlet-name>
  	<url-pattern>/testServlet1.do</url-pattern>
  </servlet-mapping>
  
  <servlet>
  	<servlet-name>Test2</servlet-name>
  	<servlet-class>com.kh.servlet.controller.TestServlet2</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>Test2</servlet-name>
  	<url-pattern>/testServlet2.do</url-pattern>
  </servlet-mapping>
</web-app>
```

<br>

- web.xml 해석 순서 <br> ![20201221_39](https://user-images.githubusercontent.com/70805241/102775492-4f710e80-43d0-11eb-8943-5d3d8c710b67.JPG)




<br><br><br><br>

[^w1]: 출처 - 위키백과
[^b1]: 출처 - [https://mangkyu.tistory.com/14](https://mangkyu.tistory.com/14)
[^w2]: 출처 - 위키백과
