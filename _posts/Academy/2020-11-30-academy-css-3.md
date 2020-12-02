---
title: "2020년 11월 30일"
excerpt: "CSS-3"
search: true
categories: 
  - Academy
tags: 
  - HTML
  - CSS
toc: true
---

## 15_레이아웃 스타일4.html


```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>15_레이아웃 스타일 4</title>
    <link rel="stylesheet" href="15_레이아웃스타일4.css" type="text/css">
</head>

<body id="body-top">
    <h1>배치 관련 스타일(position)</h1>
    <p>요소의 위치를 지정하는 속성</p>

    <hr>
    <h3>relative/absolute</h3>
    <pre>
    position : relative; 속성
    - relative가 설정된 요소의 하위 요소들은
       top, bottom, left, right 속성을 사용하여 위치를 지정할 수 있음.    
    
    ex) top : 50px --> 부모 요소로 부터 상단이 50px 떨어진 위치
    
    position : absolute;
    - 기본적인 요소의 배치 순서를 무시하고 지정된 절대 위치에 요소를 표시함
    - 단, 상위 요소가 relative, fixed, absolute 라는 position 속성값을
      가지고 있을 경우 상위 요소를 기준으로 하여 상대적인 위치를 가지게 된다.
    </pre>


    <div class="outer">
        <div class="first">first</div>
        <div class="second">second</div>
        <div class="third">third</div>
    </div>

    <h3>position : fixed; (고정 위치)</h3>
    <p>
        상위 요소를 기준으로하여 해당 요소가 항상 고정된 위치를 가지게 하는 속성. 
    </p>
    <div class="fixed-btn">
        <a href="#body-top">TOP</a>
    </div>

    <br> <br> <br> <br> <br>
    
    <hr>
    <h3>요소 Stack 순서 지정 스타일 (z-index)</h3>
    <pre>
    - 요소가 쌓이는 순서를 지정하는 속성
    - 양의 정수값을 이용하여 값을 부여할 수 있고
      값이 클수록 위쪽에 쌓임.
    
    <b style="color:red">
        (주의사항)
        z-index는 relative, absolute, fixed position이
        설정된 요소에서만 정상 동작 함.
        ->만약 z-index가 정상 동작하지 않는 경우
        상위 요소에 position:relative; 속성을 추가하면 해결 될 가능성이 높음. 
    </b>
    </pre>

    <div class="wrap">
        <div class="z-test1">1</div>
        <div class="z-test2">2</div>
        <div class="z-test3">3</div>
    </div>

    <hr>
    <h3>요소 표시 여부 지정 스타일</h3>
    <pre>
    display : none;
    - 요소가 보이지 않고, 공간도 차지하지 않음.
    
    visibility : hidden;
    - 요소가 보이지 않지만, 요소 크기 만큼의 공간을 차지함.

    </pre>

    <div class="vis-test vis-test1"></div>
    <div class="vis-test vis-test2"></div>
    <div class="vis-test vis-test3"></div>
    <div class="vis-test vis-test4"></div>
    <div class="vis-test vis-test5"></div>


    <hr>
    <h1>요소 정렬 스타일(float, clear)</h1>
    <pre>
    float: 요소를 띄워서 정렬하는 속성
    -> float가 설정된 요소들은 설정된 방향으로 흐르듯이 정렬되어 배치됨.

    clear : float로 인해 요소들이 띄워져 흐르는 상태를 해제하는 속성.
    - 해제할 방향을 지정하면 그 방향의 float가 해제됨
    ex) float : left; 해제하려면 clear : left;
    ex) float : right; 해제하려면 clear : right;
    ex) float : left 와 float : right; 가 동시 존재할 때 
        해제하려면 clear : both; ( left, right 둘 다 해제)

    </pre>

    <h4>float가 지정되지 않았을 때</h4>
    <div class="float-test">
        <div>1</div>
        <div>2</div>
        <div>3</div>
        <div>4</div>
        <div>Hello</div>
        <div>World</div>
        <div>5</div>
    </div>

    
    <h4>float : left;</h4>
    <div class="float-test">
        <div class="float-left">1</div>
        <div class="float-left">2</div>
        <div class="float-left">3</div>
        <div class="float-left">4</div>
        <div>Hello</div>
        <div>World</div>
        <div class="clear-left">5</div>
    </div>


    <h4>float : right;</h4>
    <div class="float-test">
        <div class="float-right">1</div>
        <div class="float-right">2</div>
        <div class="float-right">3</div>
        <div class="float-right">4</div>
        <div>Hello</div>
        <div>World</div>
        <div class="clear-right">5</div>
    </div>


    <h4>float : left + right;</h4>
    <div class="float-test">
        <div class="float-left">1</div>
        <div class="float-left">2</div>
        <div class="float-right">3</div>
        <div class="float-right">4</div>
        <div>Hello</div>
        <div>World</div>
        <div class="clear-both">5</div>
    </div>


</body>
</html>
```
<br/>
<br/>


## 15_레이아웃스타일4.css
<br/>

```css
/* 모든 div 태그에 검정 실선 테두리 적용 */
div{ border : 1px solid black;}

.outer{
    border : 1px solid red;
    position : relative;
    /* 하위 요소들을 top, bottom, left, right를 통해 위치 지정을 할 수 있음 */

}

.first{
    width : 300px;
    height : 300px;
    background-color: yellow;
}

.second{
    width : 200px;
    height : 200px;
    background-color : yellowgreen;
    position : absolute;
    /* 1) 기본적인 요소의 배치 순서를 무시 */
    /* 2) 상위 요소가 relative -> 상대적인 위치를 가짐 */
    top : 50px;
    left : 50px;
    /* 상위 요소로부터 위쪽 50px, 왼쪽 50px 떨어진 위치에 배치  */
    
}

.third{
    /* 가로 100px, 세로 100px, 빨간 배경색, second 가운데 위치하게*/
    width : 100px;
    height : 100px;
    position : absolute;
    background-color: red;
    top: 100px;
    left: 100px;
}

/* 고정 배치 fixed */
.fixed-btn{
    width : 50px;
    height : 50px;

    /* 글자(인라인요소) 가운데 정렬 */
    text-align: center;

    /* 장평 조절 */
    line-height: 50px;

    /* 테두리를 둥근 모양으로 지정 */
    border-radius : 50%;

    /*고정 배치*/
    position : fixed;

    /* body 태그는 기본적으로 relative 이기 때문에
    right, bottom 사용 시 적용됨 */
    right:50px;
    bottom:50px;

}

.fixed-btn > a {
    color : black;

    /* 밑줄 제거 */
    text-decoration: none;

}

/* z-index */
.wrap {
    position: relative;
}

.z-test3{
    background-color: red;
    width : 150px;
    height : 150px;
}

.z-test2{
    background-color: green;
    width: 100px;
    height: 100px;
    position: absolute;
    top:50px;
    left: 50px;
    z-index : 5;
}

.z-test1{
    background-color:yellow;
    width: 50px;
    height:50px;
    position : absolute;
    top: 100px;
    left:100px;
    z-index : 100000;
}

/*요소 표시 여부 지정 스타일*/
.vis-test{
    width:100px;
    height:100px;
    border : 1px solid black;
}

.vis-test1{background-color:  red;}
.vis-test2{
    background-color: orange;
    visibility:hidden;
    /*요소를 숨기지만 요소 크기만큼의 공간은 차지*/     
}
.vis-test3{background-color:yellow;}
.vis-test4{
    background-color:  green;
    display : none;
    /*요소를 숨기고, 공간을 차지하지 않음*/
}
.vis-test5{background-color: blue;}

/* float */
.float-test{
    background-color: skyblue;
    width: 300px;
    height: 200px;

}

.float-left{
    background-color: yellow;
    width: 50px;
    height: 50px;
    
    float : left;
}

/* float 해제 */
.clear-left{
    clear:left;
    background-color: red;
}

.float-right{
    background-color: pink;
    width: 50px;
    height: 50px;

    float:right;
    /*먼저 작성된 순서대로 오른쪽으로 배치된다.*/

}
.clear-right{
    clear : right;
    background-color: red;
}
.clear-both{
    clear :both;
    background-color: red;
}


```
<br/>
<br/>

## 16_변형1

```html
<!doctype html>
<html lang="ko">

<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>16_변형1</title>
	<style>
		img {
			width: 250px;
			height: 200px;
		}

		.trans1:hover {
			-ms-transform: translateX(50px);
			/* IE 9 */
			-webkit-transform: translateX(50px);
			/* Safari */
			transform: translateX(50px);
			/* Standard syntax */
		}

		.trans2:hover {
			-ms-transform: translateY(50px);
			/* IE 9 */
			-webkit-transform: translateY(50px);
			/* Safari */
			transform: translateY(50px);
			/* Standard syntax */
		}

		.trans3:hover {
			-ms-transform: translate(50px, 50px);
			/* IE 9 */
			-webkit-transform: translate(50px, 50px);
			/* Safari */
			transform: translate(50px, 50px);
			/* Standard syntax */
		}

		.trans4:hover {
			-ms-transform: scaleX(2);
			/* IE 9 */
			-webkit-transform: scaleX(2);
			/* Safari */
			transform: scaleX(2);
			/* Standard syntax */
		}

		.trans5:hover {
			-ms-transform: scaleY(2);
			/* IE 9 */
			-webkit-transform: scaleY(2);
			/* Safari */
			transform: scaleY(2);
			/* Standard syntax */
		}

		.trans6:hover {
			-ms-transform: scale(2, 2);
			/* IE 9 */
			-webkit-transform: scale(2, 2);
			/* Safari */
			transform: scale(2, 2);
			/* Standard syntax */
		}

		.trans7:hover {
			-ms-transform: rotate(45deg);
			/* IE 9 */
			-webkit-transform: rotate(45deg);
			/* Safari */
			transform: rotate(45deg);
			/* Standard syntax */
		}

		.trans8:hover {
			-ms-transform: skewX(45deg);
			/* IE 9 */
			-webkit-transform: skewX(45deg);
			/* Safari */
			transform: skewX(45deg);
			/* Standard syntax */
		}

		.trans9:hover {
			-ms-transform: skewY(45deg);
			/* IE 9 */
			-webkit-transform: skewY(45deg);
			/* Safari */
			transform: skewY(45deg);
			/* Standard syntax */
		}

		.trans10:hover {
			-ms-transform: skew(10deg, 10deg);
			/* IE 9 */
			-webkit-transform: skew(10deg, 10deg);
			/* Safari */
			transform: skew(10deg, 10deg);
			/* Standard syntax */
		}
	</style>
</head>

<body>
	<h1>2차원 변형</h1>
	<h3>좌우로 움직이기</h3>
	<img src="sample/image/flower1.png" class="trans1">
	<hr>
	<h3>위아래로 움직이기</h3>
	<img src="sample/image/flower2.png" class="trans2">
	<hr>
	<h3>대각선으로 움직이기</h3>
	<img src="sample/image/flower3.png" class="trans3">
	<hr>
	<h3>가로로 확대 축소</h3>
	<img src="sample/image/flower4.png" class="trans4">
	<hr>
	<h3>세로로 확대 축소</h3>
	<img src="sample/image/flower5.png" class="trans5">
	<hr>
	<h3>전체 확대 축소</h3>
	<img src="sample/image/river1.png" class="trans6">
	<hr>
	<h3>지정한 각도만큼 회전</h3>
	<img src="sample/image/river2.png" class="trans7">
	<hr>
	<h3>지정한 각도만큼 가로로 뒤틀기</h3>
	<img src="sample/image/city1.png" class="trans8">
	<hr>
	<h3>지정한 각도만큼 세로로 뒤틀기</h3>
	<img src="sample/image/tower1.png" class="trans9">
	<hr>
	<h3>지정한 각도만큼 전체 뒤틀기</h3>
	<img src="sample/image/forest1.png" class="trans10">
	<hr>


</body>

</html>
```

<br/>
<br/>

## 17_변형2

```html
<!doctype html>
<html lang="ko">

<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>17_변형2</title>
	<style>
		img {
			width: 250px;
			height: 200px;
		}

		.trans1:hover {
			transform: perspective(300px) translate3d(10px, 0px, 100px);
			-webkit-transform: perspective(300px) translate3d(10px, 0px, 100px);
			-moz-transform: perspective(300px) translate3d(10px, 0px, 100px);
			-ms-transform: perspective(300px) translate3d(10px, 0px, 100px);
			-o-transform: perspective(300px) translate3d(10px, 0px, 100px);
		}

		.trans2:hover {
			transform: perspective(300px) translateZ(100px);
			-webkit-transform: perspective(300px) translateZ(100px);
			-moz-transform: perspective(300px) translateZ(100px);
			-ms-transform: perspective(300px) translateZ(100px);
			-o-transform: perspective(300px) translateZ(100px);
		}

		.trans3:hover {
			transform: perspective(300px) rotateX(30deg);
			-webkit-transform: perspective(300px) rotateX(30deg);
			-moz-transform: perspective(300px) rotateX(30deg);
			-ms-transform: perspective(300px) rotateX(30deg);
			-o-transform: perspective(300px) rotateX(30deg);
		}

		.trans4:hover {
			transform: perspective(300px) rotateY(30deg);
			-webkit-transform: perspective(300px) rotateY(30deg);
			-moz-transform: perspective(300px) rotateY(30deg);
			-ms-transform: perspective(300px) rotateY(30deg);
			-o-transform: perspective(300px) rotateY(30deg);
		}

		.trans5:hover {
			transform: perspective(300px) rotateZ(30deg);
			-webkit-transform: perspective(300px) rotateZ(30deg);
			-moz-transform: perspective(300px) rotateZ(30deg);
			-ms-transform: perspective(300px) rotateZ(30deg);
			-o-transform: perspective(300px) rotateZ(30deg);
		}

		.trans6:hover {
			transform: perspective(300px) rotate3d(2.5, 1.5, -1.5, 30deg);
		}
	</style>
</head>

<body>
	<h1>3차원 변형</h1>
	<h3>지정한 크기만큼 x, y, z이동</h3>
	<img src="sample/image/flower1.png" class="trans1">
	<hr>

	<h3>지정한 크기만큼 Z축으로 이동</h3>
	<img src="sample/image/flower2.png" class="trans2">
	<hr>

	<h3>지정한 크기만큼 x축 방향으로 회전</h3>
	<img src="sample/image/flower3.png" class="trans3">
	<hr>

	<h3>지정한 크기만큼 y축 방향으로 회전</h3>
	<img src="sample/image/flower4.png" class="trans4">
	<hr>

	<h3>지정한 크기만큼 z축 방향으로 회전</h3>
	<img src="sample/image/flower5.png" class="trans5">
	<hr>

	<h3>지정한 크기만큼 x,y,z축 방향으로 회전</h3>
	<img src="sample/image/river1.png" class="trans6">
	<hr>






</body>

</html>
```
<br/>
<br/>

## 18_변형3

```html
<!doctype html>
<html lang="ko">

<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>18_변형3</title>
	<style>
		.wrap1 {
			border: 1px solid black;
			width: 300px;
			height: 200px;
			display: inline-block;
		}

		.flower {
			width: 300px;
			height: 200px;
			border: 1px solid black;

		}

		.left-top:hover {
			transform-origin: left top;
			transform: rotateZ(25deg);
		}

		.right-top:hover {
			transform-origin: right top;
			transform: rotateZ(25deg);
		}

		.left-bottom:hover {
			transform-origin: left bottom;
			transform: rotateZ(25deg);
		}

		.right-bottom:hover {
			transform-origin: right bottom;
			transform: rotateZ(25deg);
		}

		.wrap2 {
			width: 200px;
			height: 200px;
			border: 1px black solid;
			display: inline-block;
		}

		.box1 {
			transform: perspective(500px) rotateY(45deg);
			width: 200px;
			height: 200px;
			background: red;
		}

		.box2 {
			transform: rotateX(45deg);
			width: 200px;
			height: 200px;
			background: orange;
			transform-origin: left top;
		}

		.style1 {
			transform-style: flat;
		}

		.style2 {
			transform-style: preserve-3d;
		}

		.wrap3 {
			width: 300px;
			height: 200px;
			border: 1px solid black;
			display: inline-block;
		}

		.back {
			width: 300px;
			height: 200px;
		}

		.back1:hover {
			transform: rotateY(180deg);
			backface-visibility: hidden;
		}

		.back2:hover {
			transform: rotateY(180deg);
			backface-visibility: visible;
		}
	</style>
</head>

<body>
	<h1>기타 변형 속성</h1>
	<h3>변형 기준점 설정하기</h3>
	<p>transform-origin 속성으로 변형의 기준점을 설정할 수 있다.</p>
	<div>
		<div class="wrap1">
			<img src="sample/image/flower1.png" class="flower left-top">
		</div>
		<div class="wrap1">
			<img src="sample/image/flower1.png" class="flower right-top">
		</div>
		<div class="wrap1">
			<img src="sample/image/flower1.png" class="flower left-bottom">
		</div>
		<div class="wrap1">
			<img src="sample/image/flower1.png" class="flower right-bottom">
		</div>

	</div>

	<hr>

	<h3>3d변형 적용하기</h3>
	<div class="wrap2">
		<div class="box1 style1">
			<div class="box2"></div>
		</div>
	</div>
	<div class="wrap2">
		<div class="box1 style2">
			<div class="box2"></div>
		</div>
	</div>

	<hr>

	<h3>사진 뒷면 보이게 하기</h3>
	<div>
		<div class="wrap3">
			<img src="sample/image/flower2.png" class="back back1">
		</div>
		<div class="wrap3">
			<img src="sample/image/flower2.png" class="back back2">
		</div>
	</div>


</body>

</html>
```
<br/>
<br/>

# 웹문서 구조 만들기

##   01_기본 구조 

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>01_기본 구조</title>

    <style>
      /*요소의 영역 4종류 : content, padding, border, margin */
      
      /*실습을 위해 div 테두리 표시*/
      div{border : 10px solid red;}

      .test1{
          /* width, height : 기본적으로 content 영역의 크기를 지정하는 속성 */
          width: 300px;
          height:200px;

          border : 20px solid blue;
          padding : 20px;
          margin : 20px;
      }

      .test2{

          width : 300px;
          height : 200px;
          /*width: 260px;
          height : 160px;*/

          box-sizing : border-box;
          /* !!중요함!!!
           box-sizing : width, height 속성의 범위를 지정하는 속성

           기본값 -> box-sizing:content-box;
              --> width, height로 지정한 값은 content 영역의 크기
           
              box-sizing:border-box;
              --> width, height로 지정한 값은 
              content + padding + border의 합과 같은 크기로  자동 지정
              */
          border : 20px solid yellow;
      }

      /* div 상하 2등분 */
      #wrapper{
          width:600px;
          height:500px;
      }

      #div-1{
          background-color: yellow;
          border : 10px solid blue;

          /* 가변 크기 == 부모 요소 content 크기에 비례 */
          width: 100%;
          height : 50%;

            box-sizing : border-box;
            /* width/height의 속성값 ==  content + padding + border 값과 같게 만듦 */
      }

      #div-2{
          background-color: green;
          border : 10px solid pink;

           /* 가변 크기 == 부모 요소 content 크기에 비례 */
           width: 100%;
          height : 50%;

          box-sizing : border-box;
            /* width/height의 속성값 ==  content + padding + border 값과 같게 만듦 */

      }


    </style>
</head>
<body>
    <div class="test1">
        content : 요소의 내용이 표시되는 영역<br>
        padding : content와 border 사이 영역 <br>
        border : 요소의 테두리 영역<br>
        margin : 타 요소와의 간격을 지정하는 영역<br>
    </div>

    <div class="test1">
        <div class="test2"></div>
        <!-- test1의 하얀 부분의 폭 : 340px( content + padding) 
             test1의 하얀 부분의 높이 : 240px( content + padding) 
            
            하위 태그는 상위 태그의 content 영역에 추가된다.-->
    </div>

    <hr>

    <h2>div 상하 2등분</h2>

    <!-- 1.div 전체를 감싸고 있는 div 태그 -->
    <div id="wrapper">

        <!-- 2.id가 wrapper인 요소를 상하 2등분 하기 위한 div 태그 2개 -->
        <div id="div-1"></div>
        <div id="div-2"></div>



    </div>


    <!-- 
       1. HTML 요소의 영역 종류와 의미 파악
        (content, padding , border, margin)

       2. 상위 요소 내에 작성된 하위 요소의 위치
       ->하위 요소는 상위 요소의 content 영역에 표시된다.

       3. 가변 크기
       -> 상위 요소 크기에 비례

       4. box-sizing 속성
       width/height 속성의 범위를 지정하는 속성
       ->width/height 값이 어떤 영역 까지의 합을 나타내는지 설정

        content-box : content 영역 까지
        ex) box-sizing:content-box;
            width : 100px;  -> content 폭이 100px이다.
        
        border-box : content+padding+border 영역 까지
        ex) box-sizing:border-box;
            width : 100px; -> content+padding 양 쪽+border 양 쪽  영역 전체가 100px 이다.

        
       
     -->
</body>
</html>
```
<br/>
<br/>