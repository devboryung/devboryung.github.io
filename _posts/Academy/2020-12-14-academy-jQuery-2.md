---
title: "2020년 12월 14일"
excerpt: "jQuery - 2"
search: true
categories: 
  - Academy
tags: 
  - jQuery
  - Javascript
  - HTML
  - CSS
toc: true
---

## Content 관련 메소드

### html() 메소드


```html
<h3>html() 메소드</h3>
<pre>
선택된 요소의 content 영역에 작성될 내용을 설정하거나
content 영역의 내용을 얻어오는 메소드.
(Javascript의 innerHTML과 같은 기능)

setter : 작성된 html 태그를 실제 태그로 인식함.
getter : 해당 요소의 content에 작성된 내용을 얻어옴.(태그도 포함함.)
</pre>

<h1 id="test1"><a href="https://www.naver.com">네이버로 이동</a></h1>
<h1 id="test2"></h1>

<script>

    $(document).ready(function(){
        // id가 test1인 요소의 내용을 태그 포함하여 얻어오기
        var content1 = $("#test1").html();
        console.log(content1);
        // <a href="https://www.naver.com">네이버로 이동</a>

        // id가 test2인 요소에 content1을 삽입
        // (html태그는 태그로 인식하도록 하기)
        $("#test2").html(content1);

        var tmp = "<a href='https://www.google.com'>구글로 이동</a>";
        $("#test2").html(tmp);
    });
</script>
```
<br><br>

### text() 메소드

```html
<h3>text() 메소드</h3>
<pre>
    선택된 요소의 content 영역에 작성될 내용을 설정하거나
    content 영역의 내용을 얻어오는 메소드.
    (Javascript의 innerText과 같은 기능)

    setter : 작성된 html 태그를 실제 태그로 인식하지 못 함(글자로만 취급).
    getter : 해당 요소의 content에 작성된 내용을 얻어옴.(태그를 무시함.태그는 삭제되고 내용만 얻어옴)
</pre>

<h1 id="test3"><a href="https://www.naver.com">네이버로 이동</a></h1>
<h1 id="test4"></h1>

<script>
    $(function(){
        var content2 = $("#test3").text();
        console.log(content2);
        // 네이버로 이동 --> 태그 무시하고 값만 얻어옴

        $("#test4").text("<a href='https://www.naver.com'>네이버로 이동</a>");
        // 태그가 인식이 안 되고 글자 그대로 출력 됨.

    });
</script>
```
<br><br>

### html(), text() 매개변수로 함수 전달하기

```html
<h3>html(), text() 매개변수로 함수 전달하기</h3>
<pre>
jQuery 메소드 중 html(), text(), val()는
매개변수로 함수를 전달할 수 있다.

함수의 형태는 
function(index, html){} 의 형식을 가지고 있음.

$("선택자").html(function(index,html){ });

index : 현재 접근중인 요소의 index를 반환
html : html()로 인해 변경되기 전의 content 값
</pre>

<div class="testClass1">테스트1</div>
<div class="testClass1">테스트2</div>
<div class="testClass1">테스트3</div>

<script>
    $(document).ready(function(){
        
        $(".testClass1").html(function(index,html){
            var str = "<h3>현재 인덱스 : " + index + ", 변경 전 content : " + html + "</h3>";
            return str;
        });
    });
</script>
```
<br/>


## 객체(== 태그, 요소) 생성 및 제거

### 객체 생성 방법

```html
<h3>객체 생성 방법</h3>
<div id="area1"></div>
<script>
    $(document).ready(function () {

        // 1) HTML 태그 작성 방법으로 객체 생성 방법
        var obj1 = "<h4>Create text with HTML</h4>";

        // 2) DOM 객체를 이용한 객체 생성 방법
        var obj2 = document.createElement("h4");
        var textNode = document.createTextNode("Create text with DOM");
        obj2.appendChild(textNode);

        // 3) jQuery 방식의 객체 생성 방법
        var obj3 = $("<h4>").text("create text with jQuery");
        // $("<h4>") == document.createElement("h4");

        $("#area1").append(obj1, obj2, obj3);
        $("#area1").append(obj1, obj2, obj3);
        // html태그를 직접 작성한 문자열을 이용해서 
        // 객체를 추가한 경우 여러번 재사용이 가능하지만
        
        // DOM,jQuery 방식으로 생성된 객체는
        // 한 번 삽입되면 재사용이 불가능함.
        // (여러번 삽입 시 마지막 삽입에만 적용됨);
        
        

    });
</script>
```
<br><br>


### 객체 삽입 메소드 - 1
```html
<h3>객체 삽입 메소드 - 1</h3>
<pre>
선택자 요소(A)를 기준으로 삽입 메소드 매개변수에
생성된 요소(B) 또는 함수의 리턴값을 추가
        
$(A).append(B) : A요소 내 뒷부분에 B를 추가(자식)
$(A).prepend(B) : A요소 내 앞부분에 B를 추가(자식)
$(A).after(B) : A의 요소 뒷부분에 B를 추가(형제)
$(A).before(B) : A의 요소 앞부분에 B를 추가(형제)
</pre>

<h1 id="test1"><span>A</span></h1>
<h1 id="test2"><span>A</span></h1>	
<h1 id="test3"><span>A</span></h1>
<hr>
<h1 id="test4"><span>A</span></h1>

<script>
$(document).ready(function () {

    // window.setInterval을 이용해서 1초마다 새로운 객체 삽입
        
    var index = 0;
    var arr = ["B","C", "D", "E"];

    var interval1 = setInterval(function(){
    // var el = $("<span>").addClass("added").text(arr[index++]);: jQuery 방식,, 마지막에 쓴것만(before) 적용 됨
    //  <span Class="added">B</span>

        var el = '<span class="added">'+ arr[index++] +'</span>';
        // 문자열로 작성된 것은 재사용이 된다.

        $("#test1").append(el);
        $("#test2").prepend(el);
        $("#test3").after(el);
        $("#test4").before(el);

        // E까지 출력된 후 interval 멈추기
        // -> window.clearInterval

        if(index > 3){ // 배열 내용을 모두 출력했을 때, (arr.length-1)
            window.clearInterval(interval1);
        // interval1 변수에 있는 setInterval을 없앰.
            }
        }, 1000);
});
</script>
```
<br>

### 객체 삽입 메소드 - 2
```html
<h3>객체 삽입 메소드 - 2</h3>
<pre>
생성된 요소(B)를 매개변수로 지정한 선택자(A) 요소에 추가
(part1의 메소드와 선택자, 생성구문의 순서가 반대)	

$(B).appendTo(A) : B를 A의 요소 내 뒷부분에 추가(자식)
$(B).prependTo(A) : B를 A의 요소 내 앞부분에 추가(자식)
$(B).insertAfter(A) : B를 A의 요소 뒤에 추가(형제)
$(B).insertBefore(A) : B를 A의 요소 앞에 추가(형제)
</pre>

<h1 id="test5"><span>A</span></h1>
<h1 id="test6"><span>A</span></h1>
<h1 id="test7"><span>A</span></h1>
<hr>
<h1 id="test8"><span>A</span></h1>

<script>
    $(document).ready(function () {

        var index2 =0;
        var arr2 = ["B","C","D","E"];

        var interval2 = setInterval(function(){
            
            var el = "<span class='added'>" + arr2[index2++]+ "</span>" ;

            $(el).appendTo($("#test5"));
            $(el).prependTo($("#test6"));
            $(el).insertAfter($("#test7"));
            $(el).insertBefore($("#test8"));

            
            if(index2 > arr2.length-1){
                window.clearInterval(interval2);
            }

        },1000);

    });
</script>
```
<br>

### 객체 복제 메소드
```html
<h3>객체 복제 메소드</h3>
<p>
    clone([true|flase]) : html요소를 복사하는 메소드<br>
    매개변수로 이벤트 복사여부를 지정(기본값 : false)
</p>

<button id="clone">clone</button>
<div id="clone-test">
    <div id="item1" class="item"><span>안녕?</span></div>
</div>

<script>

    $(function(){

        //.hover 매개변수 함수 2개 작성 가능, 첫번째 : 마우스가 들어왔을 때, 두번째:마우스가 떠났을 때
        $("#item1").hover(function(){
            // 마우스가 들어왔을 때
            
            // 마우스가 들어온 요소에 lime 클래스 추가
            $(this).addClass("lime");

        },function(){
            // 마우스가 떠났을 때
            $(this).removeClass("lime");
        });


        // id가 clone인 버튼이 클릭되었을 때
        $('#clone').click(function(){
            // id가 item1인 요소를 이벤트까지 복제하여 
            // id가 clone-test인 요소의 마지막 자식으로 추가하기
            $("#item1").clone(true).appendTo($("#clone-test"));
        });
    });

</script>

```
<br>

### 객체 제거 메소드
```html
<h3>객체 제거 메소드</h3>
<p>
    empty() : 모든 자식요소를 제거<br>
    remove() : DOM요소 잘라내기. 연관된 이벤트도 모두 삭제<br>
    detach() : DOM요소 잘라내기. 연관된 이벤트 모두 보관함.
    <!-- detach : 떼다, 파견하다 -->
</p>

<button id="empty">empty</button>&nbsp;
<button id="remove">remove</button>&nbsp;
<button id="detach">detach</button><br>

<div id="remove-test" class="box">
    <div id="item2" class="item"><span>안녕</span></div>
</div>
<div id="result" class="box"></div>

<script>
    // id가 item2인 요소에 마우스가 들어갔을 때 lime 클래스 추가
    // 마우스가 나갔을 때 lime 클래스 제거
    $("#item2").hover(function(){
        $(this).addClass("lime");
    },function(){
        $(this).removeClass("lime");
    });

    // empty 이벤트 핸들러
    $("#empty").click(function(){
        // id가 remove-test인 요소의 자식 요소들을 모두 제거
        $("#remove-test").empty();
    });

    // remove 이벤트 핸들러
    $("#remove").click(function(){
        var el = $("#item2").remove();
        console.log(el);
        // 요소를 잘라내서 페이지엔 안 보이지만 요소가 저장되어있음.(이벤트 저장X)
        $("#result").append(el);
    });

    // detach 이벤트 핸들러
    $("#detach").click(function(){
        var el = $("#item2").detach();
        console.log(el);
        // 요소를 잘라내서 페이지엔 안 보이지만 요소가 저장되어있음.(이벤트 포함)
        $("#result").append(el);
    });

</script>
```
<br>

## jQuery 메소드

### each() 메소드
```html
<h3>객체 제거 메소드</h3>
<pre>
객체, 배열을 순차 접근하는 for in, for of와 비슷한 메소드로

객체나 배열의 요소를 순차적으로 접근하면서 index도 알려주는 메소드.

[작성법]
1) $.each(object, function(index, item){});
object : 객체 또는 배열
index : 현재 접근중인 배역의 인덱스 또는 배열의 key 값
item : 현재 접근중인 배열의 요소 또는 객체의 value 값

2) $("선택자").each(function(index,item){});
$("선택자") : 여러 요소를 배열로 반환
</pre>

<div id="area1"></div>


<script>
$(document).ready(function(){
    // 객체배열 생성
    var arr = [{name : "네이버", link : "https://www.naver.com"},
                {name : "구글", link : "https://www.google.com"},
                {name : "w3school", link : "https://www.w3schools.com"}];

    $.each(arr, function(index,item){
        // console.log(index);
        // console.log(item);
    
        // a태그 생성
        var str = index + "번째 : " + item.name + "로 이동";
        var el = $("<a>").text(str).attr("href",item.link);
        // .attr("속성명","속성값")
        // -> 해당 요소에 특정 속성 추가
        $("#area1").append(el);
        $("#area1").append($("<br>")); // br태그 추가
    
    
    });

});
</script>


<hr>

<div id="wrap">
    <h1>item-0</h1>
    <h1>item-1</h1>
    <h1>item-2</h1>
    <h1>item-3</h1>
    <h1>item-4</h1>
</div>

<script>
    $(function(){
        // id가 wrap인 요소의 자식 중 h1태그 선택
        // $("#wrap").children("h1")
        
        $("#wrap>h1").each(function(index,item){
            $(item).addClass("highlight_" + index);
            // item : 현재 접근중인 배열 요소
            // 현재 배열 요소 : #wrap의 자식 중 h1 태그
            // $(item) : h1태그 하나 선택
            // index : 현재 접근중인 배열 요소의 인덱스(0~4);
        
        });
    
    });
</script>

```
<br>

## 이벤트

### 이벤트 관련 속성
```html
<h3>이벤트 관련 속성</h3>
<p>이벤트핸들러의 매개변수로 event객체를 전달함.
    인라인 이벤트 모델 방식으로 이벤트 설정 시 매개변수 키워드는 event로 고정.</p>
<button onclick="test1(event);">실행확인</button>
<script>
    function test1(e){
        console.log(e);
        console.log(e.target);
    }
</script>
```

<br>
<br>

### 이벤트 연결 on() / 해제 off()

```html
<h3>이벤트 연결 on() / 해제 off()</h3>
<p>
    요소 객체에 이벤트 발생 시 연결될 이벤트 핸들러를 지정하는 메소드이다.<br>

    $('선택자').bind() : 현재 존재하는 문서 객체만 이벤트 연결<br>
    $('선택자').unbind() : bind()로 연결된 이벤트 제거 <br>

</p>
<h4>
    bind(), unbind() 메소드는 jquery3.0부터 deprecated로 설정되어<br>
    대신 on(), off() 메소드를 사용함.<br><br>

    $('선택자').on("이벤트명", "이벤트 핸들러") : bind()대신에 on()으로 대체됨<br>
    $('선택자').off() : on()으로 연결된 이벤트 제거
</h4>


<h1 id="test1">마우스를 올려보세요</h1>
<button id="btn1">이벤트 제거</button>

<script>
    $("#test1").on("mouseenter", function(event){
        // mouseenter : 마우스가 들어왔을 때 라는 이벤트
        // event : 발생한 이벤트에 대한 정보가 담겨있는 객체
        // event.target : 이벤트가 발생한 요소

        // console.log(event.target);
        console.log(this);

        $(event.target).css("backgroundColor","lightgray").text("마우스 들어옴");

        $(this).css("cursor","pointer");
    });

    
    $("#test1").on({"mouseleave": function(event){
        // mouseleave : 마우스가 떠났을 때
        $(event.target).css("backgroundColor","yellow").text("마우스 나감");
    },"click":function(){
        // event 언제 써야될지 모를 때 항상 쓰는게 좋음.
        alert("마우스 클릭됨.");
    }});


    // #btn1 클릭시 #test1 이벤트 모두 제거

    $("#btn1").on("click",function(event){
        $("#test1").off("mouseenter").off("mouseleave").off("click").css("backgroundColor","blue").css("color","white");
    });
</script>
```
<br>

### $('선택자').on( events , selector , handler )
```html
<h3>$('선택자').on( events , selector , handler )</h3>
<pre>
선택자를 기준으로 매개변수로 전달된 selector에 지정한 event 발생 시
해당 handler를 동적 적용시켜 이벤트 처리를 할 수 있음.

** 페이지 로딩 후 스크립트 코드로 인해 동적으로 추가된 요소는
일반적인 방법으로 이벤트 지정이 불가능함. 
이 때 아래 이벤트 연결 방법을 사용하면됨.

    $(document).on( events , selector , handler );
    -> 정적이든, 동적이든 문서내에 작성된 요소 중
        selector에 해당하는 요소에 events가 발생하는 경우
        handler를 수행하라는 의미 
</pre>


<div id="wrap">
    <h2>클릭 시 h2 태그요소 추가</h2>
</div>

<script>
    var count = 0;

    //$("#wrap>h2").on("click",  function(event){ -> 현재 작성돼있는 h2에만 이벤트 적용됨.
    $(document).on("click", "#wrap>h2", function(event){  // -> 동적으로 추가된 h2에도 이벤트가 적용됨, 
                                                            // 현재 작성되어있는것 뿐만 아니라 새로 추가된 것을 포함해서
                                                            // 문서 내의 모든 #wrap>h2에 이벤트가 적용 된다.
        var h2 = "<h2>클릭으로 인해 추가됨-" + (count++) +"</h2>";
        
        $(event.target).after(h2);
    });
</script>
```
<br><br>

### one() 메소드
```html
<h3>one() 메소드</h3>
<p>이벤트에 대한 동작을 딱 한 번만 연결할 때 사용</p>
<button id="test4">클릭하세요</button>

<script>
    $("#test4").one("click", function () {
        alert("처음이자 마지막 이벤트 발생");
        $(this).css("background-color", "red")
            .text("클릭 불가")
            .prop("disabled", true);
    });
</script>
```
<br><br>

## 연습문제_1
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>연습문제_1</title>
    <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>

    <style>
        *{
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        .area{
            display: inline-block;
            width: 100px;
        }

        .box{
            width: 100%;
            height: 100px;

            border : 1px solid black;
            text-align: center;
            line-height: 100px;
            cursor : pointer;
            /* 마우스 커서의 모양을 포인터(손가락)모양으로 바꾸는 것 */
            transition-duration: 0.5s;
        }

        .input-box{
            width: 100%;
            margin: auto;
        }
    </style>

</head>
<body>
    <div class="area">
        <div class="box"></div>
        <input type="text" class="input-box">
    </div>
    <div class="area">
        <div class="box"></div>
        <input type="text" class="input-box">
    </div>
    <div class="area">
        <div class="box"></div>
        <input type="text" class="input-box">
    </div>
    <div class="area">
        <div class="box"></div>
        <input type="text" class="input-box">
    </div>
    <div class="area">
        <div class="box"></div>
        <input type="text" class="input-box">
    </div>

    <script>
        // box 클래스 요소를 클릭했을 때 box 클래스 요소의 배경색을 
        // 클릭된 box 바로 아래에 작성된 input태그의 값으로 변경

        $(".box").click(function(){

            var boxColor = $(this).css("backgroundColor");
            // getter 방법 : 요소의 배경색을 얻어 옴.
            
            console.log(boxColor);
            // rgba(0, 0, 0, 0)

            var inputColor = $(this).next().val();
            // this : 이벤트가 발생한 요소
            // $(this) : 이벤트가 발생한 요소 선택
            // .next() : 다음 형제 요소 선택
            // .val() : input 태그에 작성된 값을 얻어옴

            console.log(inputColor);
            
            if(boxColor == "rgba(0, 0, 0, 0)") {
                // 배경색이 지정되지 않았다면

                $(this).css("backgroundColor", inputColor); // setter 방법
                // $(this) : 이벤트가 발생한 요소 선택
                // css("backgroundColor", inputColor) 
                // : 해당 요소의 스타일 중 배경색을 inputColor로 변경
            } else {
                // 배경색이 지정되어 있다면

                $(this).css("backgroundColor", "rgba(0, 0, 0, 0)");
            }
        });

        // input 태그에 색상명이 작성되는 경우 
        // 작성된 글자색으로 input태그의 color변경

        $(".input-box").keyup(function(){
            // input-box 클래스를 가진 요소에서 
            // 키보드가 올라오는 이벤트가 발생했을 때

            $(this).css("color",$(this).val());
            // $(this).val() : 이벤트가 발생한 요소의 값을 얻어옴
        });


    </script>

</body>
</html>
```
<br>

## 연습문제_2
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>연습문제_2</title>
    <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
    <style>

    #wrap{
        width : 250px;
        height : 50px;
        background-color: pink;
    }

    .added{
            font-size: 40px;
        }
    </style>
</head>
<body>
    <button type="button" id="start">시작</button>
    <button type="button" id="stop">정지</button>
    <div id="wrap"></div>

    <script>

        $("#start").on("click",function(event){
            $("#wrap").empty();
            var count =0;
            
            var interval = setInterval(function(){
                el = '<span class="added">' + count++ + '</span>';
               
               if(count<=10){
                   $("#wrap").append(el);
                }else{
                    count =0;
                    $("#wrap").empty();
                }
            },200);
            
            $("#stop").on("click",function(event){
                 window.clearInterval(interval);
            });
        });



    </script>
</body>
</html>
```
<br>
