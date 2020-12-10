---
title: "2020년 12월 10일"
excerpt: "Javascript-5"
search: true
categories: 
  - Academy
tags: 
  - Javascript
  - HTML
toc: true
---

## 이벤트(Event)
```html
    <h1>이벤트(Event)</h1>
    <pre>
    이벤트 : 동작, 행위 ex) click

    이벤트 리스너(또는 이벤트 핸들러)
        : 이벤트가 발생하는 것을 대기하고 있다가
            이벤트 발생 시 연결해둔 기능(function)을 수행하는 코드
    
    이벤트 객체 : 발생한 이벤트에 대한 정보를 담고있는 객체
            (이벤트 발생 좌표, 이벤트 발생 요소 정보, 시간 등등)
    </pre>
```
<br>

### 고전 이벤트 모델
요소 객체가 가지고 있는 이벤트 속성에 이벤트 핸들러를 연결하는 방법으로<br/>
    특정 이벤트를 제거하고 싶을 때는
    on이벤트명에 null 값을 대입하면된다.
```html
    <button type="button" id="test1">이벤트 수행</button>
    <button type="button" id="test2">이벤트 삭제</button>
    <div id="area1" class="area"></div>
    <script>
        var test1 = document.getElementById("test1");
        var test2 = document.getElementById("test2");
        var area1 = document.getElementById("area1");

        // 고전 이벤트 모델
        test1.onclick = function() {
            area1.innerHTML = "이벤트 수행 버튼이 클릭돼서 연결될 기능이 수행됨.";
        }

        test2.onclick = function() {
            area1.innerHTML = "이벤트 수행 버튼의 이벤트 핸들러를 삭제시킴";

            test1.onclick = null; // 이벤트 제거
        }
    </script>
```

<br>

### 이벤트 객체
이벤트 객체는 이벤트 발생 시 <br/>
이벤트 핸들러에 작성된 함수의 매개변수(e 또는 window.event)로
발생한 이벤트 정보를 전달함.
<br/>
이벤트 핸들러 내부에서 e.target 또는 this를 이용하여
이벤트가 발생한 요소를 선택할 수 있음.

```html

    <button type="button" id="test3">실행 확인</button>
    <script>
        document.getElementById("test3").onclick = function(e) {
            // e : 이벤트 객체
            // == window.event (보통 ie 8버전 이전 브라우저에서 사용함)

            var event = e || window.event;

            // 이벤트 객체에 있는 모든 정보 확인
            console.log(event);

            // 이벤트가 발생한 요소 확인
            console.log(event.target);
            console.log(this);
            // 이벤트 핸들러 내에서 this는 이벤트가 발생한 요소를 나타냄.
            
            // 이벤트가 발생한 요소의 배경색을 red로 변경
            event.target.style.backgroundColor = "red";
            
            // 이벤트가 발생한 요소의 글자색을 white로 변경
            this.style.color = "white";

        }
    </script>
```

<br>

### 인라인 이벤트 모델
요소 내부에 이벤트를 작성하는 방법으로<br/>
요소 내부에 on이벤트명 = 함수명();을 통해
script에 작성된 함수를 호출하는 방식을 선호함.

```html

    <button type="button" onclick="test3();">실행확인</button>
    <script>
        function test3() {
            alert("인라인 이벤트 모델 확인");
        }
    </script>
```

<br>

### 이벤트 핸들러
 - HTML 문서의 모든 요소가 로딩이 완료되면<br/>
      브라우저가 발생시키는 이벤트<br/>
- 문서의 로딩이 완료된 후 수행되어야 하는 기능이 있을 경우 사용.<br/>
     ex) 스크립트 코드보다 뒤쪽에 작성된 요소를 얻어와야될 경우<br/>
- window.onload = function() {} 또는
      &lt;body onload="함수명()"&gt; 둘 중 하나로 사용 가능.
- onload는 HTML 문서당 한 번만 작성할 수 있음.

```html
  <script>
        // HTML은 위에서 아래로 순차적으로 해석되는데
        // area4가 해석되지 않은 상태에서 내용을 얻어오려고 했기 때문에
        // 에러가 발생함.
        // -> window.onload로 해결
        window.onload = function(){
            console.log(document.getElementById("area4").innerText);
        }
    </script>
    <div id="area4">window.onload 테스트 중</div>
```

<br>

### 기본 이벤트 제거
 기본 이벤트란?<br/>
html 태그에 기본적으로 부여되어 있는 기능, 행위, 동작을 나타냄.<br/>
ex) submit 버튼<br/>
&nbsp;&nbsp;&nbsp;&nbsp;-> form 태그 내부에 있는 submit 버튼 클릭 시 <br/>
&nbsp;&nbsp;&nbsp;&nbsp;    form 태그에 작성된 action 주소로<br/>
 &nbsp;&nbsp;&nbsp;&nbsp;   input 태그 값들을 전송하고 페이지 갱신<br/>
 &nbsp;&nbsp;&nbsp;&nbsp;   (입력 양식 제출 후 페이지 갱신)<br/>
<br/>
- 기본 이벤트 제거 방법 <br/>
이벤트 핸들러의 리턴값을 false로 만들면된다.<br/>

```html
    <form action="#" method="post" onsubmit="return validate();">
        비밀번호 : <input type="password" name="pwd1" id="pwd1"><br>
        비밀번호 확인 : <input type="password" name="pwd2" id="pwd2"><br>
        <button type="submit">제출</button>
    </form> 

    <script>
        function validate() { // 비밀번호 유효성 검사

            // 비밀번호가 입력된 요소 자체를 얻어옴
            var pwd1 = document.getElementById("pwd1");
            var pwd2 = document.getElementById("pwd2");

            // 1) 입력이 되었는가
            if (pwd1.value.trim().length == 0) {
                // .trim() : 문자열 양쪽 공백 제거
                alert("비밀번호를 입력해주세요.");
                pwd1.focus(); // pwd1 요소에 커서를 옮겨둠.
                return false;
            }
            if (pwd2.value.trim().length == 0) {
                // .trim() : 문자열 양쪽 공백 제거
                alert("비밀번호 확인을 입력해주세요.");
                pwd2.focus(); // pwd1 요소에 커서를 옮겨둠.
                return false;
            }

            // 2) 비밀번호 생성 규칙에 맞게 작성되었는가 --> 정규표현식
            // 3) 비민번호, 비밀번호 확인이 일치하는가

            if (pwd1.value != pwd2.value) {
                // 비밀번호 불일치
                alert("비밀번호가 일치하지 않습니다.");
                pwd2.focus();
                return false;
            }
        }
    </script>
```

<br>

### 표준 이벤트 모델(addEventListener)
- w3c에서 공식적으로 지정한 이벤트 모델

- 한 요소에 여러 이벤트 핸들러(리스너)를 설정할 수 있음

- ie 9 버전 이상부터 사용 가능
```html
    <button id="btn1">고전 이벤트 모델 방식</button>
    <button id="btn2">표준 이벤트 모델 방식</button>

    <script>
        var btn1 = document.getElementById("btn1");
        var btn2 = document.getElementById("btn2");

        // 고전 이벤트 모델
        btn1.onclick = function(){
            alert("고전 이벤트 모델 방식 테스트"); 
        }
        btn1.onclick = function(){
            btn1.style.backgroundColor = "red";
        }
        // 앞서 작성된 alert 이벤트 핸들러가 사라지고
        // 마지막에 작성된 배경색 변경 이벤트 핸들러만 남음.

        // 표준 이벤트 모델
        btn2.addEventListener("click", function(){
            alert("표준 이벤트 모델 방식 테스트");
        });

        btn2.addEventListener("click", function(){
            btn2.style.backgroundColor = "yellow";
        });
        // 작성한 두 이벤트 핸들러가 모두 정상 동작함.
    </script>
```

<br><br>

## 정규표현식(Regular Expression)이란?
 특정한 규칙을 가진 문자열의 집합을 표현하는데 사용하는 형식 언어로<br/>
    정규 표현식을 이용하면 입력된 문자열에 대하여 <br/>
    특정 조건 검색, 일치 여부, 문자열 치환에 대한 조건문을
    간단하게 처리할 수 있다.
```html
<h3>정규 표현식 참고 사이트</h3>
	<ul>
		<li><a href="https://regexper.com/" target="_blank">작성한 정규표현식을 그림으로 나타내주는 사이트</a></li>
		<li><a href="https://regexr.com/" target="_blank">정규표현식 테스트 사이트</a></li>
		<li><a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/%EC%A0%95%EA%B7%9C%EC%8B%9D"
				target="_blank">
				MDN - 정규표현식 설명
			</a></li>
	</ul>
```

<br>

### 객체 생성 및 메소드
- 정규 표현식 객체 선언 방법<br/>
&nbsp;&nbsp;&nbsp;&nbsp;	1) var regExp = new RegExp("정규표현식");<br/>
&nbsp;&nbsp;&nbsp;&nbsp;    2) var regExp = /정규표현식/;  (양쪽 / == 정규 표현식 리터럴 표기법)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;    -> 두 번째 방법을 더 많이 씀


```html

	<pre>

	- 정규 표현식 객체 메소드

	test() : 문자열에서 정규식 변수의 값과 일치하는 값이 있으면 true / 없으면 false
	exec() : 문자열에서 정규식 변수의 값과 일치하는 값이 있으면 처음 매치된 문자열 반환
	

	- String 내장 객체에서 제공하는 정규표현식 사용 가능 메소드

	match() : 문자열에서 정규식 변수의 값과 일치하는 모든 값을 배열로 반환
	replace() : 문자열에서 정규식 변수의 값과 일치하는 일치하는 부분을 새로운 값으로 변경.
	search() : 맨 처음 일치하는 부분의 시작 인덱스 반환
	split() : 정규식 변수에 지정된 값을 구분자로 하여 배열 생성


	</pre>
	<button onclick="test1();">확인하기</button>
	<div id="area1" class="area"></div>
	<script>
		function test1() {
			var str = 'javascript jquery ajax script';
			var area1 = document.getElementById('area1');

            // 정규표현식 객체 선언
            // var regExp = new RegExp("script");
            var regExp = /script/g;
            // g(global) : 문자열 전체에 대해서 검사 진행

			area1.innerHTML = "str : " + str + "<br>";
			area1.innerHTML += "test()함수 사용 : " + regExp.test(str) + "<br>";
			area1.innerHTML += "exec()함수 사용 : " + regExp.exec(str) + "<br>";

			//정규표현식의 메소드를 직접 사용하기보다 string메소드를 사용하는 것이 일반적이다.
			area1.innerHTML += "match()함수 사용 : " + str.match(regExp) + "<br>";
			area1.innerHTML += "replace()함수 사용 : " + str.replace(regExp, "스크립트") + "<br>";
			area1.innerHTML += "search()함수 사용 : " + str.search(regEcxp)+ "<br>";
			area1.innerHTML += "split()함수 사용 : " + str.split(regExp) + "<br>";
		}
	</script>
```

<br>

## 메타 문자
```html
	<h3>메타 문자를 이용한 문자 검색, 치환</h3>
	<p>특정 메타 문자를 사용하여 문자열에 정규식이 존재하는지<br>
		test()를 이용하여 검사하거나, replace() 이용하여 치환.</p>

	<br><br>

    	<button onclick="test2();">실행확인</button>
    	<div id="area2"></div>
        <script>
            function test2() {
               var area2 = document.getElementById("area2");
               var printStr = "";
      
               //var str = "javascript jquery ajax";
               var str = 'javascript jquery ajax script';
               printStr += "str : " + str + "<br><br>";
      
                  printStr += "<h4>메타문자가 없는 경우 : '완전히' 일치하는 값(문자열) 매칭</h4>";
      
                  printStr += "/a/ -> 문자열에서 a라는 문자와 일치하는 값 매칭<br>";
                  
                  regExp = /a/g;
      
                  printStr += "regExp.test(str) : " + regExp.test(str) + "<br>";
                  // test() : 일치하는 값이 있으면 true
      
                  printStr += "regExp.exec(str) : " + regExp.exec(str) + "<br>";
                  // exec() : 정규식과 일치하는 값이 있으면
                  // 처음 매칭된 문자열 반환
      
                  printStr += "str.replace(regExp,'***') : " + str.replace(regExp,'***') + "<br>";
      
                  printStr += "----------------------------------------------------------<br>";
      
                  printStr += "<h4>[] : []내에 문자 중 하나라도 존재하는 값 매칭</h4>";
                  printStr += "/[as]/ -> 문자열에 a 또는 s 중 하나라도 매칭 <br>";
      
                  regExp = /[as]/g;
      
                  printStr += "regExp.test(str) : " + regExp.test(str) + "<br>";
      
                  printStr += "regExp.exec(str) : " + regExp.exec(str) + "<br>";
      
                  printStr += "str.replace(regExp,'***') : " + str.replace(regExp,'***') + "<br>";
                  // a 랑 s 가 모두 *** 로 바뀐다.
      
                  printStr += "----------------------------------------------------------<br>";
                
                  printStr += "<h4>^ : 문자열의 시작을 의미</h4>";
                  printStr += "/^j/ -> 문자열의 시작이 j인 문자열 매칭<br>";
                  regExp = /^a/;
      
                printStr += "regExp.test(str) : " + regExp.test(str) + "<br>";
                printStr += "regExp.exec(str) : " + regExp.exec(str) + "<br>";
                printStr += "str.replace(regExp,'***') : " + str.replace(regExp,'***') + "<br>";
                
                printStr += "<h4> /^[jsab]/ : 문자열의 시작이 j, s, a, b 중 하나일 때 </h4>"
                regExp = /^[jsab]/;
                
                // str = "aheollworld";
                printStr += "regExp.test(str) : " + regExp.test(str) + "<br>";
                printStr += "regExp.exec(str) : " + regExp.exec(str) + "<br>";
                printStr += "str.replace(regExp,'***') : " + str.replace(regExp,'***') + "<br>";

                printStr += "<h4> $ : 문자열의 끝을 의미 </h4>";
                printStr += "/t$/ : -> 문자열의 끝이 t인 문자열 매칭<br>";
                regExp = /t$/;
                printStr += "regExp.test(str) : " + regExp.test(str) + "<br>";
                printStr += "regExp.exec(str) : " + regExp.exec(str) + "<br>";
                printStr += "str.replace(regExp,'***') : " + str.replace(regExp,'***') + "<br>";
                printStr += "----------------------------------------------------------<br>";

                printStr += "<h4>/^[js][\\w\\s]*[tx]$/ : 문자열 시작이 j, s이고 끝이 t, x인 문자열</h4>";
                regExp = /^[js][\w\s]*[tx]$/;
                // \w : 아무 문자 (공백은 제외)
                // \s : 공백 문자
                // * : 0 번 이상

                printStr += "regExp.test(str) : " + regExp.test(str) + "<br>";
                printStr += "regExp.exec(str) : " + regExp.exec(str) + "<br>";
                printStr += "str.replace(regExp,'***') : " + str.replace(regExp,'***') + "<br>";
                printStr += "----------------------------------------------------------<br>";


                printStr += "<h4> + : 한 번 이상 반복을 의미 </h4>";
                printStr += "<h4> * : 0 번 이상 반복을 의미 </h4>";
                // var str2 = "aaabbbaaa";
                var str2 = "bbbbbbbbbbbbb";

                regExp = /a+/; // a가 한 번 이상 반복되는 부분을 매칭
                regExp = /a*/; // a가 0 번 이상 반복되는 부분을 매칭

                printStr += "regExp.test(str2) : " + regExp.test(str2) + "<br>";
                printStr += "regExp.exec(str2) : " + regExp.exec(str2) + "<br>";
                printStr += "str.replace(regExp,'***') : " + str2.replace(regExp,'***') + "<br>";
                printStr += "----------------------------------------------------------<br>";

                printStr += "<h4> . : 개행 문자를 제외한 모든 단일 문자 매칭 </h4>"

                regExp = /....o/;
                str2 = "HelloWorld";
                printStr += "regExp.test(str2) : " + regExp.test(str2) + "<br>";
                printStr += "regExp.exec(str2) : " + regExp.exec(str2) + "<br>";
                printStr += "str.replace(regExp,'***') : " + str2.replace(regExp,'***') + "<br>";
                printStr += "----------------------------------------------------------<br>";

                printStr += "<h4> /^[0-9]+$/숫자만 입력한 경우 매칭</h4>";
                regExp = /^[0-9]+$/;

                printStr += "regExp.test('123455465475473') : " + regExp.test('123455465475473') + "<br>";

                printStr += "----------------------------------------------------------<br>";

                printStr += "<h4>/^[a-z]+$/ : 영어 소문자만 입력한 경우 매칭</h4>"
                printStr += "<h4>/^[A-Z]+$/ : 영어 대문자만 입력한 경우 매칭</h4>"

                printStr += "<h4>/^[A-Za-z0-9\\_]+$/ : 영어 대,소문자 + 숫ㅅ자 +'_'만 입력한 경우 매칭</h4>"
                regExp = /^[A-Za-z0-9\_]+$/;

                printStr += "regExp.test('kh_j_A_1006') : " + regExp.test('kh_j_A_1006') + "<br>";


                printStr += "<h4> /^[가 - 힣]+$/: 한글만 입력 </h4>"
                regExp = /^[가-힣]+$/;

                printStr += "regExp.test('도대체') : " + regExp.test('도대체') + "<br>";
                printStr += "----------------------------------------------------------<br>";


                area2.innerHTML = printStr;
            }
         </script>

         	<hr>

	<h3>메타문자를 이용한 주민번호 확인</h3>
	<label>주민번호 입력 : </label>
	<input type="text" id="pno">
	<button onclick="test3();">실행확인</button>
    <script>
        function test3() {
              var pno = document.getElementById("pno").value;
  
              var regExp = /[0-9][0-9][01][0-9][0-3][0-9]-[1-4][0-9][0-9][0-9][0-9][0-9][0-9]/;
  
              if(regExp.test(pno)){
                  alert("매칭됨");
              }else{
                  alert("매칭되지 않음");
              }
        }
     </script>

	<hr>

	<h3>추가 메타 문자</h3>
	<p>\d : 숫자 <br>
		\w : 아무 단어(숫자 포함)<br>
		\s : 공백문자(탭, 띄어쓰기, 줄바꿈)<br>
		\D : 숫자 아님<br>
		\W : 아무 단어 아님<br>
		\S : 공백 문자 아님</p>
	<label>주민번호 입력 : </label>
	<input type="text" id="pno2">
	<button onclick="test4();">실행확인</button>
	<div id="area7" class="area"></div>
	<script>
		function test4() {
            var pno2 = document.getElementById("pno2").value;
  
              var regExp = /\d\d[01]\d[0-3]\d-[1-4]\d\d\d\d\d\d/;
  
              if(regExp.test(pno2)){
                  alert("매칭됨");
              }else{
                  alert("매칭되지 않음");
              }

		}
	</script>
```

<br>

### 수량 문자
```html
	<h3>수량문자</h3>
	<p>a+ : a가 적어도 1개 이상<br>
		a* : a가 0개 또는 여러개<br>
		a? : a가 0개 또는 1개<br>
		a{5} : a가 5개 ex)aaaaa <br>
		a{2,5} : a가 2~5개 ex) aa, aaa, aaaa, aaaaa<br>
		a{2, } : a가 2개 이상 <br>
		a{ , 2} : a가 2개 이하 </p>
	<label>주민번호 입력 : </label>
	<input type="text" id="pno3">
	<button onclick="test5();">실행확인</button>
    <script>
        function test5() {
              var pno = document.getElementById("pno3").value;
  
              var regExp = /^\d{2}(0[1-9]|1[0-2])(0[1-9]|[1-2][0-9]|3[01])-[1234]\d{6}$/;
                      //   년도 , 3번째 자리가 0이면 4번째는 1~9, 3번째 자리가 1이면 4번째 자리는 0~2    

                /*  /^\d{2} : 년도 (아무숫자 2개)
                (0[1-9]|1[0-2]) : 월(3번째자리가0이면1~9, 1이면 0~2)
                (0[1-9]|[1-2][0-9]|3[01]) : 일(0이들어오면 1~9, 1이나2가 들어오면 0~9, 3이면 0혹은1)
                -[1234]\d{6}$/; : 성별[1~4] , 아무숫자6개 */
  
              if(regExp.test(pno)){
                  alert("매칭됨");
              }else{
                  alert("매칭되지 않음");
              }
  
  
        }
     </script>
```

<br>

### 플래그 문자
```html
<h3>플래그문자</h3>

<p>g : 전역비교를 수행한다<br>
    i : 대소문자를 가리지 않고 비교한다<br>
    m : 여러 줄의 검사를 수행한다</p>
```

<br><br>

## 회원가입 유효성 검사
```html
<!DOCTYPE html>
<html lang="ko">

<head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>23_정규표현식</title>
        <style>
                label {
                        display: inline-block;
                        width: 100px;
                }
        </style>

</head>

<body>
        <h1>회원가입 유효성 체크</h1>

        <form action="" method="post" onsubmit="return validate();">
                <!-- 제출시 행동 -->
                <label for="userid">* 유저아이디</label>
                <input type="text" name="userid" id="userid" required><br>

                <label for="pass">* 비밀번호</label>
                <input type="password" name="pass" id="pass" required><br>

                <label for="pass1">* 확인</label>
                <input type="password" name="pass1" id="pass1" required><br>

                <label for="name">* 이름</label>
                <input type="text" name="name" id="name" required><br>

                <label for="email">* 이메일</label>
                <input type="text" name="email" id="email" required><br>
                <br>

                <label for="tel1">전화번호</label>
                <input type="text" name="tel1" id="tel1" maxlength="3" size="3">-
                <input type="text" name="tel2" id="tel2" maxlength="4" size="4">-
                <input type="text" name="tel3" id="tel3" maxlength="4" size="4">
                <br><br>

                <label for="job">직업</label>
                <select name="job" id="job">
                        <option>개발자</option>
                        <option>프로그래머</option>
                        <option>자영업자</option>
                </select>
                <br><br>

                <label for="gender">성별</label>
                <input type="radio" name="gender" value="m"> 남
                <input type="radio" name="gender" value="f"> 여
                <br><br>

                취미<br>
                <input type="checkbox" name="hobby" value="reading"> 독서
                <input type="checkbox" name="hobby" value="drama"> 드라마보기
                <input type="checkbox" name="hobby" value="soccer"> 축구
                <br>
                <input type="checkbox" name="hobby" value="movie"> 영화보기
                <input type="checkbox" name="hobby" value="basket"> 드라마보기
                <input type="checkbox" name="hobby" value="game"> 게임<br>
                <br><br>
                <input type="reset" value="리셋">
                <input type="submit" value="완료">
        </form>

        <script>
            function validate(){ // 유효성 검사
                // 아이디 : 첫 글자는 소문자, 대/소문자+숫자로 총 4~12글자
                // 비밀번호 : 대/소문자 + 숫자 +!@#$%, 총 8~12글자
                //          + 비밀번호 확인과 일치하는지도 판별
                // 이메일 : /^[\w]{4,}@[\w]+(\.[\w]+){1,3}$/
                // 이름 : 한글 2~5글자 
                // 전화번호 :
                // - 앞자리 : 010, 011, 016, 017, 019
                // - 중간자리 : 아무 숫자 3~4글자
                // - 끝자리 : 아무 숫자 4글자

                var userid = document.getElementById("userid");
                var pass = document.getElementById("pass");
                var pass1 = document.getElementById("pass1");
                var name = document.getElementById("name");
                var email = document.getElementById("email");
                var tel1 = document.getElementById("tel1");
                var tel2 = document.getElementById("tel2");
                var tel3 = document.getElementById("tel3");
         
                // 아이디 : 첫 글자는 소문자, 대/소문자+숫자로 총 4~12글자
                var regExp = /^[a-z][A-Za-z\d]{3,11}$/;

                if(regExp.test(userid.value)){
                    //입력된 id가 정규식에 매칭되지 않으면
                    alert("아이디 : 첫 글자는 소문자, 대/소문자+숫자로 총 4~12글자");
                    userid.focus(); 
                    return false;
                }
             
             
                // 비밀번호 : 대/소문자 + 숫자 + !@#$%, 총 8~12글자
                //          + 비밀번호 확인과 일치하는지도 판별    
                regExp = /^[A-Za-z\d!@#$%]{8,12}$/; 

                if(!regExp.test(pass.value)){
                    // 비밀번호가 정규식에 매칭되지 않으면
                    alert("비밀번호 : 대/소문자 + 숫자 + !@#$%, 총 8~12글자");
                    pass.focus();
                    return false;
                }else{
                    if(pass.value != pass1.value){
                        // 비밀번호, 비밀번호 확인 불일치
                        alert("비밀번호, 비밀번호 확인 불일치");
                        pass1.focus();
                        return false;
                        }
                }

                // 이름 : 한글 2~5글자
                regExp = /^[가-힣]{2,5}$/;
                if(!regExp.test(name.value)){
                    // 비밀번호가 정규식에 매칭되지 않으면
                    alert("이름 : 한글 2~5글자");
                    name.focus(); 
                    return false;
                }

                // 이메일 : /^[\w]{4,}@[\w]+(\.[\w]+){1,3}$/
                regExp = /^[\w]{4,}@[\w]+(\.[\w]+){1,3}$/;
                if(!regExp.test(email.value)){
                    alert("이메일 : /^[\w]{4,}@[\w]+(\.[\w]+){1,3}$/");
                    email.focus(); 
                    return false;
                }


                // 전화번호 :
                // - 앞자리 : 010, 011, 016, 017, 019
                // - 중간자리 : 아무 숫자 3~4글자
                // - 끝자리 : 아무 숫자 4글자
                if( !(/^01[01679]$/.test(tel1.value) &&
                      /^\d{3,4}$/.test(tel2.value) &&
                      /^\d{4}$/.test(tel3.value) )){
                          // 전화번호 중 하나라도 매칭되지 않는 경우
                          alert("전화번호 형식이 알맞지 않습니다.");
                          tel3.focus();
                          return false;
                      }
            }
        </script>
</body>

</html>
```

<br><br>

## 복습 문제
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>복습문제_1</title>
    <style>
        #printArea{ height : 100px; }
    </style>
</head>
<body>
    <pre>
    fieldset, legend를 사용하여 입력, 출력 영역을 나누고
    출력 영역에는 id 속성 값이 printArea인 div를 추가.

    id가 printArea인 요소에 height를 100px로 설정

    입력 영역에 이름, 나이, 주소를 입력받는 input 태그와
    "입력" 버튼을 추가

    입력 버튼 클릭 시 inputFn() 함수가 호출됨.

    inputFn()은 입력 받은 이름, 나이, 주소를 
    전역변수로 선언된 info라는 변수에 객체 형태로 저장돼야함

    출력 영역 하단에는
    "출력", "초기화" 버튼을 ㅈ가성

    "출력" 버튼 클릭 시 printFn() 함수가 호출되며
    id가 printArea인 영역에 전역변수 info에 저장된 값이 모두 출력됨
    이름 : xxx
    나이 : xxx
    주소 : xxx

    단, 입력이 되지 않은 상태에서 출력 버튼을 클릭할 경우
    경고창으로 "입력부터 진행해주세요."를 출력
    </pre>

    <hr>

    <fieldset>
        <legend>입력</legend>
        이름 : <input id="name" class="inputInfo"> <br>
        나이 : <input type="number" id="age" class="inputInfo"> <br>
        주소 : <input id="addr" class="inputInfo"> <br>
        <button type="button" onclick="inputFn();">입력</button>
    </fieldset>


    <fieldset>
        <legend>출력</legend>
        <div id="printArea">
            <button type="button" onclick="printFn();">출력</button>
            <button type="button" id="resetBtn">초기화</button>
        </div>
    </fieldset>

    <script>
        var info;

        function inputFn(){
            // 입력 버튼 클릭 시 inputFn() 함수가 호출됨.
            // inputFn()은 입력받은 이름, 나이,주소를 전역변수로 선언된 info라는 변수에 객체 형태로 저장.
            info = {
                name : document.getElementById("name").value,
                age : document.getElementById("age").value,
                addr : document.getElementById("addr").value
            }
            
        }

        function printFn(){
            // "출력" 버튼 클릭 시 prinFn()함수가 호출되며
            // id가 printArea인 영역에 전역변수 info에 저장된 값이 모두 출력됨.
            // 이름 : xxx
            // 나이 : xxx
            // 주소 : xxx
            // 단, 입력이 되지 않은 상태에서 출력버튼을 클릭할 경우
            // 경고창으로 "입력부터 진행해주세요." 를 출력

            
            if(info == undefined){
                alert("입력부터 진행해 주세요.")
            }else{
                document.getElementById("printArea").innerHTML 
                    = "이름 : " + info.name + "<br>" +
                      "나이 : " + info.age + "<br>" +
                      "주소 : " + info.addr + "<br>";
            }
        }

        document.getElementById("resetBtn").onclick = function(){
            info = undefined;
            document.getElementById("printArea").innerHTML = "";

            // + 입력창 모두 삭제
           
            document.getElementById("name").value = "";
            document.getElementById("age").value = "";
            document.getElementById("addr").value = "";

        }

    </script>
</body>
</html>
```