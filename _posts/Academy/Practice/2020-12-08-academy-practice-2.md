---
title: "2020년 12월 08일"
excerpt: "Parctice-2"
search: true
categories: 
  - Parctice
  - Academy
tags: 
  - Javascript
  - HTML
toc: true
---

## 연습문제

### 출력 예시
![연습문제](https://user-images.githubusercontent.com/73421820/101490927-63ffe080-39a6-11eb-8665-0b96c43334ba.PNG)

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>13_연습문제_2</title>
</head>
<body>
    <button type="button" id="add">추가</button>
    <button type="button" id="sum">합</button>
    <button type="button" id="avg">평균</button>
    <button type="button" id="reset">초기화</button>
    <br><br>
    
    <div id="wrapper">
        1번째 입력 : <input type="number" class="input"><br>
        
    </div>
    <div id="result"></div>

    <script>
        var count=1;

        document.getElementById("add").onclick = function(){

            // input 요소 값 유지를 위하여 새로운 요소 추가 전
            // 이전 요소들의 value값을 별도 배열에 저장.
            var arr = document.getElementsByClassName("input");
            var num = [];
            for(var i of arr){
                num.push(i.value);
            }

            document.getElementById("wrapper").innerHTML +=
            ++count +"번째 입력 : <input type='number' class='input'><br>";

            document.getElementById("result").innerHTML =""; // 합/평균 값 삭제

            // 이전 input 값을 새로 추가된 요소에 적용한다.
            var arr2 = document.getElementsByClassName("input");
            for(var i=0;i<num.length; i++){
                arr2[i].value = num[i];
            }
        }
           

        document.getElementById("reset").onclick = function(){
            document.getElementById("wrapper").innerHTML =
            "1번째 입력 : <input type='number' class='input'><br>";
            count =1;
            
            document.getElementById("result").innerHTML="";// 합/평균 값 삭제

        }


        document.getElementById("sum").onclick =function(){
            document.getElementById("result").innerHTML =
            "<br><b>합계</b> : " + sumAll() +"<br>";
        }

        function sumAll(){
            var arr = document.getElementsByClassName("input");
            var sum = 0;

            for(var i of arr){
                sum += Number(i.value);
            }
            return sum;
        }


        document.getElementById("avg").onclick =function(){
          
            document.getElementById("result").innerHTML =
            "<br><b>평균</b> : " + avg() +"<br>";
        }

        function avg(){
          var arr = document.getElementsByClassName("input");
        
            var avg = sumAll()/arr.length;
            return avg;
        }

    </script>
</body>
</html>
```
