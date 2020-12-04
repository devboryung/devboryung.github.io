---
title: "2020년 12월 04일"
excerpt: "Javascript-1"
search: true
categories: 
  - Academy
tags: 
  - Javascript
  - HTML
toc: true
---
## 스크립트 언어란?
기존에 이미 존재하는 프로그램의 동작을  사용자의 요구에 맞게 수행되도록 제어하는 용도의 언어이다. <br/>
소스코드의 수정이 많은 곳에서 사용하기 적합하다.(Interpreter 방식)<br/>
    *Interpreter란? 소스코드를 한 줄씩 읽어 바로 실행하는 방식.<br/>
스크립트 언어들은 빠르게 배우고 짧은 소스코드 파일로 상호작용을 하도록 고안되었다.
{: .notice--success}
<br/>

## Javascript란?
웹 브라우저에서 많이 사용하는 인터프리터 방식의 객체 지향 프로그래밍 언어.<br/>
HTML 문서 내에 기술되고 , HTML과 함께 웹 브라우저에 의해서 해석됨.<br/>
 -> 클라이언트 사이드 스크립트 언어라고 한다.(클라이언트 쪽에서 해석되는 스크립트 언어)<br/>
*(참고)JSP,파이썬 -> 서버 사이드 스크립트 언어(서버에서 해석되는 스크립트 언어)<br/>
*(참고)Javascript란 ECMA 스크립트 표준을 따르는 대표적인 웹 기술이다.<br/>
{: .notice--success}
<br/>

### Javascript 작성 방법
    자바스크립트 소스 코드는 함수 단위로 작성하고, 이벤트 속성을 이용하여 구동되게한다. 
    이벤트 속성은 태그마다 차이가 있음. 
    -> on이벤트명 = "실행할 함수명([전달값]);" 

<br/><br/>

## inline 방식
태그에 직접 간단한 소스코드를 작성해서 실행되게 하는 방법.    

<br/>

```html
<body>
<button type="button" 
            onclick="window.alert('버튼이 클릭되어서 경고창이 출력됨.')" >경고창 출력</button>
    <button type="button" 
            onclick="console.log('버튼이 클릭되어서 콘솔창에 내용이 출력됨')">콘솔창 출력</button>
    <!-- console.log = 브라우저의 console창에 결과가 뜬다.  클라이언트 사이드이기 때문에 console창에서 소스코드 바로 작성,실행 가능하다 -->

    <h4 onmouseover="style.backgroundColor='red'" 
        onmouseout="style.backgroundColor='yellow'">마우스 오버/ 마우스아웃</h4>

    <!-- 마우스 오버 : 맘우스 아웃을 작성 안 하면 한 번 마우스가 올라가면 마우스를 떼도 계속 변경된 상태를 유지한다. 
         마우스 아웃 : 마우스를 올린 뒤 떼면 계속 변경된 상태를 유지한다.-->
</body>

```

<br/><br/>


## internal 방식

자바스크립트 내용을 &lt;script&gt; 태그에 작성하여 실행되게하는 방식이다.
script태그는 head, body 태그 내부에 작성함. (보통 body태그 맨 마지막에 사용하는 것을 권장함.)   

```html
<body>
<button type="button" onclick="testFn();">함수 실행 확인용 버튼.</button>

    <script type="text/javascript">
     /* type="text/javascript"  = 기본값  (텍스트로 된 자바스크립트 라는 의미이다.)*/
        function testFn(){
            window.alert("testFn() 함수 실행 확인");
            console.log("testFn() 함수 실행 확인");
        }
    </script>
</body>
```


<br/><br/>

## external 방식
별도의 .js 파일을 생성하여 해당 파일에 자바스크립트 코드를 작성하고 가져다 사용하는 방법으로, js폴더를 만들어 내부에 sample.js 파일을 생성 후 함수를 작성한다.<br/>
실제 개발에서 가장 많이 사용되는 방법이며 , 언어와 역할을 분리하기,서로 분업이 쉬워진다.

```js
// 외부에 작성된 자바스크립트

function sampleFn(msg){
    window.alert("전달받은 값 : " + msg);
    // window. 생략 가능
    console.log("전달받은 값 : " + msg);
}

// 해당 js파일을 사용하고자 하는 html 파일의
// head 태그에 script 연결 코드 작성.
```

```html

<head>
    <!-- <head>에 external 방식의 js파일 연결 -->
    <script src="js/sample.js"></script>
</head>

<body>
<button type="button" onclick="sampleFn('배고파파파')">버튼</button>
</body>
```



<br/><br/>

## 데이터 출력
자바스크립트 코드가 function()에 작성되지 않고  바로 script 태그 아래에 작성되면 <br/>
브라우저가 html 코드를 위에서 아래로 순서대로 해석하다가  script 코드를 만나면 바로 해당 코드를 수행함.
<br/>

```html
<script>
        document.write("document.write()로 출력한 내용");
        alert("출력문구가 출력되었습니다.");
    </script>

    <br>
    <button type="button" onclick="writeFn();">출력</button>
    <script>
        function writeFn(){
            document.write("HTML 문서가 완전히 로드된 후 " 
                            +" document.write()를 호출하게 되면" 
                            +"기존에 HTML 문서에 작성되어있던 내용이 모두 삭제됨.");
        }
        // 기존에 로드된 내용이 다 사라지고  document.write에 작성된 내용만 남기 때문에 잘 사용하지 않음..
    </script>
```


<br/><br/>

## innerText /innerHTML 를 이용한 내용 변경
**innerText : 자바스크립트에서 요소의 내용을 읽거나 변경할 때 innerText라는 속성을 사용함.
태그가 인식이 안 되며 텍스트 형태로 출력된다.<br/>
**innerHTML : 요소의 내용을 변경할 때 내용에 포함된 태그를 
실제 태그로 인식시키기 위해서는 innerHTML을 사용해야함.

```html
 <button type="button" onclick="check1();">innerText 확인</button>
    <button type="button" onclick="check2();">innerHTML 확인</button>
    <script>
        function check1(){
            // 요소의 아이디가 "area1"인 요소를 선택하여 얻어와
            // area1 이라는 변수에 저장한다. 
            var area1 = document.getElementById("area1");

            // 정말 id가 area1인 요소를 얻어 왔는지 확인한다.
            console.log(area1);

            // 얻어온 요소에서 작성된 내용만 얻어와 출력하기.
            console.log(area1.innerText);   // getter처럼 얻어오는 용도로 쓰임.

            // 요소에 작성된 내용 변경
            //area1.innerText = "innerText로 인해 내용이 변화됨." //setter처럼 값을 세팅하는 용도로 쓰임.
            
            area1.innerText = "<h4>innerText로 인해 내용이 변화됨.</h4>"
            // <h4></h4> 태그로 인식 안되고 그대로 출력됨.
        
        }
        function check2(){
            var area1 = document.getElementById("area1");

            // innerHTML을 getter로 사용
            console.log(area1.innerHTML);
            // getter로 사용 시 innerText와 같은 효과(내용만 읽어옴.)

            // innerHTML을 setter로 사용
            area1.innerHTML = "<h4>InnerHTML로 인해 내용이 변화됨.</h4>"
            // 태그가 인식해서 글씨가 굵어짐.
        }
    </script>


```
<br/><br/>


## console.log()
브라우저 개발자도구 -> console 화면에 내용을 출력하고자 할 때 사용.<br/>
(주로 디버깅할 때 사용.)

```html
<button type="button" onclick="printConsole(10, 20);">console.log() 출력</button>

    <script>
        function printConsole(x, y){
            console.log(x + "+" + y + "=" + (x+y));
        }
    </script>

```
<br/><br/>




## window.confirm()을 이용한 데이터 입력
어떤 질문에 "예 / 아니오" 의 결과를 얻고자 할 때 사용함.<br/>
경고창에 "확인","취소" 버튼이 나타나며<br/>
"확인" 클릭 시 true, "취소" 클릭 시 false 가 반환된다.   <br/>

```html
<div id="area2"></div>

    <button type="button" onclick="checkSleepy();">졸린지 확인</button>

    <script>
        function checkSleepy(){
           //console.log(window.confirm("여기에 출력할 내용 작성"));
           // window.confirm 먼저 실행 후 console.log 실행 된다.

           var result = window.confirm("졸리면 확인, 아니면 취소를 누르세요.");
            // window.confirm의 결과값을 result라는 변수에 저장함.

            var str="";  // 결과를 저장할 변수 선언 후 빈 문자열을 저장하였다.

            if(result){
                str ="졸려 죽겠다...";
            }else{
                str ="왜 안 졸리지?";
            }

            // 아이디가 "area2"인 요소를 얻어와 area2 변수에 저장
            var area2 = document.getElementById("area2");

            // area2에 h3 태그로 str에 저장된 내용을 출력 + 글자 빨간색

            area2.innerHTML = "<h3>"+str+"</h3>";

            area2.style.color = "red";

            /*  area2에 글자색을 빨간색으로 하는 방법
            1) style 태그에서 #area2을 선택하여 색 변경
            2) inline style을 이용하여 색 변경
                <h3 style='color:'red'>
            3) 자바스크립트를 이용하여 색 변경
                area2.style.color = "red";
            */
        }

    </script>

```
<br/><br/>

## HTML태그 아이디로 접근
 

```html
<div id="area1" class="area"></div>
    <button type="button" onclick="accessId();">아이디로 접근</button>

    <script>
        function accessId(){
            // 아이디가 area1인 요소의 배경색을 노란색, 텍스트 출력
            var area1 = document.getElementById("area1");

            // 배경 노란색으로 변경
            area1.style.backgroundColor = "yellow";

            // 텍스트 출력
            area1.innerText = "아이디로 접근 성공";
        }
    </script>

    <div id="area2" class="area"></div>
    <button type="button" onclick="accessId2()">클릭마다 배경색변경</button>
    <script>
        function accessId2(){
            //아이디가 "area2"인 요소를 얻어와 변수 aera2에 저장
            var area2 = document.getElementById("area2");

            //area2의 배경색을 얻어와 bgColor 변수에 저장
            var bgColor = area2.style.backgroundColor; //getter
            if(bgColor == "red"){
                area2.style.backgroundColor = "yellow";
            }else{
                area2.style.backgroundColor = "red";
            }
        }
    </script>

```
<br/><br/>




## HTML태그 클래스명으로 접근


```html
<div class="area test"></div>
    <div class="area test"></div>
    <button type="button" onclick="accessClass()">클래스로 접근</button>

    <script>
        function accessClass(){
            // 클래스 속성값으로 "test"를 포함하고 있는 모든 요소 얻어와
            // testArr 변수에 저장

            var testArr = document.getElementsByClassName("test");
            // ->getElementsXXX로 선택된 요소들은 배열(Array)의 형태로 반환된다.
      
            testArr[0].style.backgroundColor = "lightblue";
            testArr[1].style.backgroundColor = "lightpink";
      
        }
    </script>
```
<br/><br/>


## HTML태그 태그명으로 접근


```html
<ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
    </ul>
    <button type="button" onclick="accessTagName()">
        태그명으로 접근    
    </button>
    <script>
        function accessTagName(){
            // 문서 내에 있는 모든 li 태그를 얻어와서 list 변수에 저장
            var list = document.getElementsByTagName("li");

            for(var i=0; i<list.length; i++){
                list[i].style.backgroundColor
                    ="rgb(130, 220, " + (50 + 50 * i) + ")";
            }
        }
    </script>

```
<br/><br/>


## HTML태그 name으로 접근


```html
<fieldset>
        <legend>취미</legend>

        <table>
            <tr>
                <td>
                    <input type="checkbox" name="hobby" id="game" value="game">
                    <label for="game">게임</label>  <!-- id가 game인 것과 연결이 됨-->
                </td>
                <td>
                    <input type="checkbox" name="hobby" id="music" value="music">
                    <label for="music">음악</label>  <!-- id가 music인 것과 연결이 됨-->
                </td>
                <td>
                    <input type="checkbox" name="hobby" id="movie" value="movie">
                    <label for="movie">영화감상</label>  <!-- id가 movie인 것과 연결이 됨-->
                </td>
            </tr>
            <tr>
                <td>
                    <input type="checkbox" name="hobby" id="eat" value="eat">
                    <label for="eat">먹기</label>  <!-- id가 eat인 것과 연결이 됨-->
                </td>
                <td>
                    <input type="checkbox" name="hobby" id="book" value="book">
                    <label for="book">독서</label>  <!-- id가 book인 것과 연결이 됨-->
                </td>
                <td>
                    <input type="checkbox" name="hobby" id="sleep" value="sleep">
                    <label for="sleep">잠자기</label>  <!-- id가 sleep인 것과 연결이 됨-->
                </td>
            </tr>
        </table>
    </fieldset>
    
    <div id="area3" class="area"></div>

    <button type="button" onclick="accessName();">name으로 접근</button>

    <script>
        function accessName(){
            // 문서 내에서 name 속성값이 "hobby"인 요소를 모두 얻어와
            // 배열 형태로 hobbyList변수에 저장.
            var hobbyList = document.getElementsByName("hobby");

            // 아이디가 "area3"인 요소를 얻어와 area3변수에 저장
            var area3 = document.getElementById("area3");

            // area3 요소의 모든 내용을 삭제
            area3.innerHTML="";

            // 체크 개수를 저장할 변수 선언
            var count =0;

            // 체크된 요소의 value값을 출력하고, 개수를 카운트
            for(var i=0; i<hobbyList.length; i++ ){

                if(hobbyList[i].checked){
                    //.checked : 해당 요소가 체크가 되어있으면 true, 아니면 false
                    area3.innerHTML += hobbyList[i].value + " ";     

                    count++;
                }

            }

            area3.innerHTML +="<br><br>선택한 취미 개수 :" + count;


        }
    </script>

```
<br/><br/>

## text box(입력상자)에 작성된 값 읽어오기


```html
입력 : <input type="text" id="input" name="input">
    <button type="button" onclick="readValue();">input값 확인</button>

    <div id="area4" class="area"></div>

    <script>
        function readValue(){

            // input,div 요소 얻어오기
            var input = document.getElementById("input");
            var area4 = document.getElementById("area4");

            // 길이는 띄어쓰기 포함해서 계산이 된다.
            console.log(input.value.length);

            // input 태그에 작성된 값(value)의 앞 뒤 공백을 모두 제거했을 때
            // 글자 길이가 0인 경우 == 아무것도 작성하지 않은 경우
            if(input.value.trim().length == 0){
                // 문자열.trim() : 문자열 양쪽에 공백 제거
                alert("값을 입력해주세요");

                // input태그에 있는 입력된 값 초기화
                input.value="";

                // 커서를 input에 이동하여 초점을 맞춤
                input.focus();
            }else{
                // input 태그에 작성된 값(value)를 얻어와 area4에 출력
                area4.innerHTML ="<h3>" + input.value + "</h3>"
            }
        }
    </script>

```
<br/><br/>

## 변수 선언

```html
 <pre>
        <script>
     1)   변수명; //전역 변수(global)  (전역 변수는 var 없이도 변수 선언 가능)
     2)   var 변수명; // 전역 변수(global)
            function 함수명() {
             3)  var 변수명; //지역 변수(local)
            }
        </script>
    </pre>
```
```html
<script>
        // 전역 변수 선언
        str = "전역 변수"; //window라는 창 자체를 나타내는 객체의 필드로 str이 등록됨.

        
        var str2="var 전역 변수"; // script 태그 바로 아래 작성된 var가 붙은 변수는 전역 변수가 된다.

        function varTest(){
            //전역변수와 동일한 변수명을 가진 지역변수 선언
            var str = "지역 변수";

            console.log("str 호출");
            console.log(str); //지역 변수
            // 동일한 이름의 변수가 있을 경우 가까운 변수(지역변수)가 우선권을 가짐.

            console.log(window.str); //전역 변수
            // 전역변수로 선언하면 window 객체에 등록됨.

            console.log(this.str); //전역 변수
            // this = 현재 창(문서)에 있는 str

            console.log("----------------------------------------------------");

            console.log("str2 호출");
            console.log(str2); //"var 전역 변수"
            console.log(window.str2); //"var 전역 변수"
            console.log(this.str2);  //"var 전역 변수"

            console.log("----------------------------------------------------");

            // 새로운 지역 변수 추가

            var str3="새로운 지역 변수";
            console.log("str3 호출");
            console.log(str3);  // 새로운 지역 변수
            console.log(window.str3); //undefined : 정의되지 않음
            console.log(this.str3);   //undefined : 정의되지 않음

            console.log("----------------------------------------------------");

            // function 내부에 var 키워드 없이 선언된 변수는 전역 변수가 된다.
            str4 = "새로운 전역 변수";
            console.log("str4 호출");
            console.log(str4);  // 새로운 전역 변수
            console.log(window.str4); // 새로운 전역 변수
            console.log(this.str4);   // 새로운 전역 변수
        }
        varTest(); // 함수 호출
    </script>


```
<br/><br/>

## 자바스크립트의 자료형
 자바스크립트의 변수는 별도로 자료형을 지정하지 않음.<br/>
->변수에 저장되는 값(리터럴)에 의해서 자료형이 결정됨.

```html
<ul>
        <!-- char 자료형이 없음. "", '' 둘 다 문자열을 나타낸다. -->
        <li>string(문자열) -- "문자열", '문자열', "A", 'A'</li>

        <li>number(숫자) -- 1, 0.123, 99999999999</li>

        <li>boolean(논리값) -- true, false</li>

        <li>object(객체) -- 배열(Array), { name : "홍길동" }</li>
        <!-- { name : "홍길동" } == 자바스크립트 객체 표기법 == json -->

        <li>function(함수) -- var t1 = function test(){} </li>
        <!-- 함수를 변수에 저장 가능함. t1을 호출하면 test 함수 실행된다. -->

        <li>undefined(정의되지 않은 변수) -- var a</li>
        <!-- 변수에 값을 초기화 하지 않음 -->
    </ul>

    <button type="button" onclick="typeTest();">자료형 테스트</button>
    
    <div id="area1"></div>
    
    <script>

        function typeTest(){
            var area1 = document.getElementById("area1");

            var name = "홍길동"; // string
            var age = 20; // number
            var check = true; //boolean
            var hobbyList = ['게임','노래','잠']; //object
            var user = { id : "user01" , pwd : "pass01" }; //object
            var fn = function plusNumber(num1, num2){ //function
                        var sum = num1+num2;
                        return sum;
                     }
            var undef;  //undefined

            // typeof : 값의 자료형을 확인하는 연산자
            
            area1.innerHTML = ""; // area1 이전에 있던 내용 삭제


            area1.innerHTML += name + "의 자료형 : " + typeof name + "<br>";
            area1.innerHTML += age + "의 자료형 : " + typeof age + "<br>";
            area1.innerHTML += check + "의 자료형 : " + typeof check + "<br>";
            area1.innerHTML += hobbyList + "의 자료형 : " + typeof hobbyList + "<br>";
            area1.innerHTML += user + "의 자료형 : " + typeof user + "<br>";
            area1.innerHTML += fn + "의 자료형 : " + typeof fn + "<br>";
            area1.innerHTML += undef + "의 자료형 : " + typeof undef + "<br>";

            console.log(user.id);
            console.log(user.pwd);

            console.log(fn(100,200));
        }

    </script>


```
<br/><br/>


## 문자열(string)
"" 또는 ''으로 묶여 있는 리터럴 <br/>
자바스크립터에서 문자열을 위한 함수가 내장되어 있는  내장객체 String을 제공함.

```html
<button type="button" onclick="showStringFn();">문자열 관련 함수</button>

    <div id="area1" class="area"></div>

    <script>

        function showStringFn(){
            var str1 = "Hello World";

            var area1 = document.getElementById("area1");

            area1.innerHTML =""; // area1의 기존 내용 삭제

            area1.innerHTML += "toUpperCase() : " + str1.toUpperCase()+ "<br>"; //대문자
            area1.innerHTML += "toLowerCase() : " + str1.toLowerCase()+ "<br>"; //소문자

            area1.innerHTML += "length : " + str1.length + "<br>"; //문자열 길이

            //  문자열 내 특정 문자 인덱스 검색
            area1.innerHTML += "앞에서 부터 첫 번째 l의 위치 "+ str1.indexOf("l")+("<br>");
            area1.innerHTML += "뒤에서 부터 첫 번째 l의 위치 "+ str1.lastIndexOf("l")+("<br>");
        

            // 문자 추출
            for(var i=0; i<str1.length; i++){
                area1.innerHTML += i+"번째 인덱스 : " + str1.charAt(i)+("<br>");
            }

            // 문자열 자르기
            area1.innerHTML += "문자열 일부만 잘라서 반환 : " + str1.substring(6,str1.length)+("<br>");
                                                                            //6 이상 11 미만
             
            var str2 = "사과, 바나나, 복숭아, 키위, 귤";
            
            var fruits = str2.split(", ");

            area1.innerHTML += "str2.split() : " + fruits;
            console.log(fruits);
        
        }
    </script>
```
<br/><br/>


## 숫자(number)
 정수, 부동소수점(실수), Infinity, NaN 리터럴이 존재.<br/>
수학과 관련된 함수를 제공하는 Math 내장 객체가 존재함.

```html
<button type="button" onclick="showMathFn();">수학 관련 함수</button>
    <div id="area2" class="area"></div>
    <script>
        function showMathFn(){
            var area2 = document.getElementById("area2");

            area2.innerHTML = ""; //area2 기존에 있던 내용 삭제

            var num1 = -123;
            
            area2.innerHTML += "num1 : " + num1 + "<br>";
            area2.innerHTML += "절대값 : " + Math.abs(num1) + "<br>";




            var num2 = 123.456;
            // 반올림, 올림, 내림은 소수점 첫번재 자리에서 반올림 하도록 되어있음. 
            // 일의 자리까지만 표시 됨.
            // round, ceil, floor = 일의 자리까지 표시하는 역할, 소수점을 지정해 줄 수 없음.

            area2.innerHTML += "소수점 셋째 자리에서 반올림 : " + Math.round(num2*100)/100 + "<br>";
            
            area2.innerHTML += "소수점 셋째 자리에서 올림 : " + Math.ceil(num2*100)/100 + "<br>";
            
            area2.innerHTML += "소수점 셋째 자리에서 내림 : " + Math.floor(num2*100)/100 + "<br>";
        
        
        
            // 난수
            area2.innerHTML += "임의의 난수 발생 : " + Math.random() + "<br>";
                                                    // 0 <= random < 1

            area2.innerHTML += "(1 / 0) = " + (1 / 0)+ "<br>"; // Infinity
            area2.innerHTML += Math.abs("abc")+ "<br>"; // NaN(Not a Number)
        
        
        
        }
    </script>

```
<br/><br/>