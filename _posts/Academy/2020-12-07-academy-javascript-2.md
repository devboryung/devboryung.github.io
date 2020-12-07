---
title: "2020년 12월 07일"
excerpt: "Javascript-2"
search: true
categories: 
  - Academy
tags: 
  - Javascript
  - HTML
toc: true
---
## 데이터 형변환
자바스크립트에서 문자열과 숫자 형변환하는 방법

숫자 -> 문자열로 바꾸는 법 : 숫자 + 문자열 = 문자열<br/>
 ex) 123 + "A" = "123A" / 123 +"" = "123"<br/>
** 자바스크립트 문자열 리터럴 표기법 : "", ''
{: .notice--success}
<br/>



<br/><br/>

## "", '' 혼용 사용

"", '' 두 기호를 혼용해서 사용하는 경우 : <br/>
인라인 방식의 자바스크립트 함수 호출 시 매개변수로 문자열이 포함되는 경우;<br/>
함수를 인지시키는 "", 매개변수 문자열을 인지시키는 '' (""/'' 위치 변경 가능)<br/>



```html
<body>
<button type="button" onclick="testFn('Hello World');">test</button>
    <script>
        function testFn(str){
            alert(str);
        }
    </script>
```

<br/><br/>


## 문자열과 숫자의 + 연산

문자열 + 숫자열 = 문자열(이어쓰기)<br/>
숫자 + 문자열 = 문자열(이어쓰기)<br/>
숫자 + 숫자 + 문자열 = 문자열(숫자의 합 + 문자 이어쓰기[사칙연산 우선 순위])  

```html
<button type="button" onclick="testPlus();">문자열,숫자의 +연산</button>
    <div id="area1" class="area"></div>
    <script>
        function testPlus(){
            var area1 = document.getElementById("area1");
            area1.innerHTML =""; //area1 기존 내용 삭제

            var test1 = 7 + 7; // 14
            var test2 = 7 + "7"; // 77
            var test3 = 7 + '7'; // 77
            var test4 = 7 + 7 + "7"; // 147
            var test5 = 7 + "7" + 7; // 777 
            var test6 = "7" + 7 + 7; // 777
            var test7 = "7" + (7 + 7); // 714

            area1.innerHTML += "7 + 7 = " + test1 + "<br>";
            area1.innerHTML += '7 + "7" = ' + test2 + "<br>";
            area1.innerHTML += "7 + '7' = " + test3 + "<br>";
            area1.innerHTML += '7 + 7 + "7" = ' + test4 + "<br>";
            area1.innerHTML += '7 + "7" + 7 = ' + test5 + "<br>";
            area1.innerHTML += '"7" + 7 + 7 = ' + test6 + "<br>";
            area1.innerHTML += '"7" + ( 7 + 7 ) = ' + test7 + "<br>";
        }
    </script>
```


<br/><br/>

## 문자열 -> 숫자
input태그등을 이용한 입력된 숫자는 숫자가 아닌 문자열로 인식됨.<br/><br/>
자바스크립트에서 문자열 -> 숫자로 바꿀 수 있는<br/>
내장객체 Number()와  내장함수 parseInt(), parseFloat()가 있음. <br/>
*자바스크립트에서 모든 실수는 float 자료형이다. <br/>

```html
<button type="button" onclick="castingTest();">문자열->숫자 형변환</button>
    <div id="area2" class="area"></div>
    <script>
        function castingTest(){
            var iNum = 2;
            var sNum = "3";
            var fNum = "1.234";

            var test1 = iNum + sNum; //23
            
            // 내장객체 Number("문자열 숫자") -->숫자로 형변환
            var test2 = iNum + Number(sNum); //5

            // 내장함수 parseInt(), parseFloat();
            // 매개변수로 "문자열로된 숫자"를 작성.
            var test3 = iNum + parseInt(sNum); //5
            var test4 = iNum + parseFloat(sNum); //5 (5.0X)

            var test5 = iNum + fNum; //21.234;
            var test6 = iNum + Number(fNum); //3.234
            var test7 = iNum + parseInt(fNum); //3 (정수로만 반환 됨)
            // parseInt() 수행 시 -> 소수 부분은 버림처리되고 정수 부분만 숫자로 변함.
            var test8 = iNum + parseFloat(fNum); //3.234


            var area2 = document.getElementById("area2");

            area2.innerHTML ="";
            area2.innerHTML += ('2 + "3" = ' + test1 + "<br>");
            area2.innerHTML += ('2 + Number("3") = '  + test2 + "<br>");
            area2.innerHTML += ('2 + parseInt("3") = '  + test3 + "<br>");
            area2.innerHTML += ('2 + parseFloat("3") = '  + test4 + "<br>");
            area2.innerHTML += ('2 + "1.234" = ' + test5 + "<br>");
            area2.innerHTML += ('2 + Number("1.234") = '  + test6 + "<br>");
            area2.innerHTML += ('2 + parseInt("1.234") = '  + test7 + "<br>");
            area2.innerHTML += ('2 + parseFloat("1.234") = '  + test8 + "<br>");
           
            
        }
    </script>
```

<br/><br/>

## 연산자
= : 대입 연산자<br/><br/>
== , != : 동등 비교 연산자  <br/>
        -> 값이 일치하는지를 비교<br/>
     ex) 2 == 2 > true  /  2 == "2" > true<br/><br/>
=== , !== : 일치 연산자 , 동일 비교 연산자  <br/>
          ->자료형과 값이 모두 일치하는지를 비교<br/>
         ex) 2 == 2 > true / 2 == "2" > false<br/>
{: .notice--success}
<br/>

```html
<button type="button" onclick="opTest();">확인하기</button>

    <h3>동등 비교 연산자 (==,!=)</h3>
    <div id="area1" class="area"></div>
    <h3>동일 비교 연산자 (===,!==)</h3>
    <div id="area2" class="area"></div>

    <script>
        function opTest(){
            var num = 1;
            var sNum = "1";

            var boolT = true;
            var sBoolT = "true";

            var num0 = 0;
            var sNum0 = "0";
            var str = "";

            var nullVal = null;
            var unDef = undefined; 
            //null : 참조하는 값이 없다. (자료형은 부여됨)
            //undefined : 자료형과 참조하는 값이 없다.

            var arr = document.getElementsByClassName("area");

            arr[0].innerHTML="";
            arr[1].innerHTML="";

            // 동등 
            arr[0].innerHTML += '1 == 1 : ' + (num == 1)+"<br>";
            arr[0].innerHTML += '1 == "1" : ' + (num == sNum)+"<br>";
            arr[0].innerHTML += '1 == true : ' + (num == boolT)+ "<br>";
            arr[0].innerHTML += '1 == "true" : ' + (num == sBoolT)+ "<br>";
            arr[0].innerHTML += '0 == "0" : ' + (num0 == sNum0)+ "<br>";
            arr[0].innerHTML += '0 == "" : ' + (num0 == str)+ "<br>";
            arr[0].innerHTML += '"0" == "" : ' + (sNum0 == str)+ "<br>";
            arr[0].innerHTML += 'null == undefined : ' + (nullVal == unDef) + "<br>";

            // 동일
            arr[1].innerHTML += '1 === 1 : ' + (num === 1)+"<br>";
            arr[1].innerHTML += '1 === "1" : ' + (num === sNum)+"<br>";
            arr[1].innerHTML += '1 === true : ' + (num === boolT)+ "<br>";
            arr[1].innerHTML += '1 === "true" : ' + (num === sBoolT)+ "<br>";
            arr[1].innerHTML += '0 === "0" : ' + (num0 === sNum0)+ "<br>";
            arr[1].innerHTML += '0 === "" : ' + (num0 === str)+ "<br>";
            arr[1].innerHTML += '"0" === "" : ' + (sNum0 === str)+ "<br>";
            arr[1].innerHTML += 'null === undefined : ' + (nullVal === unDef) + "<br>";

        }
    </script>
```



<br/><br/>

## 연산자를 이용한 짧은 조건문
&& : 참이면 수행<br/>
|| : 거짓이면 수행<br/>
<br/>

```html
<button type="button" onclick="checkCondition();">실행확인</button>
    <script>
        function checkCondition(){
            //프롬포트로 숫자를 입력받아 홀, 짝 구분
            var input = window.prompt("숫자를 입력하세요");
            //prompt에 입력한 숫자의 자료형은 문자열이다.
            // -> 숫자 변형을 위해 Number() 내장객체 사용

            
            input % 2 == 0 && alert("짝수 입니다."); 
            input % 2 == 0 || alert("홀수 입니다.");
        }
    </script>
```


<br/><br/>

## 배열이란?
자바스크립트에서는 리터럴값이 지정되기 전 까지 변수의 자료형이 지정되지 않음.<br/><br/>
배열 선언 시 자료형이 지정되지 않아 모든 자료형을 보관할 수 있음. <br/>
== 자바의 컬렉션과 비슷함.<br/>
-- 모든 자료형 저장 가능<br/>
-- 배열 길이 제한이 없음<br/>
{: .notice--success}
<br/>

```html
 <button type="button" onclick="arrayTest1();">확인하기</button>
    <div id="area1" class="area"></div>

    <script>
        function arrayTest1(){
            // 배열 선언 및 초기화
            // 자바 - int[] arr = {1, 2, 3, 4};
            var arr1 = ['홍길동', 20, true, [1,2,3,4]]; //2차원배열

            var area1 = document.getElementById("area1");
            // 배열 내에 있는 요소 모두 출력
        
            area1.innerHTML = arr1 + "<br><br>";
            console.log(arr1);

            // for문을 이용하여 배열 내 요소 모두 출력
            for(var i=0; i<arr1.length; i++){
                 area1.innerHTML += arr1[i] + "<br>";
             } 

            // for in 구문 : 객체(Object)의 모든 요소를 순차적으로 접근
            // 객체 : Object, Array, Set, Map, String ...
            // i 값이 0부터 배열길이-1 까지 차례대로 1씩 증가
            for(var i in arr1){ 
                 area1.innerHTML += arr1[i] + "<br>";
             }
            // for in구문 사용시 
            // 자바스크립트 객체(Object)가 아닌 경우
            // 보이지 않는 내부 값들도 접근하게 되는 경우가 생김.
            // Object에만 사용하는것을 권장함.

            // for of 구문 : Array의 요소에 순차적으로 접근
            // == 자바의 향상된 for문과 비슷
            for(var item of arr1){
                 area1.innerHTML += item + "<br>";
            }
        }
    </script>
```
<br/><br/>


## 배열 선언 및 할당
배열 선언 및 할당 시 크기를 지정하거나, 지정하지 않을 수 있다.

```html
<script>
        function arrayTest2(){
            // new Array();

            // 비어있는 배열
            //var arr1 = new Array(); // Object(Array)
            var arr1 =[];
            //console.log(arr1);

            // 일정 크기를 가지고 있는 배열 선언 및 할당
            var arr2 = new Array(3);
            //console.log(arr2);

            // 빈칸이 있는 배열에 값 대입
            arr2[0] = "김밥";
            arr2[1] = "장조림";
            arr2[2] = "간계밥";

            // 빈칸이 아닌 3번 인덱스에 요소 추가
            arr2[3] = "무엇";
            // 자바 배열과 달리 길이 제한이 없어서
            // 설정된 크기보다 더 많은 요소를 추가할 수 있다.

            console.log(arr2);

            var area2 = document.getElementById("area2");
            area2.innerHTML="";

            for(var item of arr2){
                area2.innerHTML += item + "<br>";
            }

            // 크기가 지정되지 않은 arr1에 요소 추가
            arr1[0] ="포도";
            arr1[1] ="귤";
            arr1[2] ="사탕";
            arr1[3] ="음";
            
            area2.innerHTML += "<br>";
            for(var item of arr1){
                area2.innerHTML += item + "<br>";
            }

        }
    </script>
```
<br/><br/>




## 배열 관련 함수
자바스크립트에서는 배열도 객체이기 때문에 함수(메소드)를 가지고 있음.<br/>
indexof() : 일치하는 배열 요소의 인덱스를 반환<br/>
concat() : 다수의 배열을 결합<br/><br/>
toString(), join() : 배열 요소를 결합하여 하나의 문자열로 반환.<br/>
      - toString() : 요소 사이에 , 를 찍음.<br/>
      - join() : 요소 사이에 구분자를 지정.<br/><br/>
sort() : 배열을 오름차순 정렬<br/>
reverse() : 배열의 순서를 뒤집음.<br/>
{: .notice--success}
<br/><br/>

## concat() : 다수의 배열을 결합

```html
<button type="button" onclick="arrayTest4();">확인하기</button>
    <div id="area4" class="area"></div>
    <script>
        function arrayTest4(){
            var area4 = document.getElementById("area4");

            var arr1 = ["사과","딸기","바나나"];
            var arr2 = ["메로나","죠스바","스크류바"];
            var arr3 = arr1.concat(arr2); //arr1+arr2
            var arr4 = arr2.concat(arr1); //arr2+arr1
            var arr5 = arr1.concat(arr2,arr1,arr2) // arr1+arr2+arr1+arr2

            area4.innerHTML =""; // area4 기존 내용을 다 삭제
            
            area4.innerHTML += "arr1 : " + arr1+ "<br>";
            area4.innerHTML += "arr2 : " + arr2+ "<br>";
            area4.innerHTML += "arr3 : " + arr3+ "<br>";
            area4.innerHTML += "arr4 : " + arr4+ "<br>";
            area4.innerHTML += "arr5 : " + arr5+ "<br>";
        }
    </script>

```
<br/><br/>

## toString(), join() : 배열 요소를 결합하여 하나의 문자열로 반환.
 - toString() : 요소 사이에 , 를 찍음.<br/>
 - join() : 요소 사이에 구분자를 지정.<br/>

```html
<button type="button" onclick="arrayTest5();">확인하기</button>
    <div id="area5" class="area"></div>
    <script>

        function arrayTest5(){
            var area5 = document.getElementById("area5");

            var arr = ["사과","딸기","바나나","파인애플"]; //array타입
            var arr2 = arr.toString(); // string타입 (,로 구분)
            var arr3 = arr.join("/") // string타입 (/로 구분)

            area5.innerHTML ="";
            area5.innerHTML +="arr : " + arr + ", 자료형 : " + typeof(arr) +"<br>"; 
            area5.innerHTML +="arr2 : " + arr2 + ", 자료형 : " + typeof(arr2) +"<br>"; 
            area5.innerHTML +="arr3 : " + arr3 + ", 자료형 : " + typeof(arr3) +"<br>"; 
            //typeof, typeof() : 자료형을 확인하는 연산자
        }
    </script>
```
<br/><br/>




## sort(), reverse()
- sort() : 배열을 오름차순 정렬 <br/>
- reverse() : 배열의 순서를 뒤집음.

```html
<button type="button" onclick="arrayTest6();">확인하기</button>
    <script>
        function arrayTest6(){
            // 1~100사이 난수 10개를 배열에 저장 후
            // 오름차순/내림차순 출력

            var arr = new Array();

            for(var i=0; i<10; i++){
                arr[i] =  Math.floor(Math.random()*100+1);
            }

            console.log(arr);

            // ** sort(), reverse()는 원본 배열에 영향 미침.
            // == 원본 배열 자체가 정렬되서 원본 데이터가 손상.

            console.log(arr.sort()); //문자열이기 때문에 제일 앞자리를 기준으로 비교 정렬함.

            console.log(arr.sort(function(a,b){return a-b;}));

            console.log("정렬 후 원본 배열 확인 : " + arr);
            //  -> 원본 배열 자체가 정렬됨.

            // 내림차순
            // -> 오름차순으로 정렬된 배열 순서를 reverse()로 뒤집음
            console.log(arr.reverse());
        }
    </script>
```
<br/><br/>


## push(),pop(),shift(),unshift()
- push() : 배열의 맨 뒤에 요소 추가<br/>
- pop() : 배열의 맨 뒤 요소를 꺼내옴(제거) <br/><br/>
- shift() : 배열의 맨 앞에 요소를 제거<br/>
- unshift() : 배열의 맨 앞에 요소 추가<br/>

<br/>

```html
<button type="button" onclick="arrayTest7();">확인하기</button>
    <div id="area7" class="area"></div>    
    <script>
        function arrayTest7(){
            var area7 = document.getElementById("area7");

            var arr = ["어피치","제이지","무지","콘"];

            area7.innerHTML ="";

            area7.innerHTML += "기존 arr : " + arr + "<br>";

            push() : 배열 마지막에 추가
            arr.push("라이언");
            area7.innerHTML += "push 후 arr : " + arr + "<br>";

             pop() : 배열 마지막 요소를 꺼내서 제거
            area7.innerHTML += "arr 마지막 요소 pop : " + arr.pop()+"<br>";
            area7.innerHTML += "pop 후 arr : " + arr + "<br>";

             arr 배열의 마지막 요소("콘")를 빼서 arr배열 제일 앞에 배치
            arr.unshift(arr.pop());
            area7.innerHTML += "pop + unshift 후 arr : " + arr + "<br>";

            
        }
    </script>

```
<br/><br/>


## slice(),splice()
- slice() : 배열 요소를 선택해서 자르기 <br>
- splice() : 배열의 지정한 index 위치 요소 제거 후 새로운 요소를 추가

```html
<button type="button" onclick="arrayTest8();">확인하기</button>
    <div id="area8" class="area"></div>
    <script>
        function arrayTest8(){
            var area8 = document.getElementById("area8");

            var arr = ["Java", "DB", "HTML5", "CSS3"];

            //area8.innerHTML = ""; 안하는 이유,  = 대입이기 때문에 원래 있던 값 없어지고 대입 됨
            area8.innerHTML = "원본 arr : " + arr + "<br>";

            // slice(시작인덱스, 종료인덱스)
            // 원본 배열에 영향을 미치지 않음
            area8.innerHTML += "arr.slice(1,2) : " + arr.slice(1,2) + "<br>";
                                //1이상 2미만
            area8.innerHTML += "slice이후 원본 arr : " + arr + "<br>";

            // splice(시작인덱스, 제거할 수, 추가할 값)
            // 원본 배열에 영향을 미침.
            // 지정한 범위 요소 제거 -> 추가할 값을 해당 위치에 추가
            area8.innerHTML += "splice(2, 1, 'javascript') : " + arr.splice(2, 1, 'javascript') +"<br>";
            area8.innerHTML += "splice이후 원본 arr : " + arr + "<br>";

        }
    </script>

```
<br/><br/>

## 연습문제 


```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>10_연습문제_1input색</title>
    <style>
        .area{
            width :100px;
            height:100px;
            box-sizing: border-box;
            border : 1px solid black;
            display : inline-block;

            transition-duration : 2s;
        }

        .inputColor{
            padding : 0;
            margin :0;
            box-sizing : border-box;
            width: 100px;
            display : inline-block;
            transition-duration:2s;
        }

    </style>

</head>
<body>

    <div class="area" id="area1"></div>
    <div class="area" id="area2"></div>
    <div class="area" id="area3"></div>
    <div class="area" id="area4"></div>
    <div class="area" id="area5"></div>
    <div class="area" id="area6"></div>
    <br>
    <input type="text" class="inputColor" id="color1"> 
    <input type="text" class="inputColor" id="color2"> 
    <input type="text" class="inputColor" id="color3"> 
    <input type="text" class="inputColor" id="color4"> 
    <input type="text" class="inputColor" id="color5"> 
    <input type="text" class="inputColor" id="color6"> <br><br>
    <button type="button" id="btn">색변경</button>

    <script>
        document.getElementById("btn").onclick = function(){
            var inputlist = document.getElementsByClassName("inputColor");

            var arealist = document.getElementsByClassName("area");
            for(var i=0; i<inputlist.length; i++){
                arealist[i].style.backgroundColor = inputlist[i].value;
                inputlist[i].style.backgroundColor = inputlist[i].value;
            }   
        
            
        }
    </script>



</body>
```
<br/><br/>

