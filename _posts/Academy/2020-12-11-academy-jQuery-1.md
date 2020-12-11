---
title: "2020년 12월 11일"
excerpt: "jQuery - 1"
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

## jQuery
```html
<h3>jQuery란?</h3>
<pre>
기존에 복잡했던 클라이언트 측 HTML 스크립팅을
간소화 하기위해 고안된 Javascript 라이브러리.

jQuery는 적은 양의 코드로 빠르고, 풍부한 기능을 제공함.
(라이브러리(library) : 프로그램에 없는 추가적인 기능)
</pre>
```
<br>


### jQuery 라이브러리 연결 방법
```html
<h3>jQuery 라이브러리 연결 방법</h3>
<pre>
1) head 태그 내부에 script 태그를 작성하여
    jQuery 라이브러리를 HTML 문서의 로드가 시작될 때 연결하기.
        -> head 태그의 내용은 페이지가 나타나기 전에 해석됨.
        -> 페이지의 전체적인 로딩이 느려짐.

2) body태그 하단부에 script 태그를 작성하여
    jQuery 라이브러리를 HTML 문서의 로드가 끝날 때 연결하기.
    -> HTML 내용들이 먼저 화면에 보여지고
        이후 라이브러리와 같은 무거운 파일을 추가적으로 로딩함.

*script 태그 작성 방법
&lt;script src="js파일경로"&gt;&lt;/script&gt;

- jQuery 공식 홈페이지 : https://jquery.com/
- jQueru CDN 코드 제공 페이지 : https://code.jquery.com/

1. jQuery 라이브러리를 다운로드하여
    오프라인에서도 사용할 수 있는 연결 방법

2. CDN(content Delivery Network)를 이용하여
    온라인 환경에서 직접 다운로드 없이 연결
</pre>
```
<br>

### jQuery 기본 문법
```html
    <h3>jQuery 시작하기</h3>
    <pre>
    * jQuery 기본 작성 방법

    $("선택자").메소드명("속성명", "속성값");

    선택자 : css 선택자
    메소드 : jQuery 제공 메소드(함수)
</pre>


<button onclick="testFn();">문서 내 모든 글자를 빨간색으로 변경</button>

<script>
    function testFn() {
        $("*").css("color", "red");
        // css() 메소드 : 스타일 관련 기능 수행

        // document.getElementsByTagName("body")[0].style.color = "red";
    }
</script>
```
<br>

### jQuery ready() 함수
```html
<h3>jQuery의 ready() 함수</h3>
<pre>
    - Javascript의 window.onload 이벤트처럼
        HTML 페이지 로딩이 완료된 후 수행할 기능을 작성하는 함수

    - ready() 함수 작성 방법 3가지
    1) jQuery(document).ready(function() {코드;});

    2) $(document).ready(function() {코드;});
        ($ == jquery를 의미함)
    
    3) $(function() {코드;});

    - window.onload와의 차이점

    window.onload는 여러번 작성되면 마지막 작성된 이벤트만 수행됨
    ready()는 여러번 작성되어도 모두 수행됨
</pre>

<script>
    //window.onload 테스트
    console.log("test1 : " + test1);
    console.log("test2 : " + test2);

    window.onload = function() {
        console.log("로딩 완료 후 test1 : " + test1);
    }
    window.onload = function() {
        console.log("로딩 완료 후 test2 : " + test2);
    }
    // 마지막 window.onload 구문만 수행

    var test1 = "window.onload 테스트1";
    var test2 = "window.onload 테스트2"


    //ready() 함수
    console.log("test3 : " + test3);
    console.log("test4 : " + test4);

    $(document).ready(function() {
        console.log("로딩 완료 후 test3 : " + test3);
    });

    $(function(){
        console.log("로딩 완료 후 test4 : " + test4);
    })
    var test3 = "ready() 함수 테스트3";
    var test4 = "ready() 함수 테스트4";
</script>
```
<br><br>

## jQuery 선택자
### 기본 선택자
```html
<h1>기본 선택자</h1>
<!-- jQuery의 선택자는 CSS와 동일하다 -->

<h3>태그 선택자</h3>

<h5>Java</h5>
<h5>Oracle</h5>
<h5>HTML</h5>
<p>CSS3</p>
<p>Javascript</p>
<p>jQuery</p>

<script>
    $(document).ready(function(){

        // 한 종류의 태그를 선택하는 태그 선택자
        $("h5").css("color", "red");

        $("p").css("color", "blue");

        // 여러 선택자를 동시 사용할 경우 ,로 구분
        $("h5, p").css("backgroundColor", "yellowgreen");
    });
</script>
```
<br>

### 클래스 선택자
```html
<h3>클래스 선택자</h3>

<h1 class="item">test1</h1>
<h1 class="item select">test2</h1>
<h1 class="item">test3</h1>
<h1 class="select">test4</h1>
<script>
    $(function(){
        // 클래스가 item인 요소의 글자색을 orange로 변경
        $(".item").css("color", "orange");

        // 클래스가 item, select를 모두 가지고 있는 요소의 배경색을
        // yellow로 변경

        $(".item.select").css("backgroundColor", "yellow");

    });

</script>
```
<br>

### 아이디 선택자
```html
<h3>아이디 선택자</h3>

총 4 ~ 12 글자의 영어 대/소문자+숫자로 이루어진 문자열<br>
단, 첫 글자는 반드시 영어 소문자<br>
<input type="text" id="input1">
<button type="button" id="btn1">검사</button>

<script>
    // jQuery 이벤트 핸들러(리스너) 작성 방법
    // $("선택자").이벤트명(수행할함수);

    $("#btn1").click(function(){
        var regExp = /^[a-z][a-zA-Z\d]{3,11}$/;

        // input태그에 입력된 값 얻어오기
        // var value = document.getElementById("input1").value; <- javascript
        var value = $("#input1").val();

        console.log(value);

        if(regExp.test(value)){
            // value가 정규식에 매칭되면 true
            alert("규칙에 맞게 작성됨.")
        }else{
            alert("규칙에 부합하지 않음.");
        }
    })

</script>
```
<br>

### 자식 선택자와 후손 선택자
```html
<h3>자식 선택자와 후손 선택자</h3>
<!--
    자식 선택자 : >
    후손 선택자 : (띄어쓰기)
-->
<div id="area">
    <ul>
        <li><h3>사과</h3></li>
        <li>바나나</li>
        <li>오렌지</li>
        <li class="aaa">포도</li>
        <li class="aaa">딸기</li>
    </ul>

    <h3>자식 h3 태그</h3>
    <h3 class="aaa">class = "aaa"</h3>
</div>

<script>
    $(document).ready(function(){
        $("#area > h3").css("color", "violet");

        $("#area > ul > .aaa").css("backgroundColor", "red");

        $("#area h3").css("backgroundColor", "skyblue");
    });
</script>
```
<br>

### 속성 선택자
```html
<h3>속성 선택자</h3>
<pre>
    요소[속성] : 특정 속성을 가지고 있는 객체 선택.
    요소[속성 = 값] : 속성 안의 값이 특정 값과 같은 객체 선택.
    요소[속성 ~= 값] : 속성 안의 값이 특정 값을 단어로써 포함하는 객체 선택.
    요소[속성 ^= 값] : 속성 안의 값이 특정 값으로 시작하는 객체 선택.
    요소[속성 $= 값] : 속성 안의 값이 특정 값으로 끝나는 객체 선택.
    요소[속성 *= 값] : 속성 안의 값이 특정 값을 포함하는 객체 선택.
</pre>
<form>
    성별 : 
    <label for="male">남자</label>
    <input type="radio" name="gender" id="male" value="남자입니다.">
    <label for="female">여자</label>
    <input type="radio" name="gender" id="female" value="여자입니다.">

    <button type="button" id="btn2">확인</button> 
    <button type="reset">초기화</button>
    <br>

    <input type="text" id="input2" readonly>
</form>

<script>
    /* 확인 버튼 클릭 시 
    남자를 선택힌 경우 -> "남자입니다." 
    여자를 선택한 경우 -> "여자입니다."
    아무것도 선택하지 않은 경우 -> "성별을 선택해주세요."
    문자열을 input2에 출력    */

    $("#btn2").click(function(){
        var gender = $("input[name=gender]:checked");
        // checked : 체크된 요소만 선택하는 css 선택자
        console.log(gender);
        
        if(gender.length == 0){
            //"성별을 선택해주세요."
            $("#input2").val("성별을 선택해주세요.");
        }else{
            $("#input2").val(gender[0].value);
            // jQuery 선택자로 선택된 input 태그 값 : .val()
            // javascript 선택자 또는 javascript 변수, 배열로 선택
            // input 태그의 값 : .value
        }
    });

    // $("#btn2").click(function(){
    //     var gender = $("input[name=gender]:checked").val();
    //     console.log(gender);
    //     if(gender == undefined){
    //         $("#input2").val("성별을 선택해주세요.");
    //     }else{
    //         $("#input2").val( gender );
    //     }
    // });

</script>
```
<br>

### 입력 양식 필터 선택자
```html
<h1>입력 양식 필터 선택자</h1>
<pre>
input 태그의 type 속성에 따라 선택자를 이용하여 요소 선택이 가능함.
</pre>

텍스트 박스 : <input type="text"> <br>
넘버 박스 : <input type="number"> <br>
비밀번호 : <input type="password"> <br>

버튼 : <input type="button" value="버튼"> <br>
reset : <input type="reset" value="reset"> <br>
submit : <input type="submit" value="submit"> <br>

radio : <input type="radio" >
checkbox : <input type="checkbox">

<script>
    $(document).ready(function(){
        //input 태그 중 type이 text인 요소 모두 선택
        $("input:text").css("backgroundColor", "red");


        $("input[type=number]").css("backgroundColor", "orange");
        // input:number 선택자는 지원하지 않음
        $("input:password").css("backgroundColor", "yellow");

        $("input:button").css("width", "100px").css("height", "100px");
        // method chaining(메소드 체이닝)
        // 선행 메소드의 결과가 계속 특정 요소(객체)가 반환될 경우
        // 사용할 수 있음.
        
        // reset 버튼 배경색 파란색, 글자색 흰색
        $("input:reset")
            .css("backgroundColor", "blue").css("color", "white");
        
        // submit 버튼 너비 100px, 배경색 pink, 작성된 글자를 "제출"로 변경
        $("input:submit")
            .css({ "width":"100px", "backgroundColor":"pink" })
            .val("제출");

        // 자바스크립트 객체 표기법
        // { K:V, K:V, K:V .... } 

        // radio 버튼 체크하기
        $("input:radio").prop("checked", true);

        // 속성을 추가하는 메소드
        // .prop("속성명", 속성값) : 속성값이 true, false인 속성을 추가할 때 사용
                                    // (속성명만 작성하는 경우)
        // .attr("속성명", 속성값) : 속성명, 속성 값이 모두 작성되어야 하는 경우 사용

        //checkbox가 체크 되어있고, 너비 50px, 높이 50px
        $("input:checkbox").prop("checked", true)
            .css({"width":"50px", "height":"50px"});
    });
</script>


<h1>입력 양식 필터 선택자2</h1>

<h3>checkbox 상태에 따른 선택자</h3>
취미 : 
게임 <input type="checkbox" name="hobby" value="game">
축구 <input type="checkbox" name="hobby" value="football">
영화 <input type="checkbox" name="hobby" value="movie">

<script>

    function changeFn() {
        // console.log(1);

        // .prop("속성명")
        // 해당 속성이 존재하면 true, 없으면 false

        // 이벤트 핸들러 내부에서의 this : 이벤트가 발생한 요소
        //$(this) : 이벤트가 발생한 요소를 선택
        if( $(this).prop("checked") ) {
            // change  이벤트가 발생한 checked 요소에 체그가 되어 있는지 확인

            $(this).css({"width":"50px", "height":"50px"})
        } else{
            $(this).css({"width":"1em", "height":"1em"})
        }
    }

    // name 속성이 hooby인 input 태그의 값이 변화했을 때
    // changeFn() 함수 호출하여 수행함
    $("input[name=hobby]").change(changeFn);
</script>

<hr>

<h3>select > option 태그의 상태데 대한 선택자</h3>
<select id="national">
    <option>미국</option>
    <option>일본</option>
    <option>중국</option>
    <option selected>한국</option>
</select>

선택한 나라 : <input type="text" id="result" readonly>
<script>

    // select의 값이 변했을 때 이벤트 핸들러 수행
    $("#national").change(changeFn2);

    function changeFn2() {
        // #national의 자식 option 태그 중
        // 선택된 option 태그의 값을 value 변수에 저장
        var value = $("#national>option:selected").val();

        // #result 요소 값을 value로 변경
        $("#result").val(value);
    }

    // 문서 로딩이 완료된 후 changeFn2 함수 호출
    $(document).ready(changeFn2);
</script>

<hr>

<h3>input 태그 활성/비활성 상태에 대한 선택자</h3>
활성화 텍스트 박스 : <input type="text"> <br>
비활성화 텍스트 박스 : <input type="text" disabled> <br>

<br>

활성화 버튼 : <input type="button" value="활성화"> <br>
비활성화 버튼 : <input type="button" value="비활성화" disabled><br>

<script>
    $(document).ready(function(){
        // 활성화된 input 태그 요소의 배경색을 초록색
        // 비활성화된 input 태그 요소의 배경색을 빨간 색으로 변경

        $("input:enabled").css("backgroundColor", "rgb(200,250,150)");
        $("input:disabled").css("backgroundColor", "rgb(255,170,20)");
    });
</script>
```
<br>

### 순서에 따른 필터 선택자
```html
<h3>순서에 따른 필터 선택자</h3>
<table>
    <tr>
        <th>이름</th>
        <th>혈액형</th>
        <th>나이</th>
    </tr>
    <tr>
        <td>홍길동</td>
        <td>B형</td>
        <td>20</td>
    </tr>
    <tr>
        <td>이한솔</td>
        <td>O형</td>
        <td>27</td>
    </tr>
    <tr>
        <td>김삼순</td>
        <td>A형</td>
        <td>23</td>
    </tr>
    <tr>
        <td>고아성</td>
        <td>O형</td>
        <td>29</td>
    </tr>
    <tr>
        <td>김선희</td>
        <td>B형</td>
        <td>24</td>
    </tr>
    <tr>
        <td colspan="2">총원</td>
        <td>5명</td>
    </tr>
</table>

<script>
    // 선택자:odd  -> 홀수 번째 인덱스 요소 선택
    // 선택자:even -> 짝수 번째 인덱스 요소 선택
    // 선택자:first -> 첫 번째 인덱스 요소 선택
    // 선택자:last -> 마지막 번째 인덱스 요소 선택


    $(document).ready(function(){
        // 인덱스 홀수 번째 배경을 흰색
        $("tr:odd").css("backgroundColor", "white");

        // 인덱스 짝수 번째 배경을 lightgray
        $("tr:even").css("backgroundColor", "lightgray");
        
        // 첫 번째 줄의 배경색을 검은색, 글자색 흰색
        $("tr:first").css("backgroundColor", "black")
            .css("color", "white");
        
        // 마지막 줄의 배경색을 빨간색, 글자색 흰색
        $("tr:last").css("backgroundColor", "red")
            .css("color", "white");
        
        // td 요소에 작성된 값 가운데 정렬
        $("td").css("text-align", "center");
    });
</script>
```
<br><br>

## 필터링 메소드
```html
<h3>필터링 메소드</h3>
<pre>
선택자로 지정한 요소 그룹 내에서
특정 위치 또는 특정 요소만을 선택하는 jQuery 메소드
</pre>

<h4 class="test">test-1</h4>
<h4 class="test">test-2</h4>
<h4 class="test">test-3</h4>
<h4 class="test">test-4</h4>
<h4>test-5</h4>
<h4 class="test">test-6</h4>
<h5 class="test">test-7</h5>

<script>
    $(function(){
        //filter() 메소드
        // 매개변수에 작성된 선택자와 일치하는 요소를 선택
        
        // test 클래스를 가진 요소 중
        // 인덱스 번호가 짝수 번째인 요소의
        // 배경색을 tomato, 글자색 white로 변경
        $(".test").filter(":even")
            .css({"backgroundColor":"tomato", "color":"white"});

    
        // 함수를 매개변수로 하는 filter() 메소드
        $(".test").filter(function(index){
            // index : test 클래스 요소의 index 번호가 순차적으로 접근
            return index % 2 == 1;
            // 반환값이 true이면 해당 index 요소를 선택함
        })
            .css({"backgroundColor":"yellowgreen", "color":"white"});

        // h4 태그 요소 중 첫 번째 요소의
        // 글씨 크기를 2배로 변경
        // first() : 선택된 요소 중 첫 번째 요소 선택
        $("h4").first().css("font-size", "2em");

        //h4 태그 요소 중 마지막 요소의
        // 내용을 "마지막 h4 태그 요소"로 변경
        // last() : 선택된 요소 중 마지막 요소 선택

        $("h4").last().text("마지막 h4 태그 요소");

        //text("내용") : 선택한 요소의 content를 "내용"으로 변경

        // eq(index) 메소드 : index 번째 요소를 선택
        // h4 태그 중 3번째 인덱스 요소의 배경색을 blue 변경
        $("h4").eq(3).css("backgroundColor", "blue");

        // not("선택자") : 그룹 내에서 "선택자" 요소를 제외한 요소 선택
        // h4 태그 중 클래스가 test가 아닌 요소의 배경색을 yellow로 변경
        $("h4").not(".test").css("backgroundColor", "yellow");
    }); 
</script>
```
<br><br>

## 순회(탐색)메소드
```html
<h1>순회(탐색) 메소드</h1>
<h3>Ancestors(조상) 메소드 : 
    선택된 요소의 상위 요소들을 선택할 수 있는 메소드</h3>
<pre>
$('요소명').parent() 
    - 선택된 요소의 바로 위 상위 요소

$('요소명').parents([매개변수]) 
    - 선택된 요소의 모든 상위 요소 리턴.
    - 매개변수가 있으면 매개변수와 일치하는 부모만 리턴. 

$('요소명').parentsUntil(매개변수)
    - 선택된 요소부터 매개변수 요소까지 범위의 요소 리턴.

</pre>

<div class="wrap">
    <div class="type">div (great-grand parent)
        <ul>ul (grand parent)
            <li>li (direct parent)
                <span>span</span>
            </li>
        </ul>
    </div>
    
    <div class="type">div (grand parent)
        <p>p (direct parent)
            <span>span</span>
        </p> 
    </div> 
</div>
<script>
    $(document).ready(function(){

        // parent() : 선택된 요소 바로 상위 요소를 선택
        // span 태그의 부모 요소의 스타일 변경
        $("span").parent()
            .css({"border":"2px solid red", "color":"red"});

        // parents() : 선택된 요소의 모든 상위요소 선택
        // li 태그의 모든 상위 요소 스타일 변경
        $("li").parents().css("color", "blue");

        // parents("선택자") : 선택된 요소의 모든 상위 요소 중
        //                    "선택자"와 일치항는 요소만 선택
        // li 태그의 모든 상위 요소 중 div만 스타일 변경
        $("li").parents("div").css("border", "2px dashed magenta");

        // parentsUntil("선택자") :
        // 선택된 요소부터 "선택자"로 지정된 요소 이전까지의 모든 부모 선택
        // span 태그부터 div 부모 요소 이전까지의 스타일 변경
        $("span").parentsUntil("div").css("backgroundColor", "lightgreen");
    });

</script>
```
<br>

### descendants(자손, 후손) 메소드
```html
<h3>descendants(자손, 후손) 메소드 : 선택된 요소의 하위 요소들을 선택할 수 있는 메소드</h3>
<pre>
$('요소명').children([매개변수]) 
    - 선택된 요소의 모든 자식 객체 리턴
    - 매개변수가 있으면 매개변수와 일치하는 자식 객체만 리턴

$('요소명').find(매개변수)
    - 선택된 요소의 인자와 일치하는 모든 후손 객체 리턴
</pre>

<div class="wrap">
    <div class="type">div (great-grand parent)
        <ul>ul (grand parent)
            <li>li (direct parent)
                <span>span</span>
            </li>
        </ul>
    </div>

    <div class="type">div (grandparent)
        <p>p (direct parent)
            <span>span</span>
        </p>
    </div>
</div>

<script>
    // .css()의 매개변수로 스타일을 작성하는 것은 깔끔하지 않음 
    //  -> 별도로 객체를 미리 생성해둠.
    var style1 = {"color":"red", "border":"2px solid red"};
    var style2 = {"color":"blue", "border":"2px solid blue"};
    var style3 = {"color":"green", "border":"2px solid green"};
    var style4 = {"color":"orange", "border":"2px solid orange"};
    var style5 = {"color":"purple", "border":"2px solid purple"};

    //children() : 선택된 요소 바로 아래 요소(자식) 선택
    // 클래스가 wrap인 요소의 바로 아래 자식의 스타일을 style1로 설정함
    $(".wrap").children().css(style1);

    // 클래스가 wrap인 요소의 자식의 자식 스타일을 style2로 설정
    $(".wrap").children().children().css(style2);

    // children("선택자")
    // 선택된 요소의 자식 중 "선택자"인 자식만 선택

    // 클래스가 wrap인 요소의 자식의 자식 중 ul 태그만 스타일을 style3으로 설정
    $(".wrap").children().children("ul").css(style3);

    // 클래스가 wrap인 요소의 후손 중
    // li 태그의 스타일을 style4로 설정 (children()만 사용)
    $(".wrap").children().children().children("li").css(style4);

    //find("선택자") : 선택된 요소의 후손 중 선택자와 일치하는 모든 후손 선택
    //클래스가 wrap인 요소의 후손 중 span 태그의 스타일을 style5로 설정
    $(".wrap").find("span").css(style5);
</script>
```
<br>

### sideways(옆으로, 옆에서) 메소드
```html
<h3>sideways(옆으로, 옆에서) 메소드 :
    같은 레벨에 있는 요소(형제)를 선택할 수 있는 메소드
</h3>

<pre>
$('요소명').siblings([매개변수])
- 선택된 요소와 같은 레벨(형제)에 있는 모든 요소 리턴
- 매개변수가 있으면 같은 레벨에 있는 요소 중 매개변수와 일치하는 모든 요소 리턴

$('요소명').next()
- 선택된 요소의 같은 레벨 중 선택된 요소 다음 한 개 요소 리턴

$('요소명').nextAll()
- 선택된 요소의 같은 레벨 중 선택된 요소 다음의 모든 요소 리턴

$('요소명').nextUntil(매개변수)
- 선택된 요소의 같은 레벨 중 매개변수 이전 까지의 모든 요소 리턴


$('요소명').prev()
- 선택된 요소의 같은 레벨 중 선택된 요소 이전 한 개 요소 리턴

$('요소명').prevAll()
- 선택된 요소의 같은 레벨 중 선택된 요소 이전의 모든 요소 리턴

$('요소명').prevUntil(매개변수)
- 선택된 요소의 같은 레벨 중 매개변수 이전 까지의 모든 요소 리턴
</pre>

<div class="wrap">div (parent)
    <p>p</p>
    <span>span</span>
    <h2>h2</h2>
    <h3>h3</h3>
    <p>p</p>
</div>

<script>
    var style1 = { "color": "red", "border": "2px solid red" };
    var style2 = { "color": "blue", "border": "2px solid blue" };
    var style3 = { "color": "green", "border": "2px solid green" };
    var style4 = { "background": "aqua", "color": "red" };

    // siblings() : 특정 요소의 형제요소 모두 선택
    // h2 태그의 형제 요소를 선택하여 스타일을 style1로 설정
    $("h2").siblings().css(style1);  //h2 태그는 제외된다.

    // siblings("선택자") : 특정 요소의 형제 요소의 형제 중
    // "선택자"와 일치하는 요소만 선택
    // h2 태그의 형제 요소 중 p 태그만 스타일을 style2로 설정
    $("h2").siblings("p").css(style2);  
    
    // next() : 선택된 요소의 다음 형제 요소 선택
    // h2 태그의 바로 다음 요소의 스타일을 style3으로 변경
    $("h2").next().css(style3);

    // nextAll() : 선택된 요소 다음에 오는 모든 형제 선택
    // h2 태그 다음에 오는 모든 형제 요소의 스타일을 sytle4로 설정
    $("h2").nextAll().css(style4);

    // nextUntil("선택자")
    // 선택된 요소 다음부터 "선택자" 이전까지의 요소를 모두 선택
    // h2 태그 다음 형제부터 p 태그 이전까지 요소의 
    // 글자 크기를 3em으로 변경
    $("h2").nextUntil("p").css("font-size", "3em");

    // is("선택자")
    // 지정된 범위 내에 "선택자" 요소가 존재하는지 확인하는 메소드
    // 존재하면 true, 아니면 false 반환

    //h2 태그 다음에 오는 모든 형제 요소 중
    //p 태그가 있는지 확인
    console.log($("h2").nextAll().is("p"));

</script>
```