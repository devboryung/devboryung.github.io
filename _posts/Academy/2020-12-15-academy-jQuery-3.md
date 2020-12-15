---
title: "2020년 12월 15일"
excerpt: "jQuery - 3"
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

##  이벤트 연결 메소드

### mouse 관련 이벤트


```html
<h3>
    mouseenter/mouseleave <br>
    mouseover/mouseout
</h3>
<h4>mouseover-mouseout : 자식 요소에 접근 시에도 이벤트 핸들링함.<br></h4>
<div class="outer o1">
    <div class="inner"></div>
</div>
<p id="output"></p>


<hr>
<h4>mouseenter-mouseleave : 자식 요소에 접근 시에는 이벤트 핸들링을 하지 않음. </h4>
<div class="outer o2">
    <div class="inner"></div>
</div>
<p id="output2"></p>

<script>
    // mouseover, mouseout
    // 커서가 요소 위에 올라오거나 나갈 경우
    // + 자식 요소에 접근해도 동작을 함

    $(".o1").on("mouseover",function(){
        document.getElementById("output").innerHTML += "OVER! ";
    });
    $(".o1").on("mouseout",function(){
        document.getElementById("output").innerHTML += "OUT! ";
    });

    // mouseenter, mouseleave
    // 커서가 요소 위에 올라오거나 나갈 경우
    // + 자식 요소에 접근 시 이벤트가 발생하지 않음
    $(".o2").on("mouseenter",function(){
        document.getElementById("output2").innerHTML += "ENTER! ";
    });
    $(".o2").on("mouseleave",function(){
        document.getElementById("output2").innerHTML += "LEAVE! ";
    });
    

</script>
```
<br><br>

### input 관련 이벤트

```html
<h3>input 관련 이벤트</h3>
<pre>
focus :  요소가 focus를 받을 때
blur : 요소의 focus가 해제되었을 때

focusin :  선택한 요소의 자식이 focus를 받을 때
focusout :  선택한 요소의 자식이 focus가 해제되었을 때

change : 요소의 값이 변경되었을 때
    * select, checkbox, radio에 change 이벤트를 작성하는 경우
        -> 값이 변경 되었을 때

    * text 형태의 input, textarea에 change 이벤트를 작성하는 경우
        -> focus를 잃을 때 (== blur) 

select : 선택한 요소의 입력 영역 값이 블럭에 잡힌경우


</pre>
focus / blur : <input type="text" id="focus-blur"><br>

<div id="focus-in-out">
    focusin / focusout : <input type="text"><br>
</div>
change1 : <input type="checkbox" id="change1"><br>
change2 : <input type="text" id="change2"><br>
select : <input type="text" id="select"><br>


<script>
    // focus, blur
    $("#focus-blur").on("focus",function(){
        $(this).css("backgroundColor","lightblue");
    });
    $("#focus-blur").on("blur",function(event){
        $(event.target).removeAttr("style");
    });

    // focusin, focusof
    // 자식 요소에 초점이 맞춰졌을 때, 잃었을 때
    $("#focus-in-out").on("focusin",function(){
        $(this).children().css("backgroundColor","coral");
    });
    $("#focus-in-out").on("focusout",function(){
        $(this).children().removeAttr("style");
    });

    // change : 선택된 요소의 값이 변했을 때
    $("#change1").on("change",function(){
        console.log("체크박스 값이 변경됨");
        // 선택 되었을 때, 해제되었을 때 둘 다 적용됨
    });
    // change 이벤트가 textbox에서 인지되는 방법
    // == blut (포커스를 읽었을 때)
    $("#change2").on("change",function(){
        console.log("text box 값이 변경됨");
    });

    // select : 요소에 입력된 내용에 블럭이 잡힌 경우
    $("#select").on("select",function(){
        console.log("내용이 블럭에 잡힘");
    });
</script>
```
<br><br>

### 키보드 이벤트

```html
<h3>키보드 이벤트</h3>
<pre>
    keydown : 키보드가 눌러질 때.(모든 키 인식함)
    keypress : 글자가 입력될 때. 펑션키, 기능키 사용 못함
    keyup : 키보드가 떼어질 때. 펑션키, 기능키 사용 못함

    **
    keydown, keyprss : 누르고 있는 만큼 인식함
    keyup : 떼어질 때 한번만 인식함
</pre>
<input type="text" id="test">
<script>
    // $("#test").on("keydown",function(event){
    //     console.log(event.key);
    //     // event.key : 어떤 키가 입력됐는지 출력(한글 : Process로 출력됨)
    // });

    // $("#test").on("keypress",function(event){
    //     console.log(event.key);
    //     // event.key : 어떤 키가 입력됐는지 출력(한글, 펑션키, 기능키 X)
    // });

    $("#test").on("keyup",function(event){
        console.log(event.key);
        // event.key : 어떤 키가 입력됐는지 출력(한글 : Process로 출력/ 펑션키, 기능키 X)
    });
</script>
```
<br/>



### 동적으로 글자수 세기

```html
<h3>동적으로 글자수 세기</h3>
<div>
    <p><span id="counter">0</span>/150</p>
    <textarea id="test2" cols="70" rows="5" style="resize:none"></textarea>
</div>
<script>
    // input 이벤트
    // 입력과 관련된 모든 동작을 인식하는 이벤트

    $("#test2").on("input",function(){
        console.log($(this).val().length);
        // .val : textarea에 입력된 값
        // .length : 문자열의 길이

        // span태그에 작성되는 내용을 입력받은 문자열의 길이로 변경
        $("#counter").text($(this).val().length);

        // 입력된 글자 수가 150을 넘어서게 되는 경우 글자 수를 빨간색으로 변경
        if($(this).val().length>=150){
            $("#counter").css("color","red");
        }else{
            $("#counter").css("color","color");
        }
    });

</script>
```
<br><br>


### 동적으로 아이디 조건 확인
```html
<h3>동적으로 아이디 조건 확인</h3>
<!-- 
    아이디 입력 -> 입력 조건 검사(정규식) -> 중복 검사
    -->
<p>
    - 영어 대/소문자 + 숫자 + 특수문자(-_) 허용<br>
    - 총 6 ~14 글자

</p>
<label for="memberId">아이디 : </label>
<input type="text" name="memberId" id="memberId">
<span id="idCheck"></span>
<br>

<script>
    $("#memberId").on("keyup",function(){

        // 1) 입력된 값 얻어오기
        var memberId = $(this).val();

        // 2) 정규식 작성 후 검사
        var regExp = /^[A-Za-z\d-_]{6,14}$/;

        if(regExp.test(memberId)){
            $("#idCheck").text("사용 가능한 아이디 형식입니다.").css("color","gray");
        }else{
            $("#idCheck").text("사용할 수 없는 아이디 형식입니다.").css("color","red");
        }

    });
</script>
```
<br>

### 동적으로 비밀번호 일치 여부 확인
```html
<h3>동적으로 비밀번호 일치 여부 확인</h3>
<p>
    - "비밀번호" 입력 후 "비밀번호 확인" 입력 시 비밀번호 일치 여부 메세지 출력<br>
    - "비밀번호" 를 미입력한 상태로 "비밀번호 확인" 입력 시 <br>
    "비밀번호 확인"에 입력된 내용을 모두 지우고<br>
    "비밀번호를 입력해 주세요." 경고창 출력 후 focus를 "비밀번호"로 이동
</p>

<!-- 비밀번호 확인을 위해서 type을 text로 진행함. -->
<label for="memberPw1">비밀번호 : </label>
<input type="text" name="memberPw1" id="memberPw1">
<span id="pwCheck"></span><br>

<label for="memberPw2">비밀번호 확인 : </label>
<input type="text" name="memberPw2" id="memberPw2">

<script>
    // 1. "비밀번호 확인" 입력 시 
    // "비밀번호"에 작성된 문자열에 길이를 얻어와 0인지 검사
    // 0인 경우 "비밀번호를 입력해주세요." 경고창 출력 후
    // focus를 "비밀번호" 으로 이동( "비밀번호" .focus())



    $("#memberPw1, #memberPw2").on("input", function(){
        var memeberPw1 = $("#memberPw1").val();
        
        if(memeberPw1.length == 0){
            alert("비밀번호를 입력해주세요.");
            $(this).val('');
            $("#memberPw1").focus();
        } else {
            if (memeberPw1 == $("#memberPw2").val()) {
                $("#pwCheck").text("비밀번호가 일치합니다.")
                    .css("color", "blue").css("font-weight", "bold");
            } else {
                $("#pwCheck").text("비밀번호가 불일치합니다.")
                    .css("color", "red");
            }
        }
});

</script>
```
<br>

## trigger()메소드

```html
<h1>trigger()메소드</h1>
특정 이벤트를 강제로 발생시키는 메소드로<br>
사용자 정의 이벤트 발생 시 사용<br>
이벤트 발생 시 인자 값 전달 가능
<div class="trg" id="trg">
    click Num : <span>0</span>
</div>
<div class="increment" id="increment">click me!</div>

<script>
    var ctn = 0;
    $("#trg").on('click', function () {
        ctn++;
        $("span").text(ctn);
    });

    // click me 버튼
    $("#increment").on("click",function () {
        // 주황색 박스에 있는 click 이벤트 동작을 수행
        $("#trg").trigger('click'); // click 이벤트핸들러를 요청함
    });
</script>
<p>
    trigger메소드는 해당 요소를 실제로 클릭하는 것이 아닌 <br>
    해당 요소에 올라가있는 click이벤트에 대한 이벤트핸들러를 호출하는 것. <br><br>
</p>

<a href="https://www.naver.com" id="goNaver" onclick="justClicked()">네이버로 이동</a>
<button id="btnTrg">trigger를 통한 a태그 클릭</button>
<script>
    $(function () {
        $("#btnTrg").on("click",function () {
            $("#goNaver").trigger("click");
            // a태그의 링크 이동
        });
    });
    function justClicked() {
        console.log("justClicked!!");
    }
</script>

```
<br>

## show_hide

### 시각적 효과(display 속성 관련 메소드)

```html
<h1>시각적 효과(display 속성 관련 메소드)</h1>
<p> Effect 메소드 : 페이지에 애니메이션 효과를 만들기 위한 메소드 집합 </p>
<pre>
1. $(‘s’).메소드명();
2. $(‘s’).메소드명([speed]);
3. $(‘s’).메소드명([speed], [easing], [callback]);
speed : 실행속도(밀리세컨초) / 숫자 or slow, fast
easing : 변경되는 지점별 속도 / linear, swing 가능
callback : 메소드 실행 후 실행할 함수
</pre>
```
<br>



### show()와 hide()

```html
<h3>show()와 hide()</h3>
<p>문서 객체를 크게 하며 보여주거나 작게 하며 사라지게 한다</p>
<p>
    $(선택자).show(speed,easing,callback)
</p>

<button id="show">보여주기</button>
<button id="hide">숨기기</button><br>
<img id="river" src="image/river1.PNG">

<hr>

<script>
    $("#show").on("click",function(){
        $("#river").show();
    });

    $("#hide").on("click",function(){
        $("#river").hide();
    });
</script>
<br>

<button id="toggle">토글버튼</button>
<!-- 토글버튼 : 버튼 하나로 on/off 동작을 하는 버튼 -->
<br>
<img id="dog" src="image/sibadog.gif">

<script>
    $("#toggle").on("click",function(){
        $("#dog").toggle(1000);
    });

    if($("#dog").css('display')!='none'){	
            $("#dog").hide(1000);
        }else{
            $("#dog").show(1000);
        }

</script>

```
<br>

## fade
```html
<pre>
- fadeIn : 점점 진하게 변하면서 보여지는 효과
- fadeOut : 점점 희미하게 변하면서 사라지는 효과
- fadeTo : 설정한 값까지 희미해지는 효과 ☞ opacity값으로 투명도 설정
- fadeToggle : fadeIn과 fadeOut을 동시에 적용하는 메소드

$(‘s’).fadeIn/ fadeout / fadeToggle
([속도],[구간별속도],[callback]);
$(‘s’).fadeTo([시간],[투명도],[구간별속도],[callback]);

</pre>
```

### fadeIn(),fadeOut()
```html
<h3>fadeIn(),fadeOut()</h3>
	<button id="fadein">fadeIn()</button>
	<button id="fadeout">fadeOut()</button><br>

	<img id="diningtable" src="image/diningtable.gif">

	<script>
        $("#fadein").on("click",function(){
            $("#diningtable").fadeIn(1000);
        });
        $("#fadeout").on("click",function(){
            $("#diningtable").fadeOut(1000);
        });
	</script>
```

<br>
<br>

### toggle()
```html
<h3>toggle()</h3>
<button id="toggle">fadeToggle()</button>
<br>
<img id="cute" src="image/cute1.gif">
<script>
    $("#toggle").on("click",function(){
        $("#cute").fadeToggle(1000, function(){ alert("토글완료");});
    });
</script>

<hr>
<h3>fadeTo()</h3>
<img id="flower2" src="image/flower2.PNG">

<script>
    $("#flower2").hover(function(){
        // mouseenter
        $(this).fadeTo(1000,0.2);
    },function(){
        // mouseleave
        $(this).fadeTo(1000,1);
    });
</script>
```

<br>
<br>

## slide


```html
<h1>slide() 메소드</h1>
<h3>slideUp(), slideDown()</h3>

<div>첫 번째 메뉴</div>
<p class="contents">1</p>
<div>두 번째 메뉴</div>
<p class="contents">2</p>
<div>세 번째 메뉴</div>
<p class="contents">3</p>
<div>네 번째 메뉴</div>
<p class="contents">4</p>
<div>다섯 번째 메뉴</div>
<p class="contents">5</p>

<script>
    $("div").on("click",function(){
        if($(this).next().css("display") == "none"){
        // $(this) : 클릭된 div 태그
        // .next : 클릭된 div 태그 다음에 존재하는 요소 <p>
        // .css("display") : p태그의 style 중 display 속성 값을 얻어 옴
        // display : 초기값 none으로 설정되어있음.
            $(this).siblings("p.contents").slideUp();
            // 클릭된 div 태그의 형제 요소 중 p태그이면서 class가 contents인 요소를 선택
            $(this).next().slideDown();
        }else{
            $(this).next().slideUp();
        }
    });
</script>
```
<br>

## 애니메이션
애니메이션 효과를 만들기 위한 메소드 집합

### animate() 메소드

```html
<h3>animate() 메소드</h3>
<p>
    <code>.animate( properties [, duration ] [, easing ] [, complete ] )</code> <br>
    properties : 변경할 css속성을 객체로 전달. <br>
    duration : 기본값은 400, millisec|slow|fast <br>
    easing : 속도 옵션. swing|linear 기본값은 swing. <br>
    더 많은 easign function은 jqueryUI 또는 별도의 plugin을 사용해야함. <br>
    * swing : 시작/끝은 느리고, 중간은 빠름. (css animation-timing-function : ease-in-out)
    complete : 애니메이션이 완료되었을때 호출될 콜백함수 작성 <br>
</p>

<button id="btn1">실행</button>
<div id="test1" class="test"></div>

<script>
    //w3schools에서 기본지원되는 속성확인
    //https://www.w3schools.com/jquery/eff_animate.asp
    //color같은 경우 기본적으로 지원하지 않음. 
    //jquery-ui의 color플러그인을 import해서 사용해야함.
    //https://api.jqueryui.com/color-animation/
    /*
    color plugin은 다음속성을 지원함.
        * backgroundColor
        * borderBottomColor
        * borderLeftColor
        * borderRightColor
        * borderTopColor
        * color
        * columnRuleColor
        * outlineColor
        * textDecorationColor
        * textEmphasisColor
    */
    $(function () {
        $("#btn1").on("click", function () {

            $("#test1").animate({ width: "500px", backgroundColor: "blue" }, 1000, function () {
                alert("애니메이션 적용 완료")
            });
        });
    });
</script>
```
<br>

```html
<h3>이미지 animate()1</h3>
<img src="../image/cute1.gif" width="150px" height="100px" class="image" />
<script>
    $(document).ready(function () {
        $(".image").on("click", function (event) {
            $(this).animate({ 'left': '-158px' }, 'slow');//body의 기본 margin 8px을 추가함.
        });
    });
</script>

<hr>

<h3>이미지 animate()2</h3>
<p>
    가운데 위치한 div컨테이너 안에서 img를 우측으로 slide해서 감추어보자. <br>
    div컨테이너의 css속성 position, width, overflow등을 설정함.
</p>
<div id="scroller">
    <img src="../image/피카츄.gif" width="150px" height="100px" class="image2" />
</div>
<script>
    $(document).ready(function () {
        $(".image2").on("click", function (event) {
            console.log("a");
            $(this).animate({ 'right': '-160px' }, 1000);//-160은 image를 감싸고 있는 div에 대한 값이다.
        });
    });
</script>
```
<br>
