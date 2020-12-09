---
title: "2020년 12월 09일"
excerpt: "Javascript-4"
search: true
categories: 
  - Academy
tags: 
  - Javascript
  - HTML
toc: true
---

## 객체
### 생성자 함수
- 객체를 생성할 때 사용하는 함수. <br/>

- 일반적인 함수와 다르게 호출 시 new 키워드를 사용하며
 호출 결과로 객체가 생성됨.<br/>
    
- 생성자 함수 사용 시 동일한 속성, 메소드를 지닌 객체 다수 생성 가능<br/>
```html
    <button type="button" id="btn1">확인하기</button>
    <div id="arae1" class="area"></div>
    <script>
        // 생성자 함수 정의
        // 생성자 함수 작성 시 함수명의 첫 글자는 대문자여야 한다.
        function Student(name, java, db, html5, css3, js){
            //속성 정의
            // 객체 내의 속성에 접근하기 위해서는 this 참조변수 사용
            this.name = name;
            this.java = java;
            this.db = db;
            this.html5 = html5;
            this.css3 = css3;
            this.js = js;

            //메소드 정의
            this.getSum = function() {
                return this.java+this.db+this.html5+this.css3+this.js;
            }

            // 오버라이딩도 가능함
            this.toString = function() {
                return this.name + " / " + this.java + " / " + this.db  
                    + " / " +  this.html5 + " / " + this.css3 + " / " + this.js;

            }
        }

        // btn1 버튼이 클릭된 경우
        document.getElementById("btn1").onclick = function() {
            var std1 = new Student("홍길동", 50, 60, 70, 80, 90);
            var std2 = new Student("김영희", 60, 70, 80, 90, 100);
            
            document.getElementById("arae1").innerHTML
              = std1 + " / 합계 : " + std1.getSum()  +"<br>";
            document.getElementById("arae1").innerHTML 
                += std2 + " / 합계 : " + std2.getSum()  +"<br>";
        }
    </script>
```

<br>

### 객체 배열
```html
    <h3>객체 배열</h3>
    <button type="button" id="btn2">확인하기</button>
    <div id="area2" class="area"></div>
    <script>
        document.getElementById("btn2").onclick = function() {
            
            // 빈 배열 선언
            var students = new Array();

            // 배열 마지막에 Student 객체를 추가
            students.push(new Student("홍길동", 50, 60, 70, 80, 90));
            students.push(new Student("고길동", 40, 63, 45, 43, 50));
            students.push(new Student("홍길순", 100, 68, 78, 45, 84));
            students.push(new Student("박철수", 55, 64, 79, 30, 94));
            students.push(new Student("김영희", 80, 61, 74, 68, 91));
            
            console.log(students);

            // id가 area2인 요소 얻어옴
            var area2 = document.getElementById("area2");

            area2.innerHTML = "";

            // for문을 이용하여 배열 내용 출력
            for(var std of students){
                area2.innerHTML += std + " / 합계 : " + std.getSum() + "<br>";
            }
        }

    </script>
    
    <hr>
    음식명 : <input type="text" class="inputFoodInfo"> <br>
    가격 : <input type="number" class="inputFoodInfo"> 
    
    <button type="button" id="add">추가</button>

    <div id="printArea"></div>

    <script>
        // 생성자 함수 정의
        function Food(inputFoodInfo) {

            // 음식명이 작성된 input 요소의 값을 얻어와
            // 객체의 fName에 대입
            this.fName = inputFoodInfo[0].value;
            this.price = inputFoodInfo[1].value;

            // toString() 오버라이딩
            this.toString = function() {
                return "음식명 : " + this.fName + " / 가격 : " + this.price + "<br>";
            }
        }

        // id가 add인 요소가 클릭된 경우
        document.getElementById("add").onclick = function() {
            // class가 inputFoodInfo인 요소를 모두 얻어와 배열로 반환
            var inputFoodInfo 
                = document.getElementsByClassName("inputFoodInfo");

            // Food 생성자 함수를 통해 객체를 생성할 때
            // 매개변수로 inputFoodInfo 배열 전달
            var food = new Food(inputFoodInfo);

            // 생성된 Food 객체를
            // id가 printArea인 요소에 출력
            document.getElementById("printArea").innerHTML += food;
        }
    </script>
```

<br>

## 연습문제
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>연습문제</title>
    <style>
        #table1{
            border : 1px solid black;
            width: 100%;
        }
        .col1 {
            border : 1px solid lightgray;
        }

        .divs{
            margin : 10px;
            width : 30%;
            height: 400px;
            display: inline-block;
            float : left;
        }

        #tableN{
            text-align: center;
        }
        
        .tds {
            border : 1px solid lightgray;
        }

        td {
            /* 출력되는 이름, 점수를 가운데 정렬 */
            text-align: center;
        }
        
    </style>
</head>
<body>
    <div id="left" class="divs">

        <h3 id="tableN">학생 점수 관리 프로그램</h3>
        <form>
            <table id="table1" border="1px">
                <tr id="row1">
                    <th class="col1">이름</th>
                    <th class="col1">Java</th>
                    <th class="col1">Oracle</th>
                    <th class="col1">JDBC</th>
                    <th class="col1">HTML</th>
                    <th class="col1">합계</th>
                    <th class="col1">평균</th>
                </tr>
            </table>
        </form>

    </div>

    <div id="right" class="divs">
        <form>
            <table id="table2">
                <tr>
                    <td>이름 : </td>
                    <td><input type="text" class="inputInfo"></td>
                </tr>
                <tr>
                    <td>Java : </td>
                    <td><input type="text" class="inputInfo"></td>
                </tr>
                <tr>
                    <td>Oracle : </td>
                    <td><input type="text" class="inputInfo"></td>
                </tr>
                <tr>
                    <td>JDBC : </td>
                    <td><input type="text" class="inputInfo"></td>
                </tr>
                <tr>
                    <td>HTML : </td>
                    <td><input type="text" class="inputInfo"></td>
                    <td><button type="button" id="btn">추가</button></td>
                </tr>
            </table>
        </form>

        <script>
            
            function Score(inputInfo){
                this.name = inputInfo[0].value;
                this.java = Number(inputInfo[1].value);
                this.oracle = Number(inputInfo[2].value);
                this.jdbc = Number(inputInfo[3].value);
                this.html = Number(inputInfo[4].value);

                this.getSum = function() {
                    return this.java + this.oracle + this.jdbc + this.html;
                }

                this.getAgv = function() {
                    return this.getSum() / 4;
                }
            }

            document.getElementById("btn").onclick = function(){
                var inputInfo = document.getElementsByClassName("inputInfo");
                var score = new Score(inputInfo);

                with(score){
                document.getElementById("table1").innerHTML +=
                '<tr>' +
                '<td class="tds">' + name + '</td>' +
                '<td class="tds">' + java + '</td>' +
                '<td class="tds">' + oracle + '</td>' +
                '<td class="tds">' + jdbc + '</td>' +
                '<td class="tds">' + html + '</td>' +
                '<td class="tds">' + getSum() + '</td>' +
                '<td class="tds">' + getAgv() + '</td>' +
                '</tr>'
                }
            }
        </script>
    </div>
</body>
</html>
```

![20201209_01](https://user-images.githubusercontent.com/70805241/101616289-8cddaf80-3a52-11eb-9759-f80ec7bba504.gif)

<br><br>


## 내장객체
```html
	<h1>내장객체</h1>
	<h3>Object 객체</h3>
	<p>자바스크립트의 가장 기본적인 내장객체이다.<br>
		Object생성자함수를 통해 만들어진 인스턴스이다.</p>
	<button onclick="test1();">실행확인</button>
	<script>
		function test1() {
			//Object를 생성하는 방법
			var obj1 = {};
			var obj2 = new Object();

			console.log(obj1);
			console.log(obj2);
		}
	</script>
```
<br>

### Object 객체의 기본 메소드
```html
	<h3>Object 객체의 기본 메소드</h3>
	<h4>hasOwnProperty()와 propertyIsEnumerable()메소드</h4>
	<button onclick="test2()">실행확인</button>
	<div id="area1" class="area"></div>
	<script>
		function test2() {
			var object = {
				//name:"asd",
				age: 20
			};

			var area1 = document.getElementById("area1");

			//속성을 가지고 있는지 확인하는 메소드로 true와 false를 리턴한다.
			area1.innerHTML += "name 속성을 가지고 있나? : " + object.hasOwnProperty('name') + "<br>";
			area1.innerHTML += "age 속성을 가지고 있나? : " + object.hasOwnProperty('age') + "<br>";

			area1.innerHTML += "age 속성은 반복문을 사용할 수 있나? : " + object.propertyIsEnumerable('age') + "<br>";

		}

	</script>
```

<br>

### constructor()메소드
```html
	<h4>constructor()메소드</h4>
	<p>Object가 가지는 객체의 생성자 메소드이다.<br>
		자료형을 검사할 때 유용하게 사용할 수 있다.</p>
	<button onclick="test4();">실행확인</button>
	<div id="area3" class="area"></div>
	<script>
		function test4() {
			var num1 = 120;
			var num2 = new Number(120);

			var area3 = document.getElementById('area3');

			console.log("num1 : " + num1 + ", num2 : " + num2);
			console.log("num1의 자료형 : " + typeof (num1));
			console.log("num2의 자료형 : " + typeof (num2));
			console.log("num1 + num2 = " + (num1 + num2));


			area3.innerHTML = "num1의 자료형 : ";
			if (typeof (num1) == 'number') {
				area3.innerHTML += "숫자 <br>";
			} else {
				area3.innerHTML += "숫자아님 <br>";
			}

			area3.innerHTML += "num2의 자료형 : ";
			if (typeof (num2) == 'number') {
				area3.innerHTML += "숫자 <br>";
			} else {
				area3.innerHTML += "숫자아님 <br>";
			}


			//두 대상을 같은 자료형으로 취급하고 싶을 때 constructor메소드를 사용한다.

			area3.innerHTML += "num1.constructor == Number :  " + (num1.constructor == Number) + "<br>";
			area3.innerHTML += "num2.constructor == Number :  " + (num2.constructor == Number);

		}
	</script>
```

<br>

### Number 객체
```html
	<h3>Number 객체</h3>
	<button onclick="test5();">실행확인</button>
	<p>toFixed Test()</p>
	<div id="area6" class="area"></div>
	<script>
		function test5() {
			var num1 = 123.456789;
			var num2 = 123;

			var area6 = document.getElementById("area6");

			//숫자를 고정 소수점 자리로 나타낸 문자열을 만든다.
			area6.innerHTML += "num1의 기본값 : " + num1 + "<br>";
			area6.innerHTML += "num2의 기본값 : " + num2 + "<br>";
			area6.innerHTML += "num1.toFixed(1) : " + num1.toFixed(1) + "<br>";
			area6.innerHTML += "num1.toFixed(4) : " + num1.toFixed(4) + "<br>";
			area6.innerHTML += "num2.toFixed(4) : " + num2.toFixed(4) + "<br>";
			area6.innerHTML += "그럼 자료형은 ? : " + typeof (num1.toFixed(1)) + "<br>";

		}

	</script>
```

<br>

### String객체의 HTML관련 메소드
```html
	<h3>String객체의 HTML관련 메소드</h3>
	<button onclick="test6();">실행확인</button>
	<p>String 객체의 메소드는 크게 기본메소드와 HTML관련 메소드로 구분할 수 있다.</p>
	<div id="area7" class="area-big"></div>
	<script>
		function test6() {
			var str = "javascript";

			var area7 = document.getElementById("area7");

			area7.innerHTML += "기본값 : " + str + "<br>";
			area7.innerHTML += "anchor() : " + str.anchor() + "<br>";
			area7.innerHTML += "big() : " + str.big() + "<br>";
			area7.innerHTML += "bold() : " + str.bold() + "<br>";
			area7.innerHTML += "fontcolor('red') : " + str.fontcolor("red") + "<br>";
			area7.innerHTML += "fontsize(20) : " + str.fontsize(20) + "<br>";
			area7.innerHTML += "italics() : " + str.italics() + "<br>";
			area7.innerHTML += "link() : " + str.link("http://www.naver.com") + "<br>";
			area7.innerHTML += "small() : " + str.small() + "<br>";
			area7.innerHTML += "strike() : " + str.strike() + "<br>";
			area7.innerHTML += "sub() : " + str.sub() + "<br>";
			area7.innerHTML += "sup() : " + str.sup() + "<br>";
		}



	</script>
```

<br>


### Date 객체
```html
	<h3>Date객체</h3>
	<p>날짜를 관리하는 객체</p>
	<button onclick="test7();">실행확인</button>
	<div id="area8" class="area area-big"></div>
	<script>
		function test7() {
			//현재 시간
			var area8 = document.getElementById("area8");

			//GMT 시
			// 그리니치 평균시(Greenwich Mean Time, GMT)는 런던을 기점

			// 현재 시간
			var date1 = new Date();

			// new Date(dateString); 
			var date2 = new Date('June 18');
			var date3 = new Date('June 18, 2020');
			var date4 = new Date('June 18, 2020 09:30:23');


			area8.innerHTML += "Date() : " + date1 + "<br>";
			area8.innerHTML += "Date('June 15') : " + date2 + "<br>";
			area8.innerHTML += "Date('June 18, 2020'') : " + date3 + "<br>";
			area8.innerHTML += "Date('June 18, 2020 09:30:23') : " + date4 + "<br>";

			//UTC시간
			// new Date(year, month-1, day, hours, minutes, seconds, milliseconds)
			var date5 = new Date(2020, 5, 18);
			var date6 = new Date(2020, 5, 18, 9, 30, 23);
			var date7 = new Date(2020, 5, 18, 9, 30, 23, 1);

			area8.innerHTML += "Date(2020, 0, 15) : " + date5 + "<br>";
			area8.innerHTML += "Date(2020, 0, 15, 17, 30, 23) : " + date6 + "<br>";
			area8.innerHTML += "Date(2020, 0, 15, 17, 30, 23, 1) : " + date7 + "<br>";
		}
	</script>



    <h3>Date 객체의 메소드</h3>
	<button onclick="test8();">실행확인</button>
	<div id="area9" class="area-big"></div>
	<script>
		function test8() {
			var date = new Date();

			var area9 = document.getElementById("area9");

			area9.innerHTML += "getFullYear() : " + date.getFullYear() + "<br>";
			area9.innerHTML += "getMonth() : " + (date.getMonth() + 1) + "<br>";
			area9.innerHTML += "getDate() : " + date.getDate() + "<br>";
			area9.innerHTML += "getDay() : " + date.getDay() + "<br>";
			area9.innerHTML += "getHours() : " + date.getHours() + "<br>";
			area9.innerHTML += "getMinutes() : " + date.getMinutes() + "<br>";
			area9.innerHTML += "getSeconds() : " + date.getSeconds() + "<br>";
			area9.innerHTML += "getMilliseconds() : " + date.getMilliseconds() + "<br>";
			area9.innerHTML += "getTimezoneOffset() : " + date.getTimezoneOffset() + "<br>";
			area9.innerHTML += "getTime() : " + date.getTime() + "<br>";


			//1970년 1월 1일 자정 기준 밀리세컨 단위로 객체 생성
			var date2 = new Date(new Date(2018, 0, 15, 17, 30, 23, 1).getTime());

			console.log(date2);


			// 날짜 사이의 간격 구하기
			var now = new Date();
			var start = new Date('2020, 10, 6');
			var end = new Date('2011, 3, 11');

			var interval1 = now.getTime() - start.getTime();
			var interval2 = end.getTime() - now.getTime();

			var time1 = Math.floor(interval1 / (1000 * 60 * 60 * 24));
			var time2 = Math.ceil(interval2 / (1000 * 60 * 60 * 24));

			alert("개강한지 " + time1 + "일 째 입니다. \n"
				+ "종강까지 " + time2 + "일 남았습니다.");
		}
	</script>
```

### Window 객체
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>18_내장객체2</title>
    <style>
        .area{
            width : 300px;
            height: 100px;
            border : 1px solid black;
            font-size: 50px;
            font-weight: bold;

        }

        .long{
            width : 100px;
            height : 300px;
            border : 1px solid black;

            /* 내용이 요소의 크기를 넘어섰을 경우에 대한 스타일 */
            overflow : scroll; /* overflow는 기본값이 visible */
        }

        .big{
            height : 400px;
            border : 1px solid black;
        }
    </style>
</head>
<body>
    <h1>Window 객체</h1>
    <pre>
    - 브라우저 창 자체를 나타내는 객체
    
    - Window 객체는 자바스크립트의 최상위 객체이며 BOM, DOM으로 나누어진다.
    
    BOM(Browser Object Model) : location, navigator, history, screen;
    DOM(Document Object Model) : document
    </pre>

    <hr>

    <h3>window 객체의 timer 관련 메소드</h3>

    <h4>setTimeout() 메소드 : 일정 시간 후 특정 내용을 한 번만 수행한다.</h4>

    <button type="button" id="btn1">확인하기</button>
    <script>
        document.getElementById("btn1").onclick = function() {
            var newWindow = window.open(); // 새창 열기

            newWindow.alert("확인 버튼을 클릭하면 3초 후에 이 페이지는 종료됩니다.");
            // 새창에 경고창 띄우기

            //window.setTimeout(함수, 지연시간(ms));
            window.setTimeout( function(){
                newWindow.close(); // 새창 닫기
            }, 3000); // 3000 = 3초


        }
    </script>

    <hr>

    <h4>setInterval() 메소드 : 일정 시간마다 특정 내용을 반복 수행함.</h4>
    <button type="button" id="btn2">확인하기</button>
    <div id="area2" class="area"></div>

    <script>
        document.getElementById("btn2").onclick = function() {
            var area2 = document.getElementById("area2");

            // 현재 시간이 기록된 객체 Date 생성
            var date = new Date();
            // 14 : 5 : 초

            area2.innerHTML = date.getHours() + " : " 
                            + date.getMinutes() + " : " 
                            + date.getSeconds();

            //window.setInterval(함수, 지연시간(ms));
            window.setInterval(function(){
                var date = new Date();
            // 14 : 5 : 초

            area2.innerHTML = date.getHours() + " : " 
                            + date.getMinutes() + " : " 
                            + date.getSeconds();

            //window.setInterval(함수, 지연시간(ms));
            window.setInterval(function(){}, 1000);
            }, 1000);
        }
    </script>

    <hr>

    <h4>clearInterval() 메소드 : 반복 수행중인 setInterval을 멈추게함.</h4>

    <button type="button" onclick="start();">시작</button>
    <button type="button" onclick="stop_stopwatch();">정지</button>
    <button type="button" onclick="record();">기록</button>

    <div id="area3" class="area"></div>
    <div id="area3-2" class="long"></div>
    <script>
        var stop = false;
        var stopwatch;

        // 스톱워치 시작
        function start(){
            stop = false;

            // 이전에 기록된 시간을 모두 삭제
            document.getElementById("area3-2").innerHTML = "";

            // 스톱워치가 출력될 요소 얻어오기
            var area3 = document.getElementById("area3");

            var count = 0;

            if(!stop){
                stopwatch = window.setInterval( function(){
                    area3.innerHTML = parseInt(count / 100 / 60 % 60)
                                        + " : " + parseInt(count / 100 % 60)
                                        + " : " + parseInt(count % 100);
                    count++;
                }, 10 );  
            }
        }

        // 스톱워치 정지
        function stop_stopwatch(){
            window.clearInterval(stopwatch);
            stop = true;
        }

        // 시간 기록
        function record(){
            document.getElementById("area3-2").innerHTML
              += document.getElementById("area3").innerHTML + "<br>";
        }
    </script>
</body>
</html>
```

### location 객체
```html
    <h3>location 객체</h3>
    <pre>
    브라우저의 주소 표시줄과 관련된 객체
    </pre>

    <button type="button" id="btn4">location확인</button>
    <div id="area4" class="big"></div>

    <script>
        document.getElementById("btn4").onclick = function() {
            
            //for in 구문 : 객체 내 속성을 순서대로 접근하는 구문
            for(var key in location) {
                document.getElementById("area4").innerHTML
                    += key + " : " + location[key] + "<br>";
            }
        }
    </script>

    <button type="button" onclick="location.href = 'https://www.naver.com'">네이버로 이동</button>

    <button type="button" onclick="location= 'https://www.naver.com'">네이버로 이동</button>

    <button type="button" onclick="location.replace('https://www.naver.com')">네이버로 이동</button>
    <!-- replace()는 현재 화면 자체를 바꾸는 메소드로 뒤로가기가 안됨. -->

    <br><br><br>

    <!-- 새로고침(==갱신) -->
    <button type="button" onclick="location.href = location.href">새로고침</button>
    <button type="button" onclick="location.href = location">새로고침</button>

    <!-- reload() : 현재 페이지 위치를 유지한채로 새로고침-->
    <button type="button" onclick="location.reload();">reload()</button>
```

<br>

## 내장함수
```html
<!DOCTYPE html>
<html lang="ko">

<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>19_내장함수</title>
	<style>
		.area {
			background: lightgray;
			border: 1px solid black;
			height: 150px;
		}

		.bigarea {
			background: lightgray;
			border: 1px solid black;
			height: 350px;
		}
	</style>
</head>

<body>
	<h1>JavaScript 내장함수</h1>
	<h3>인코딩과 디코딩에 관련된 내장함수</h3>
	<h4>웹 통신 시 유니코드 문자는 오작동을 일으킬 문제가 있어 인코딩 필요</h4>

	<p>escape : 영문 알파벳, 숫자, 일부 특수문자(@, *, -, _, ., /)를 제외한 모든 문자를 인코딩한다.<br>
		encodeURI : escape에서 인터넷주소에 사용되는 일부 특수문자(:, ;, /, =, ?, &)는 변환하지 않는다.<br>
		encodeURIComponent : 알파벳과 숫자를 제외한 모든 문자를 인코딩한다. (UTF-8방식)<br>
		<br>
		unescape : escape로 인코딩된 문자를 디코딩한다.<br>
		decodeURI : encodeURI로 인코딩된 문자를 디코딩한다.<br>
		decodeURIComponent : encodeURIComponent로 인코딩된 문자를 디코딩한다.
	</p>

	<button onclick="test1();">실행확인</button>
	<div id="area1" class="area"></div>
	<script>
		function test1() {
			var URI = 'http://www.naver.com?test=한글입니다.';

			var esURI = escape(URI);
			var enURI = encodeURI(URI);
			var enURIC = encodeURIComponent(URI);

			var area1 = document.getElementById("area1");

			area1.innerHTML = "";
			area1.innerHTML += "원본 URI : " + URI + "<br>";

			area1.innerHTML += "escape() : " + esURI + "<br>";
			area1.innerHTML += "encodeURI() : " + enURI + "<br>";
			area1.innerHTML += "encodeURIComponent() : " + enURIC + "<br>";

			area1.innerHTML += "unescape() : " + unescape(esURI) + "<br>";
			area1.innerHTML += "decodeURI() : " + decodeURI(enURI) + "<br>";
			area1.innerHTML += "decodeURIComponent() : " + decodeURIComponent(enURIC) + "<br>";
		}
	</script>

	<hr>

	<h3>eval()함수</h3>
	<!-- evaluate / evaluation -->
	<p>문자열을 자바스크립트 코드로 변환해 실행하는 함수이다.</p>
	<label>숫자1 입력 : </label><input type="text" id="number1"><br>
	<label>숫자2 입력 : </label><input type="text" id="number2"><br>
	<button onclick="test2();">실행확인</button>
	<div id="area2" class="area"></div>
	<script>
		function test2() {
			var testEval = "";

			testEval += "var number1 = parseInt(document.getElementById('number1').value);";
			testEval += "var number2 = parseInt(document.getElementById('number2').value);";
			testEval += "var sum = 0;";
			testEval += "sum = number1 + number2;";
			testEval += "document.getElementById('area2').innerHTML += number1 + '과 ' + number2 + '의 합은 : ' + sum + '입니다.';";

			eval(testEval);
			alert(number1 + '과 ' + number2 + '의 합은 뭘까요??');
		}
	</script>

</body>
</html>
```

<br>

## DOM(Document Object Model)
- HTML 내용이 표시되는 문서 자체를 나타내는 객체<br/>

- HTML에 작성된 태그들을 구조화(트리 구조)하였을 때
      각각의 태그 요소들을 노드(Node)라고 한다.<br/>
    
 - 노드 종류 : 텍스트 노드가 있는 노드(h1, h2, pre, p, div, span...)<br/>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                 텍스트 노드가 없는 노드(img, input, hr, br ...)


<br>

### 텍스트 노드가 있는 노드 생성
```html
    <h3>텍스트노드가 
        <span style="font-weight: bold; color : red;">있는 노드</span>
         생성하기</h3>

    <button type="button" id="btn1">실행확인</button>
    <div id="area1" class="area"></div>

    <script>
        document.getElementById("btn1").onclick = function() {
            // document.getElementById("area1").innerHTML 
            //   += "<h4>안녕하세요.</h4>";

            // element 생성
            var el = document.createElement("h4");
            // h4 요소가 만들어져 el 변수에 저장
            //  -> 아직 html 문서에는 배치되지 않아
            //     화면에는 보이지 않음

            // textnode 생성
            var text = document.createTextNode("안녕하세요?");

            // 노드 연결
            el.appendChild(text);
            // el(h3태그) 내부에 text("안녕하세요")를 content 마지막에 추가
            //  -> <h4>안녕하세요?</h4> 형태의 요소가 완성됨

            // 완성된 요소 el을 area1의 content 마지막에 추가하기

            document.getElementById("area1").appendChild(el);
            //area1 요소에 el을 노드 연결
         
        }
    </script>
```

<br>

### 텍스트 노드가 없는 노드 생성
```html
    <h3>텍스트 노드가 
        <span style="color : red;">없는 노드</span> 생성</h3>

        <button type="button" id="btn2">실행확인</button>
        <div id="area2" class="area">
            <!-- <img src="logo.jpg" width="200" height="100"> -->
        </div>
        <script>
            document.getElementById("btn2").onclick = function() {
                // img 요소 생성
                var img = document.createElement("img");

                // 생성된 img 요소에 속성 추가
                img.src = "logo.jpg";
                img.width = "200";
                img.height = "100";

                // img에 없는 속성 추가해보기
                // img.myProperty1 = "aaa"; -> 추가 불가능
                img.setAttribute("myProperty2", "bbb");

                //area2 요소와 img 요소 노드 연결
                document.getElementById("area2").appendChild(img);
            }
        </script>
```

<br>

### 노드 삭제
```html
        <h3>노드 삭제</h3>
        <pre>

        </pre>
        <button type="button" id="btn3">노드삭제</button>

        <script>
            document.getElementById("btn3").onclick = function() {
                document.getElementById("area2").lastChild.remove();
                // lastChild : 요소의 마지막 자식을 선택
            }
        </script>
```

<br>

### 노드 추가/삭제
```html
        <h3>노드 추가/삭제</h3>
        <button type="button" id="add">추가</button>
        <button type="button" id="sum">합계</button>

        <table>
            <tbody id="wrapper">
                <tr>
                    <td>입력 : </td>
                    <td>
                        <input type="number" class="inputNumbers">
                    </td>
                </tr>

            </tbody>
        </table>

        <!-- 합계 출력 영역 -->
        <div id="printArea"></div>

        <script>
            // 추가 버튼 클릭 시
            document.getElementById("add").onclick = function(){
    
                var tr = document.createElement("tr");
                
                // td1에 textNode("입력") 추가
                var td1 = document.createElement("td");
                var text = document.createTextNode("입력");
                td1.appendChild(text);
    
                // td2에 input요소 추가
                var td2 = document.createElement("td");
                var input = document.createElement("input");
                
                // input 태그에 필요한 속성 추가
                input.setAttribute("type", "number");
                input.setAttribute("class", "inputNumbers");
    
                td2.appendChild(input);
                
                // td3에 button요소 추가
                var td3 = document.createElement("td");
                var btn = document.createElement("button");
    
                // button에 필요한 속성 추가
                btn.setAttribute("type", "button");
                btn.setAttribute("onclick", "deleteRow(this)");
                //deleteRow(this)에서 this란?
                // -> onclick 이벤트가 발생한 요소
                //  --> 쉽게 말해서 클릭된 버튼 그 자체
    
                // button에 삭제 textnode 추가
                btn.appendChild( document.createTextNode("삭제") );
    
                td3.appendChild(btn);
    
                // tr에 추가
                tr.appendChild(td1);
                tr.appendChild(td2);
                tr.appendChild(td3);
    
                // tbody(wrapper)에 tr 추가
                document.getElementById("wrapper").appendChild(tr);
    
            }

            // 삭제함수
            function deleteRow(btn){
                // btn == 클릭된 버튼 요소
                // .parentNode : 해당 요소의 부모 요소 선택
                console.log(btn); // 삭제버튼
                console.log(btn.parentNode); //td
                console.log(btn.parentNode.parentNode); //tr
                btn.parentNode.parentNode.remove();
            }

            document.getElementById("sum").onclick = function(){
                var inputNumbers = document.getElementsByClassName("inputNumbers");
                var sum = 0;
                

                for(var i of inputNumbers){
                    sum += Number(i.value);
                }
                
                document.getElementById("printArea").innerHTML
                    = "<h3> 합계 : " + sum + "</h3>";
                
            }

        </script>
```