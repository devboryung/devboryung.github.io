---
title: "2020년 12월 08일"
excerpt: "Javascript-3"
search: true
categories: 
  - Academy
tags: 
  - Javascript
  - HTML
toc: true
---

## 함수(function)
```html
    <h1>함수(function)</h1>
    <h3>기본적인 함수 정의, 호출 방법</h3>
    <pre>
        - 함수 정의
        function 함수명([매개변수]) {
            수행할 코드;
        }

        - 함수 호출
        함수명([매개변수]);

        - 함수 호출 방법
        > 이벤트를 이용한 호출 ex) onclick = "testFn()";
        > script 태그 내 코드에서 호출 ex) testFn();
    </pre>
    <button type="button" onclick="test1();">클릭 시 함수 호출</button>
    <!-- <button type="button" id="btn1">클릭 시 함수 호출</button> -->
    <p id="p1"></p>

    <script>
        // 버튼 클릭 횟수를 p1 아이디를 가진 p 태그에 출력

        var clickCount = 0; // 전역변수
        function test1() {
            document.getElementById("p1").innerText = ++clickCount;
        }

        // id 값이 btn1인 요소가 클릭되었을 때 test1() 함수 수행
        // document.getElementById("btn1").onclick = test1();
        // 이벤트 발생 시 이미 정의된 함수가 있다면 함수를 호출만 하면 된다.
    </script>
```
<br>

### 익명 함수(이름이 없는 함수)
```html
    <h3>익명 함수(이름이 없는 함수)</h3>
    <pre>
        - function([매개변수]){
            수행할 코드;
        }

        - 이벤트별 동작(기능) 부여, 즉시 실행 함수에서 주로 사용됨
        - 변수에 함수를 저장할 때, 매개변수로 함수를 전달할 때도 사용 가능함.

    </pre>

    <button type="button" id="btn2">익명 함수 확인</button>

    <script>
        // 문서 내에서 id가 btn2인 요소가 클릭되었을 때
        // 경고창을 통해 "btn2 클릭됨"을 출력
        document.getElementById("btn2").onclick = function() {
                                                    // 익명함수
            alert("btn2 클릭됨");
        }
    </script>
```

<br>

### 즉시 실행 함수
```html
    <h3>즉시 실행 함수</h3>
    <pre>
        -  (function([매개변수]){ 수행할 코드; })();

        - 익명 함수의 한 종류.
        - 함수가 정의 되자마자 실행되는 함수.

        * 사용하는 이유
        - 함수 정의, 호출, 변수 저장 같은 함수와 관련된 일련의 과정을 거치지 않고 즉시 실행됨.
            -> 일반적인 함수 사용법보다 속도적인 측면에서 우세함.

        - 사용하려는 변수명이 이미 전역변수로 사용되고 있는 경우
          즉시 실행 함수를 이용하여 지역변수 형태로 변환하는 방법으로
          변수명 충돌 문제를 해결
    </pre>

    <button type="button" id="btn3">즉시 실행 함수 확인</button>
    <p id="p3"></p>

    <script>
        var p3 = document.getElementById("p3");

        var str = "전역 변수<br>";

        (function(){
            var str = "즉시 실행 함수<br>";
            p3.innerHTML += str;

        })();

        p3.innerHTML += str;
    </script>
```

<br>

### 함수의 매개변수(전달인자/parameter)
```html
    <pre>
        자바스크립트의 함수는
        지정된 매개변수보다 더 많거나 적은 수의 매개변수를 전달할 수 있음.
    </pre>

    <button type="button" id="btn4">확인하기</button>

    <p id="p4"></p>

    <script>
        function test4(value){
            document.getElementById("p4").innerHTML += value + "<br>";
        }

        // 아이디가 btn4인 요소가 클릭되었을 때 이벤트 동작
        document.getElementById("btn4").onclick = function() {
            // p4 요소에 작성된 내용 삭제
            document.getElementById("p4").innerHTML = "";

            // 함수 정상 호출
            test4("안녕하세요.");

            // 지정된 매개변수의 수보다 많이 전달
            test4("안녕하세요.", "반갑습니다.");
            // 초과된 매개변수를 무시

            // 지정된 매개변수의 수보다 적게 전달
            test4(); 
            // 전달되지 않은 매개변수는 undefined로 처리됨.

        }
    </script>
```

<br>

### 함수의 리턴 처리
```html
    <h3>함수의 리턴 처리</h3>
    <button type="button" id="btn5">확인하기</button>
    <p id="p5"></p>

    <script>
        function test5() {
            // 1 ~ 100 사이의 난수를 발생시켜서 반환
            return Math.floor(Math.random() * 100 + 1);
        }

        document.getElementById("btn5").onclick = function() {
            // 버튼 클릭 시 마다 p5 영역에 난수 출력
            document.getElementById("p5").innerHTML += test5() + "<br>";
        }
    </script>
```

<br>

### 가변인자 함수 테스트
```html
    <pre>
        매개변수의 개수가 변하는 함수를 말하며
        모든 함수 내부에 arguments라는 객체가 자동 생성되어
        배열 형태로 매개변수를 저장하게됨

        매개변수가 지정되지 않은 값이 넘어오면
        arguments라는 객체에 배열처럼 인덱스를 부여하고 순서대로 저장.
    </pre>

    <button type="button" id="btn6">확인하기</button>
    <p id="p6"></p>

    <script>
        document.getElementById("btn6").onclick = function() {

            document.getElementById("p6").innerHTML =  sumAll(1,2,3,4,5,6,7,8,9,10);
        }

        // 매개변수로 전달받은 값을 모두 더해서 반환하는 함수
        function sumAll() {
            console.log(typeof arguments); // arguments 자료형
            console.log(arguments.length); // arguments 길이
            console.log(arguments); // arguments에 저장된 배열 요소

            // arguments에 저장된 요소 합 구하기
            var sum = 0;
            for(var i of arguments){
                sum += i;
            }  
            return sum;

        }
    </script>
```

<br>

### 내부 함수(중첩함수)
```html
    <h3>내부 함수(중첩 함수)</h3>
    <pre>
        함수 내에 함수가 작성된 형태

        특정 함수 내에서 반복적으로 사용할 기능을 정의해 두는 형태
    </pre> 
    
    <h4>피타고라스 정리를 이용한 빗변 길이 계산 : a^2 + b^2 = c^2</h4>
    
    삼각형 밑변 길이 : 
    <input type="number" min="1" max="1000" id="width"> <br>

    삼각형 높이 길이 : 
    <input type="number" min="1" max="1000" id="height"> <br>

    삼각형의 빗변 길이 : 
    <input type="text" id="hypotenuse" readonly>
    <!-- readonly : 읽기 전용 -->

    <button type="button" id="btn7">계산하기</button>

    <!-- <script>
        function calc(w, h){
            // 밑변, 높이를 전달받아 빗변 길이를 계산하여 반환
            
            function square(x){
                // 전달받은 수를 제곱해서 반환하는 내부 함수
                return sq * x;
            }

            return Math.sqrt(square(w) + square(h));
        }

        document.getElementById("btn7").onclick = function(){
            // id가 width, heigth인 input 태그에 작성된 값을 얻어와
            // calc() 함수의 매개변수로 전달하고
            // 반환받은 값을 hypotenuse에 출력하기

            var width = document.getElementById("width").value;
            var height = document.getElementById("height").value;

            document.getElementById("hypotenues").value;
            calc(width, heigth);
        }
    </script> -->


    <script>
        function calc(w, h){
            function square(x){
                return x*x;
            }

            return Math.sqrt(square(w) + square(h));
        }

        document.getElementById("btn7").onclick = function(){
            var w = document.getElementById("width").value;
            var h = document.getElementById("height").value;
            document.getElementById("hypotenuse").value = calc(w,h);
        }
    </script>
```

<br>

### 매개변수로 함수 자료형 전달하기
```html
    <h3>매개변수로 함수 자료형을 전달</h3>
    <p>자바스크립트에서는 함수도 하나의 자료형이고 변수에 저장 가능함<br>
        -> 그러므로 매개변수로도 전달 가능함.
    </p>

    <h4>선언적 함수를 매개변수로 전달</h4> 
    <p>alert 창을 10번 출력하는 함수</p>

    <button type="button" onclick="test1(otherFn);">함수 실행</button>
    <!-- 함수명만 작성하는 경우 : 함수를 변수처럼 사용 -->
    <script>
        function otherFn(i){
            // 매개변수로 숫자를 하나 전달받아와
            // 숫자에  따라  메시지를 다르게 출력
            var str = "";

            if(i > 5){
                str = i + "번 더 클릭하세요.";
            } else if(i > 1){
                str = i + "번 남았습니다!";
            }else{
                str = "마지막 한 번!";
            }

            alert(str);
        }

        function test1(otherFn){
            // 함수를 매개변수로 받아
            // 10 ~ 1까지 1씩 감소하는 for문을 이용해서
            // 매개변수로 전달받은 함수를 반복 호출

            for(var i=10; i > 0; i--){
                otherFn(i);
            }   
        }
    </script>
```

### 익명 함수를 매개변수로 사용하기
```html
    <h4>익명 함수를 매개변수로 사용</h4>
     <!-- 익명함수를 매개변수, 반홥값으로 사용할 경우
           비동기적 효과를 나타낸다. 
        (동기식 : 줄세워서 순서대로(안전),
         비동기식 : 동시에 한번에 수행(충돌위험 ))
        -->

        <div id="area1" class="area"></div>
        <script>
            // 함수를 매개변수로 받는 함수 정의
            function test2(otherFn2){
                for(var i = 0; i < 10; i++){
                    otherFn2(i);
                }
            }

            // test2 함수 호출
            test2( function(i){
                document.getElementById("area1").innerHTML
                    += i + "번째 익명함수를 호출함.<br>";
            });
        </script>
```

<br>

### 익명 함수를 리턴하는 함수
```html
        <h3>익명 함수를 리턴하는 함수</h3>
        <pre>
            익명 함수가 리턴되는 함수를 호출하는 방법
              ->  함수명()();
            
        </pre>

        <input type="text" id="text1" readonly>
        <button type="button" onclick="test3()();">확인하기</button>
        <!--
            첫 번째 () : test3 함수 호출
            두 번째 () : 반환된 익명 함수 호출(실행)
        -->

        <script>
            function test3() {
                return function(){
                    document.getElementById("text1").value = "리턴받은 익명 함수가 호출됨.";
                }
            }

        </script>
```

<br/>

## 객체
```html
    <h1>자바스크립트 객체</h1>
    <pre>
    - 자바스크립트의 객체는 Key:Value의 형태로 이루어져있다.

    - 객체는 {}(중괄호, 객체 리터럴 표기법) 또는
      new 연산자를 이용하여 생성할 수 있고,
      value에는 모든 자료형이 대입될 수 있다.
    
    ex)
    var student1 = { name : "홍길동", age : 15,
                      address : "서울시 중구 남대문로" };
    자바스크립트 객체 표기법(JavaScript Object Natation : JSON)
    </pre>

    <button type="button" id="btn1">확인하기</button>
    <div id="area1" class="area"></div>

    <script>
        document.getElementById("btn1").onclick = function() {
            var product = {
                pName : '아이폰12',
                brand : "Apple",
                option : ["128G", "512G"]
            };

            var area1 = document.getElementById("area1");

            area1.innerHTML = ""; // 이전 내용 삭제

            area1.innerHTML += "<br>객체명.속성명으로 접근하기</b><br>";
            area1.innerHTML += "product.pName = " + product.pName + "<br>";
            area1.innerHTML += "product.brand = " + product.brand + "<br>";
            area1.innerHTML += "product.option = " + product.option + "<br>";
            area1.innerHTML += "product.option[1] = " + product.option[1] + "<br>";
            
            area1.innerHTML += "<b>객체명['속성명']으로 접근하기</b><br>";
            area1.innerHTML += "product['pName'] = " + product['pName'] + "<br>";
            area1.innerHTML += "product['brand'] = " + product['brand'] + "<br>";
            area1.innerHTML += "product['option'] = " + product['option'] + "<br>";
        }
    </script>
```

<br>

### 객체의 메소드 속성
```html
   <h3>객체의 메소드 속성</h3>
    <pre>
    객체 속성 중 자료형이 function인 속성을 메소드라고 부른다.
    </pre>

    <button type="button" id="btn2">확인하기</button>
    <div id="area2" class="area"></div>
    <script>
        document.getElementById("btn2").onclick = function(){
            var area2 = document.getElementById("area2");

            var name = "가지";

            var dog = {
                name : "로이",
                walk : function(course){

                    area2.innerHTML = name + "와 " + course + "으로 산책을 갔다.<br>";

                    // 객체 내에서 자신의 속성을 호출할 때는
                    // 반드시! this 라는 참조변수를 사용해야 한다.

                    // (타 프로그래밍 언어 개발자들이 가장 많이 하는 실수 중 하나)
                    area2.innerHTML += this.name + "와 " + course + "으로 산책을 갔다.<br>";
                }
            }

            // dog 객체의 walk 기능 호출
            dog.walk("숭인근린공원");
        }
    </script>
```

<br>

### in / with
```html
    <h3>in / with 키워드</h3>
    <pre>
    in : 객체 내부에 특정 속성이 있는지 확인하는 키워드

    with : 코드를 줄여주는 키워드, 객체 호출 시 객체명을 생략할 수 있게함.
    </pre>

    이름 : <input type="text" id="name"><br>
    국어 : <input type="text" id="kor"><br>
    영어 : <input type="text" id="eng"><br>
    수학 : <input type="text" id="mat"><br>

    <button type="button" id="btn3">실행 확인</button>
    <div id="area3" class="area big"></div>

    <script>
        document.getElementById("btn3").onclick = function() {
            var area3 = document.getElementById("area3");
            
            var student = {
                name : document.getElementById("name").value,
                kor : Number(document.getElementById("kor").value),
                eng : Number(document.getElementById("eng").value),
                mat : Number(document.getElementById("mat").value)
            }

            area3.innerHTML = "<b>in 키워드 테스트</b><br>";
            area3.innerHTML += "name : " + ("name" in student) + "<br>";
            area3.innerHTML += "kor : " + ("kor" in student) + "<br>";
            area3.innerHTML += "eng : " + ("eng" in student) + "<br>";
            area3.innerHTML += "mat : " + ("mat" in student) + "<br>";
            area3.innerHTML += "sum : " + ("sum" in student) + "<br>";

            with(student){
                area3.innerHTML += name + "<br>";
                area3.innerHTML += kor + "<br>";
                area3.innerHTML += eng + "<br>";
                area3.innerHTML += mat + "<br>";
                area3.innerHTML += "합계 : " + (kor + eng + mat) + "<br>";
            }
        }
    </script>
```

<br>

### 객체 속성 추가 / 제거
```html
    <h3>객체의 속성 추가 / 제거</h3>
    <pre>
    처음 객체를 생성한 이후 
    속성을 추가 또는 제거하는 행위를 
    "동적으로 객체 속성 추가/제거"라고 한다.
    </pre>

    <button type="button" id="btn4">실행 확인</button>
    <script>
        document.getElementById("btn4").onclick = function() {

            // 비어있는 객체 선언
            var student = {};

            console.log(student);

            // 빈 객체에 속성 추가
            student.name = "홍길동";
            student.hobby = "축구";
            student.kor = "100";
            student.mat = "80";

            // 객체명.속성명 = 값
            // 객체에 작성한 속성이 없을 경우 알아서 속성이 추가된다.

            console.log(student);   

            // 객체 속성 제거
            delete(student.mat);

            console.log(student); 
        }
    </script>
```