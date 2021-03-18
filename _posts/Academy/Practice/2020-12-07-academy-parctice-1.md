---
title: "2020년 12월 07일"
excerpt: "Practice-1"
search: true
categories: 
  - Practice
  - Academy
tags: 
  - Javascript
  - HTML
toc: true
---


## 연습문제 _ input값에 따라 색 변경하기

### 출력 예시
![색변경](https://user-images.githubusercontent.com/73421820/101496698-6a458b00-39ad-11eb-95fe-dc187c251e58.PNG)



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





## 연습문제 _ 로또 모양 출력하기
-1~45 난수 생성, 중복 제거, 오름차순 정렬


### 출력 예시
![로또](https://user-images.githubusercontent.com/73421820/101496705-6b76b800-39ad-11eb-806c-efc740d7ffe3.PNG)

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>10_연습문제_로또</title>
    <style>
        .wrapper{
            margin:auto;
        }
        .lotto{
            margin : 5px;
            width:50px;
            height:50px;
            /* border: 1px solid black; */
            border-radius: 50px; 
            display:inline-block;
            
            text-align: center;
            line-height : 50px;

            font-size : 20px;
            font-weight: bold;
            color : white;
        }
        #bonus{
            margin : 0;
        }

        .plus{
            display:inline-block;
            width:65px;
            height:50px;

            font-size : 5px;
            line-height : 50px;
            font-weight: bold;

            color : rgb(180, 170, 170)
        }
        
        button{
            margin : 10px;
            width: 60px;
            height: 20px;
            line-height :10px;

            text-align: center;
        }

        
        
    </style>
</head>
<body>

<div class="wrapper">

    <div class="lotto" id="lotto1"></div>
    <div class="lotto" id="lotto2"></div>
    <div class="lotto" id="lotto3"></div>
    <div class="lotto" id="lotto4"></div>
    <div class="lotto" id="lotto5"></div>
    <div class="lotto" id="lotto6"></div>
    <div class="plus" id="plus"> + 보너스 번호 </div>
    <div class="lotto" id="bonus" ></div>
</div>

    <br><br>
    <button type="button" id="lottobtn">for문</button>
    <button type="button" id="lottobtn2">indexof</button>
    <button type="button" id="lottobtn3">set</button>

    <script>
        
        document.getElementById("lottobtn").onclick =function(){

            // 로또 번호 입력할 배열 생성
            var lotto = document.getElementsByClassName("lotto");
           

             var num =[]; // 번호 담을 배열 생성 [보너스번호까지 7개]

            //  //랜덤 번호 생성
             for(var i=0; i<7; i++){
                var flag = true;
                 var ran = Math.floor(Math.random()*45+1);
                    for(var j=0; j<i ;j++){
                        if(num[j]==ran){
                             i--;
                             flag=false;
                        }
                    }
                    num.sort(function(a, b){return a-b;}); //오름차 정렬
                 if(flag){
                 num[i] = ran;
                 }   
              }
            console.log(num);

            for(var i=0; i<lotto.length; i++){
                lotto[i].innerHTML =""; // 이전 내용 삭제
                lotto[i].innerHTML += num[i];

                if(num[i]<=10){
                    lotto[i].style.backgroundColor =" #fbc400";
                }
                else if(num[i]<=20){
                    lotto[i].style.backgroundColor =" #85c3dd";
                }
                else if(num[i]<=30){
                    lotto[i].style.backgroundColor ="  #ff7272";
                }
                else if(num[i]<=40){
                    lotto[i].style.backgroundColor ="#be99e9";
                }
                else{
                    lotto[i].style.backgroundColor ="  #82be7d"
                }

            }
            
        }
        document.getElementById("lottobtn2").onclick = function(){
            
            var lotto = document.getElementsByClassName("lotto");

            var arr = []; //난수 담아 둘 배열 생성

            for(var i=0; i<lotto.length; i++){
                var num = Math.floor(Math.random()*45+1);

                if(arr.indexOf(num) == -1){ // arr의 index 중 num과 같은 숫자가 없다면
                    arr.push(num);
                }else{
                    i--;
                }
            }

            arr.sort(function(a, b){return a-b;}); // 오름차 정렬

            for(var i=0; i<lotto.length; i++){
                lotto[i].innerHTML =""; // 이전 내용 삭제
                lotto[i].innerHTML += arr[i];

                if(arr[i]<=10){
                    lotto[i].style.backgroundColor =" #fbc400";
                }
                else if(arr[i]<=20){
                    lotto[i].style.backgroundColor ="  #85c3dd";
                }
                else if(arr[i]<=30){
                    lotto[i].style.backgroundColor ="  #ff7272";
                }
                else if(arr[i]<=40){
                    lotto[i].style.backgroundColor ="#be99e9";
                }
                else{
                    lotto[i].style.backgroundColor =" #82be7d"
                }    
            }
        }

        document.getElementById("lottobtn3").onclick = function(){

            var lotto = document.getElementsByClassName("lotto");

            var set = new Set();
                   
            // set의 요소가 7개가 될 때 까지 난수 발생.
            while(set.size<7){
                set.add(Math.floor(Math.random()*45+1));
            }
            
            // 정렬을 위해서 set -> array로 변환
            var arr = new Array();

            for(var i of set){
                arr.push(i); 
            }
            // set에서 요소를 하나씩 꺼내 arr의 맨 뒤에 추가한다.

            arr.sort(function(a,b){return a-b;});


            for(var i=0; i<lotto.length ; i++){
                lotto[i].innerHTML =""; // 이전 내용 삭제
                lotto[i].innerHTML += arr[i];

                if(arr[i]<=10){
                    lotto[i].style.backgroundColor =" #fbc400";
                }
                else if(arr[i]<=20){
                    lotto[i].style.backgroundColor =" #85c3dd";
                }
                else if(arr[i]<=30){
                    lotto[i].style.backgroundColor ="  #ff7272";
                }
                else if(arr[i]<=40){
                    lotto[i].style.backgroundColor ="#be99e9";
                }
                else{
                    lotto[i].style.backgroundColor =" #82be7d"
                }
            }
        }
     
    </script>

</body>
```


<br/><br/>



