---
title: "2020년 11월 30일"
excerpt: "CSS-2"
search: true
categories: 
  - Academy
tags: 
  - HTML
  - CSS
toc: true
---

## 08_문단 관련 스타일

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>08_문단 관련 스타일</title>

    <style>
        /*글자 작성 방향*/
        #p1{ direction : ltr } /*왼쪽 -> 오른쪽 (기본값)*/
        #p2{ direction : rtl } /*오른쪽 -> 왼쪽 (기본값)*/

        /*텍스트 정렬*/
        /* text-align : 글자와 내부에 작성된 inline 형식의 정렬방법을 지정 */

        #p3{ text-align : left; } /*왼쪽 정렬(기본값)*/
        #p4{ text-align : right; } /*오른쪽 정렬*/
        #p5{ text-align : center; } /*가운데 정렬*/
        #p6{text-align : justify;} /*양쪽 정렬*/

        /*문장 간격(장평)*/
        #p7{line-height:normal;} /*브라우저에 지정된 기본값*/
        #p8{line-height:30px;} /*고정 크기 단위 px*/
        #p9{line-height:3em;} /*가변 크기 단위 em*/
        #p10{line-height:500%;} /*가변 크기 단위 %*/
        /*가변 크기는 부모 요소의 크기에 비례해서 지정됨.
            1em == 100% == 부모 요소 크기 
            30px    30px   자식 */

    </style>
</head>
<body>
    <h3>텍스트 글자 작성 방향 지정</h3>
    
    <p id="p1">
        동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세 
        무궁화 삼천리 화려 강산 대한 사람 대한으로 길이 보전하세.
    </p>
    <p id="p2">
        동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세 
        무궁화 삼천리 화려 강산 대한 사람 대한으로 길이 보전하세.
    </p>

    <hr>

    <h3> 텍스트 정렬 </h3>
    왼쪽, 오른쪽, 가운에, 양쪽

    <p id="p3">
        동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세 
        무궁화 삼천리 화려 강산 대한 사람 대한으로 길이 보전하세.
    </p>
    <p id="p4">
        동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세 
        무궁화 삼천리 화려 강산 대한 사람 대한으로 길이 보전하세.
    </p>
    <p id="p5">
        동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세 
        무궁화 삼천리 화려 강산 대한 사람 대한으로 길이 보전하세.
    </p>
    <p id="p6">
        동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세 
        무궁화 삼천리 화려 강산 대한 사람 대한으로 길이 보전하세.
    </p>

    <hr>

    <h3>줄 간격(장평) 조절</h3>
    <p id="p7"><!--기본 간격-->
        동해물과 백두산이 마르고 닳도록 <br>
        하느님이 보우하사 우리나라 만세 <br>
        무궁화 삼천리 화려 강산 <br>
        대한 사람 대한으로 길이 보전하세.
    </p>
    <p id="p8">
        동해물과 백두산이 마르고 닳도록 <br>
        하느님이 보우하사 우리나라 만세 <br>
        무궁화 삼천리 화려 강산 <br>
        대한 사람 대한으로 길이 보전하세.
    </p>
    <p id="p9">
        동해물과 백두산이 마르고 닳도록 <br>
        하느님이 보우하사 우리나라 만세 <br>
        무궁화 삼천리 화려 강산 <br>
        대한 사람 대한으로 길이 보전하세.
    </p>
    <p id="p10">
        동해물과 백두산이 마르고 닳도록 <br>
        하느님이 보우하사 우리나라 만세 <br>
        무궁화 삼천리 화려 강산 <br>
        대한 사람 대한으로 길이 보전하세.
    </p>
 



</body>
</html>
```
<br/>


<br/>

## 09_목록 관련 스타일

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>09_목록 관련 스타일</title>
    <style>

        /*순서 없는 목록<ul>*/

        #default-bullet{
            list-style-type: disc;/*채워진 원 모양(기본값)*/
        }

        #circle-bullet{
            list-style-type: circle;/*비어있는 원 모양*/
        }

        #square-bullet{
            list-style-type: square; /*채워진 사각형 모양
                                     (빈 사각형 모양은 없음)*/
        }

        #none-bullet{
            list-style-type:none; /*불릿 없앰*/
        }



        /* 순서 있는 목록(ol) */

		/* 1로 시작하는 10진수(기본값) */
		#default-number {
			list-style-type: decimal;
		}

		/* 앞에 0이 붙는 10진수 */
		#zero-number {
			list-style-type: decimal-leading-zero;
		}

		/* 소문자 로마 숫자 */
		#lower-rome-number {
			list-style-type: lower-roman;
		}

		/* 대문자 로마 숫자 */
		#upper-rome-number {
			list-style-type: upper-roman;
		}

		/* 알파벳 소문자 */
		#lower-alpha {
			list-style-type: lower-alpha;
		}

		/* 알파벳 대문자 */
		#upper-alpha {
			list-style-type: upper-alpha;
		}


        /*기호 대신 이미지 사용하기*/
        #image-bullet{
            list-style-image: url("pi.png");
        }


    </style>

</head>
<body>
    <h3>순서 없는 목록</h3>
    <ul id="default-bullet">
        <li>HTML5</li>
        <li>CSS3</li>
        <li>Javascript</li>
        <li>jQuery</li>
    </ul>

    <ul id="circle-bullet">
        <li>HTML5</li>
        <li>CSS3</li>
        <li>Javascript</li>
        <li>jQuery</li>
    </ul>

    <ul id="square-bullet">
        <li>HTML5</li>
        <li>CSS3</li>
        <li>Javascript</li>
        <li>jQuery</li>
    </ul>
 
    <ul id="none-bullet">
        <li>HTML5</li>
        <li>CSS3</li>
        <li>Javascript</li>
        <li>jQuery</li>
    </ul>

    <h3>순서가 있는 목록</h3>

    <h4>1로 시작하는 10진수(기본값)</h4>
	<ol id="default-number">
		<li>HTML5</li>
		<li>CSS3</li>
		<li>JavaScript</li>
		<li>JQuery</li>
	</ol>
	<h4>앞에 0이 붙는 10진수</h4>
	<ol id="zero-number">
		<li>HTML5</li>
		<li>CSS3</li>
		<li>JavaScript</li>
		<li>JQuery</li>
	</ol>
	<h4>소문자 로마 숫자</h4>
	<ol id="lower-rome-number">
		<li>HTML5</li>
		<li>CSS3</li>
		<li>JavaScript</li>
		<li>JQuery</li>
	</ol>
	<h4>대문자 로마 숫자</h4>
	<ol id="upper-rome-number">
		<li>HTML5</li>
		<li>CSS3</li>
		<li>JavaScript</li>
		<li>JQuery</li>
	</ol>
	<h4>알파벳 소문자</h4>
	<ol id="lower-alpha">
		<li>HTML5</li>
		<li>CSS3</li>
		<li>JavaScript</li>
		<li>JQuery</li>
	</ol>
	<h4>알파벳 대문자</h4>
	<ol id="upper-alpha">
		<li>HTML5</li>
		<li>CSS3</li>
		<li>JavaScript</li>
		<li>JQuery</li>
    </ol>
    
    <hr>
    <h3>기호 대신 이미지 사용하기</h3>
    <ul id="image-bullet">
        <li>HTML5</li>
		<li>CSS3</li>
		<li>JavaScript</li>
		<li>JQuery</li>
    </ul>




</body>
</html>
```
<br/>
<br/>

## 10_배경 스타일

```html
<!doctype html>
<html lang="ko">

<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>10_배경 스타일</title>
	<style>
		body {
			/* background-color : aqua;
			background-color : rgb(50, 150, 250);
			background-color : rgba(50, 150, 250, 0.7);
			background-color : hsl(180, 100%, 70%);
			background-color : hsla(240, 100%, 50%, 0.5); */
			background-color: #10aaff;
			background-image: url("gg.png");
			background-repeat: no-repeat; /*repeat 하면 배경 이미지가 계속 반복이 된다.*/
			background-size: 100%;
			width: 100%;
			height: 100%;

			/* 배경 첨부 속성*/
			background-attachment: fixed; /*배경 이미지 고정*/

		}

		#div-bg {
			width: 50%;
			background-color: white;
		}

		.div-test {
			background-color: white;
			margin: 30px;
			padding: 20px;
			width: 20%;
			border: 5px dashed black;
			float: left;
		}

		#div1 {
			/* 배경이 테두리까지 표시됨 */
			background-clip: border-box;
		}

		#div2 {

			background-clip: padding-box;
		}

		#div3 {
			background-clip: content-box;
		}

		#bg-img {
			width: 70%;
			height: 800px;
			border: 1px solid black;
			background-image: url('pi.PNG');

			/* 반복 */
			/* background-repeat: repeat; */
			/* background-repeat : repeat-x; */
			/*background-repeat : repeat-y;*/
			background-repeat: no-repeat;

			/* 크기 */
			/* 원래 크기*/
			/*background-size : auto; */

			/* 요소 안에 배경 이미지가 다 들어오도록 이미지를 확대/축소*/
			/*background-size : contain;*/

			/* 배경 이미지로 요소를 모두 덮을 수 있도록 이미지를 확대/축소*/
			/*background-size : cover;*/

			/* 배경 이미지 크기 지정 */
			/*background-size : 150px 100px;*/
			background-size: 80% 80%;

			/* 배경 이미지 위치 지정 */
			/*background-position:left top;*/
			/*background-position:right top;*/
			/*background-position:center top;*/
			/*background-position:left bottom;*/
			/*background-position:right bottom;*/
			/*background-position:center bottom;*/
			/*background-position:left center;*/
			/*background-position:right center;*/
			/*background-position:center center;*/
			/*background-position:30px 50px;*/
			background-position: 50% 50%;
			
			/* 배경 첨부 속성 */

			/* 페이지와 함께 배경도 스크롤 (기본값) */
			/*background-attachment:scroll;*/ 

			/* 페이지 스크롤과 상관없이 배경 고정. */
			background-attachment: fixed;
		}
	</style>
</head>

<body>
	<h1>배경색과 배경 이미지</h1>
	<pre> body { background-color : 색상명 | rgb | rgba | hsl | hsla | 16진수 }; 로 배경색을 지정할 수 있다.</pre>
	<a href="http://www.w3schools.com/colors/colors_names.asp" target="_blank">색상명 참조 사이트로 이동</a><br>
	<a href="http://www.colorpicker.com" target="_blank">색상 추출 사이트로 이동</a>

	<hr>
	<h3>div배경 테스트</h3>
	<pre>div영역을 설정하는 경우 배경색을 지정하기 위해서는 div에 배경 색을 별도로 지정해야한다.</pre>
	<div id="div-bg">싹이 아니한 수 구하지 그러므로 청춘이 있다. 얼음에 봄바람을 동력은 그들은 무엇을 것은 그들의 듣는다. 인간에 황금시대를 청춘이 하는 사막이다. 위하여, 이상이 그림자는 무엇이
		남는 뜨거운지라, 사랑의 칼이다. 수 청춘을 행복스럽고 되려니와, 때문이다. 있을 미인을 할지니, 원대하고, 그들의 튼튼하며, 듣는다. 끝까지 동산에는 봄날의 풍부하게 약동하다. 이성은 피부가
		속잎나고, 인도하겠다는 튼튼하며, 무한한 들어 말이다. 심장의 위하여, 끓는 것은 예수는 주는 보배를 가치를 철환하였는가?</div>

	<h3>배경 적용 범위 조정</h3>
	<pre>div영역의 배경 적용 범위를 지정할 때 background-clip 속성을 사용한다.</pre>
	<div class="div-test" id="div1">굳세게 가는 수 주며, 같은 거친 것이다. 그것을 청춘의 뭇 트고, 끝까지 이상 청춘은 교향악이다. 인도하겠다는 이는 이상은 그리하였는가? 하는 이성은
		피고 봄바람이다. 하여도 능히 밥을 더운지라 그들에게 싸인 가진 같으며, 가치를 뿐이다. 것이 지혜는 모래뿐일 튼튼하며, 눈에 것은 품으며, 것이다. 싹이 사는가 같은 피어나기 없으면, 평화스러운
		구하지 것이다. 그들은 얼음이 우리의 가장 같지 쓸쓸하랴? 있을 얼음 오아이스도 소담스러운 예수는 몸이 힘있다. 생명을 따뜻한 얼마나 이상 온갖 인생의 청춘은 무한한 실로 봄바람이다. 속잎나고, 남는
		모래뿐일 착목한는 것이다.</div>
	<div class="div-test" id="div2">굳세게 가는 수 주며, 같은 거친 것이다. 그것을 청춘의 뭇 트고, 끝까지 이상 청춘은 교향악이다. 인도하겠다는 이는 이상은 그리하였는가? 하는 이성은
		피고 봄바람이다. 하여도 능히 밥을 더운지라 그들에게 싸인 가진 같으며, 가치를 뿐이다. 것이 지혜는 모래뿐일 튼튼하며, 눈에 것은 품으며, 것이다. 싹이 사는가 같은 피어나기 없으면, 평화스러운
		구하지 것이다. 그들은 얼음이 우리의 가장 같지 쓸쓸하랴? 있을 얼음 오아이스도 소담스러운 예수는 몸이 힘있다. 생명을 따뜻한 얼마나 이상 온갖 인생의 청춘은 무한한 실로 봄바람이다. 속잎나고, 남는
		모래뿐일 착목한는 것이다.</div>
	<div class="div-test" id="div3">굳세게 가는 수 주며, 같은 거친 것이다. 그것을 청춘의 뭇 트고, 끝까지 이상 청춘은 교향악이다. 인도하겠다는 이는 이상은 그리하였는가? 하는 이성은
		피고 봄바람이다. 하여도 능히 밥을 더운지라 그들에게 싸인 가진 같으며, 가치를 뿐이다. 것이 지혜는 모래뿐일 튼튼하며, 눈에 것은 품으며, 것이다. 싹이 사는가 같은 피어나기 없으면, 평화스러운
		구하지 것이다. 그들은 얼음이 우리의 가장 같지 쓸쓸하랴? 있을 얼음 오아이스도 소담스러운 예수는 몸이 힘있다. 생명을 따뜻한 얼마나 이상 온갖 인생의 청춘은 무한한 실로 봄바람이다. 속잎나고, 남는
		모래뿐일 착목한는 것이다.</div>

	<br clear="both">
	<hr>

	<h3>배경 이미지 테스트</h3>
	<pre> background-image 속성을 이용하여 배경 사진을 넣을 수 있다. 사진 크기에 따라 자동 반복하여 이미지가 나타난다.</pre>
	<pre>배경 이미지의 반복 방법을 지정하기 위해 background-repeat 속성을 이용할 수 있다.</pre>
	<pre>배경 이미지가 요소보다 크거나 작은 경우 배경 이미지의 크기를 background-size속성으로 변경할 수 있다.</pre>
	<div id="bg-img"></div>
</body>

</html>
```




<br/><br/>

## 11_배경스타일2

```html
<!doctype html>
<html lang="ko">
	<head>
		<meta charset="UTF-8">
		<title>11_배경스타일2</title>
		<style>
			.linear-gradient {
				width: 300px;
				height: 300px;
				border: 1px solid black;
			}

			/* CSS3는 W3C에서 지정한 웹 표준이기 때문에
				거의 모든 브라우저에서 동일하게 해석됨.
				
				간혹 안되는 경우 (대표적으로 IE 8 이하 버전)

				이런 경우를 대비해서 해석할 수 있는 키워드를 지정
				-ms : IE,Edge 브라우저
				-webkit- : chrome
				-o- : Opera
				-moz- : firefox

				*/

			#gradient1 {
				background: linear-gradient(to bottom, blue, white);
				background: -webkit-linear-gradient(top, blue, white);
				background: -moz-linear-gradient(top, blue, white);
				background: -ms-linear-gradient(top, blue, white);
				background: -o-linear-gradient(top, blue, white);

			}

			#gradient2 {
				background: linear-gradient(45deg, blue, white);
				background: -webkit-linear-gradient(45deg, blue, white);
				background: -moz-linear-gradient(45deg, blue, white);
				background: -ms-linear-gradient(45deg, blue, white);
				background: -o-linear-gradient(45deg, blue, white);

			}

			#gradient3 {
				background: linear-gradient(to right, blue, white 50%, red);
				background: -webkit-linear-gradient(left, blue, white 50%, red);
				background: -moz-linear-gradient(left, blue, white 50%, red);
				background: -ms-linear-gradient(left, blue, white 50%, red);
				background: -o-linear-gradient(left, blue, white 50%, red);
			}

			#gradient4 {
				background: linear-gradient(red, orange 15%, yellow 30%, green 45%, blue 60%, navy 75%, purple 90%);
				background: -webkit-linear-gradient(red, orange 15%, yellow 30%, green 45%, blue 60%, navy 75%, purple 90%);
				background: -moz-linear-gradient(red, orange 15%, yellow 30%, green 45%, blue 60%, navy 75%, purple 90%);
				background: -ms-linear-gradient(red, orange 15%, yellow 30%, green 45%, blue 60%, navy 75%, purple 90%);
				background: -o-linear-gradient(red, orange 15%, yellow 30%, green 45%, blue 60%, navy 75%, purple 90%);
			}

			#gradient5 {
				background: repeating-linear-gradient(blue, red 30%);
				background: -webkit-repeating-linear-gradient(blue, red 30%);
				background: -moz-repeating-linear-gradient(blue, red 30%);
				background: -ms-repeating-linear-gradient(blue, red 30%);
				background: -o-repeating-linear-gradient(blue, red 30%);
			}

			#gradient6 {
				background: repeating-linear-gradient(red, red 5%, orange 5%, orange 10%, yellow 10%, yellow 15%, green 15%, green 20%, blue 20%, blue 25%, navy 25%, navy 30%, purple 30%, purple 35%);
				background: -webkit-repeating-linear-gradient(red, red 5%, orange 5%, orange 10%, yellow 10%, yellow 15%, green 15%, green 20%, blue 20%, blue 25%, navy 25%, navy 30%, purple 30%, purple 35%);
				background: -moz-repeating-linear-gradient(red, red 5%, orange 5%, orange 10%, yellow 10%, yellow 15%, green 15%, green 20%, blue 20%, blue 25%, navy 25%, navy 30%, purple 30%, purple 35%);
				background: -ms-repeating-linear-gradient(red, red 5%, orange 5%, orange 10%, yellow 10%, yellow 15%, green 15%, green 20%, blue 20%, blue 25%, navy 25%, navy 30%, purple 30%, purple 35%);
				background: -o-repeating-linear-gradient(red, red 5%, orange 5%, orange 10%, yellow 10%, yellow 15%, green 15%, green 20%, blue 20%, blue 25%, navy 25%, navy 30%, purple 30%, purple 35%);
			}

			.circle-gradient {
				width: 500px;
				height: 300px;
				border: 1px solid black;
			}

			#gradient7 {
				background: radial-gradient(white, blue, red);
				background: -webkit-radial-gradient(white, blue, red);
				background: -moz-radial-gradient(white, blue, red);
				background: -o-radial-gradient(white, blue, red);
			}

			#gradient8 {
				background: radial-gradient(circle, white, blue, red);
				background: -webkit-radial-gradient(circle, white, blue, red);
				background: -moz-radial-gradient(circle, white, blue, red);
				background: -ms-radial-gradient(circle, white, blue, red);
				background: -o-radial-gradient(circle, white, blue, red);
			}

			#gradient9 {
				background: radial-gradient(90% 90%, circle, white, blue, red);
				background: -webkit-radial-gradient(90% 90%, circle, white, blue, red);
				background: -moz-radial-gradient(90% 90%, circle, white, blue, red);
				background: -ms-radial-gradient(90% 90%, circle, white, blue, red);
				background: -o-radial-gradient(90% 90%, circle, white, blue, red);
			}

			#gradient10 {
				background: radial-gradient(30% 40%, circle closest-side, white, blue, red);
				background: -webkit-radial-gradient(30% 40%, circle closest-side, white, blue, red);
				background: -moz-radial-gradient(30% 40%, circle closest-side, white, blue, red);
				background: -ms-radial-gradient(30% 40%, circle closest-side, white, blue, red);
				background: -o-radial-gradient(30% 40%, circle closest-side, white, blue, red);
			}

			#gradient11 {
				background: radial-gradient(30% 40%, circle closest-corner, white, blue, red);
				background: -webkit-radial-gradient(30% 40%, circle closest-corner, white, blue, red);
				background: -moz-radial-gradient(30% 40%, circle closest-corner, white, blue, red);
				background: -ms-radial-gradient(30% 40%, circle closest-corner, white, blue, red);
				background: -o-radial-gradient(30% 40%, circle closest-corner, white, blue, red);
			}

			#gradient12 {
				background: radial-gradient(30% 40%, circle farthest-side, white, blue, red);
				background: -webkit-radial-gradient(30% 40%, circle farthest-side, white, blue, red);
				background: -moz-radial-gradient(30% 40%, circle farthest-side, white, blue, red);
				background: -ms-radial-gradient(30% 40%, circle farthest-side, white, blue, red);
				background: -o-radial-gradient(30% 40%, circle farthest-side, white, blue, red);
			}

			#gradient13 {
				background: radial-gradient(30% 40%, circle farthest-corner, white, blue, red);
				background: -webkit-radial-gradient(30% 40%, circle farthest-corner, white, blue, red);
				background: -moz-radial-gradient(30% 40%, circle farthest-corner, white, blue, red);
				background: -ms-radial-gradient(30% 40%, circle farthest-corner, white, blue, red);
				background: -o-radial-gradient(30% 40%, circle farthest-corner, white, blue, red);
			}

			#gradient14 {
				background: repeating-radial-gradient(white, blue, red 10%);
				background: -webkit-repeating-radial-gradient(white, blue, red 10%);
				background: -moz-repeating-radial-gradient(white, blue, red 10%);
				background: -ms-repeating-radial-gradient(white, blue, red 10%);
				background: -o-repeating-radial-gradient(white, blue, red 10%);
			}

			#gradient15 {
				background: repeating-radial-gradient(white, white 10%, blue 10%, blue 20%, red 20%, red 30%);
				background: -webkit-repeating-radial-gradient(white, white 10%, blue 10%, blue 20%, red 20%, red 30%);
				background: -moz-repeating-radial-gradient(white, white 10%, blue 10%, blue 20%, red 20%, red 30%);
				background: -ms-repeating-radial-gradient(white, white 10%, blue 10%, blue 20%, red 20%, red 30%);
				background: -o-repeating-radial-gradient(white, white 10%, blue 10%, blue 20%, red 20%, red 30%);

			}
		</style>
	</head>

	<body>
		<h1>그라데이션</h2>
			<p>웹 표준으로 지정되었지만 이전 버전과 호환을 위해 브라우저별 접두사를 이용한다.</p>

			<h3>선형그라데이션</h3>
			<p>색상이 수직, 수평, 대각선 방향으로 일정하게 변하는 것</p>
			<h4>색상만 지정</h4>
			<div id="gradient1" class="linear-gradient"></div>
			<h4>방향을 지정</h4>
			<div id="gradient2" class="linear-gradient"></div>
			<h4>중간색 지정</h4>
			<div id="gradient3" class="linear-gradient"></div>
			<h4>중간색 여러개 지정</h4>
			<div id="gradient4" class="linear-gradient"></div>
			<h4>그라데이션 반복</h4>
			<div id="gradient5" class="linear-gradient"></div>
			<h4>그라데이션 반복(색 구분 명확하게)</h4>
			<div id="gradient6" class="linear-gradient"></div>
			<hr>

			<h3>원형 그라데이션</h3>
			<p>색상이 원이나 타원의 중심부터 동심원을 그리며 바깥 방향으로 색상이 변경되는 것</p>
			<h4>색상만 지정</h4>
			<div id="gradient7" class="circle-gradient"></div>
			<h4>모양 지정</h4>
			<div id="gradient8" class="circle-gradient"></div>
			<h4>시작점 지정</h4>
			<div id="gradient9" class="circle-gradient"></div>
			<h4>사이즈 지정</h4>
			<div id="gradient10" class="circle-gradient"></div>
			<div id="gradient11" class="circle-gradient"></div>
			<div id="gradient12" class="circle-gradient"></div>
			<div id="gradient13" class="circle-gradient"></div>
			<h4>원형그라데이션 반복</h4>ㅈ
			<div id="gradient14" class="circle-gradient"></div>
			<h4>원형그라데이션 반복(색 구분 명확하게)</h4>
			<div id="gradient15" class="circle-gradient"></div>

	</body>

</html>
```



<br/><br/>

## 12_레이아웃 스타일1

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>12_레이아웃 스타일1</title>

    <style>
        .size-test{
            border : 1px solid red;
            /*테두리 : 1px 두께, 실선, 빨간색*/
        }

        #test1{
            /*고정 크기*/
            /*해당 요소의 영역 크기가 고정으로 할당되어
            창 크기가 변경되어도 요소의 크기가 변화하지 않음.*/
            width : 600px;
            height : 200px;
        }

        #test2{
            /*가변 크기*/
            /*부모 요소 또는 창 크기에 따라
            해당 요소의 크기가 변화함*/
            width:50%;
            height : 200px;
        }

        /*부모 요소 고정 크기*/
        #test3-out {
            width : 600px;
            height : 200px;
        }

        /*자식 요소 가변 크기*/
        #test3-in{
            width:50%;  /* 50% == 부모 요소 폭의 50% == 300px*/
            height:100%; /* 100% == 부모 요소 0000000000000000000높이의 100% == 200px*/
            border:1px solid blue;
        }

        /*화면 배치 방법 변경*/
        .block{
            width: 150px;
            height : 150px;
            border : 1px solid black;
            color:white;
        }
        
        .area1{background-color: midnightblue;}
        .area2{background-color: slateblue;}
        .area3{background-color: plum;}
        .area4{background-color: lightpink;}
        .area5{background-color: lightcoral;}


        /* block -> inline*/
        .block-test1{display : inline;}

        /*inline -> block*/
        .block-test2{display : block;}
        

        /*inline-block 형식*/
        .block-test3{display : inline-block;}
    </style>

</head>
<body>
    <h3>width / height</h3>
    <p> 내용(content)이 차지하는 영역의 크기를 조절할 수 있는 속성</p>

    <h4>고정 크기</h4>
    <div id="test1" class="size-test"> </div>
    <h4>가변 크기</h4>
    <div id="test2" class="size-test"> </div>

    <h4>고정 크기 부모 요소 - 가변 크기 자식 요소</h4>

    <div id="test3-out" class="size-test">
        <div id="test3-in" class="size-test"></div>
    </div>
    <!-- 부모 요소의 크기가 고정되어 있기 때문에
        자식 요소의 크기가 가변 크기로 지정되었어도
        창 크기의 변화로는 영향을 받지 않는다.-->

    <hr>

    <h1>화면 배치 방법 변경</h1>
    <p>
        block형식 요소 (div, p, pre 등) <br>
        inline형식 요소(span, img, iframe 등)<br>
        위 형식을 변경하여 배치 방식을 변경할 수 있음.
    </p>

    <h3>block형식 -> inline 형식으로 변경</h3>
    
    <div class="block block-test1 area1"> 첫 번째 영역 </div>
    <div class="block block-test1 area2"> 두 번째 영역 </div>
    <div class="block block-test1 area3"> 세 번째 영역 </div>
    <div class="block block-test1 area4"> 네 번째 영역 </div>
    <div class="block block-test1 area5"> 다섯 번째 영역 </div>

    <pre>
        block과 inline의 차이점
        1. block은 수직 분할, inline은 수평 분할
        2. block은 width, height 값을 지정할 수 있으나
            inline은 width, height 값을 지정할 수 없다.
    </pre>


    <h3>inline 형식 -> block 형식</h3>
    <span class="block block-test2 area1">첫 번째 영역</span>
    <span class="block block-test2 area2">두 번째 영역</span>
    <span class="block block-test2 area3">세 번째 영역</span>
    <span class="block block-test2 area4">네 번째 영역</span>
    <span class="block block-test2 area5">다섯 번째 영역</span>


    <h3>inline-block형식</h3>
    <p>
        영역 분할 방법은 inline형식의 방법인 수평 분할을 하지만,
        block형식의 특징인 width, height 속성을 지정할 수 있음.
    </p>
    <span class="block block-test3 area1">첫 번째 영역</span>
    <span class="block block-test3 area2">두 번째 영역</span>
    <span class="block block-test3 area3">세 번째 영역</span>
    <span class="block block-test3 area4">네 번째 영역</span>
    <span class="block block-test3 area5">다섯 번째 영역</span>


</body>
</html>
```


<br/><br/>

## 13_레이아웃 스타일2

```html
<!doctype html>
<html lang="ko">

<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>13_레이아웃 스타일2</title>
	<style>
		div {
			width: 100px;
			height: 100px;

			/* 테두리 굵기 */
			border-width: 10px;

		}

		/* 테두리 없음 */
		.border-test1 {
			border-style: none;
		}

		/* 테두리는 있지만 보이지 않음 */
		.border-test2 {
			border-style: hidden;

			/* 영역 그림자 */
			box-shadow: 5px 5px 3px 2px gray;
			/* x축 y축 번짐크기 그림자 크기 색*/
		}

		.border-test3 {
			border-style: dotted;
		}

		.border-test4 {
			border-style: dashed;

			/* 테두리 방향별 색깔 */
			border-top-color: blue;
			border-bottom-color: red;
			border-left-color: green;
			border-right-color: yellow;
		}

		.border-test5 {
			border-style: double;
			border-color: blue;
		}

		.border-test6 {
			border-style: groove;

			/* 테두리 방향별 곡선 지정 */
			/* 각 꼭지점을 기준으로 각 변의 얼만큼의 크기를 곡선으로 만들지 지정 */
			border-top-left-radius: 50px;
		}

		.border-test7 {
			border-style: inset;
			border-bottom-right-radius: 50px;
		}

		.border-test8 {
			border-style: outset;
			border-top-left-radius: 50px;
			border-bottom-right-radius: 50px;
		}

		.border-test9 {
			border-style: ridge;
			box-shadow: 5px 5px 3px 2px gray;
		}

		.border-test10 {
			border-style: solid;
			box-shadow: 5px 5px 3px 2px gray;
			border-radius:60px; /*모든 테두리가 곡선으로 바뀐다.*/
		}
	</style>
</head>

<body>
	<h1>테두리 스타일</h1>
	<h3>border-style:none</h3>
	<div class="border-test1"></div>

	<h3>border-style:hidden & box-shadow:5px 5px 3px 2px gray</h3>
	<div class="border-test2"></div>

	<h3>border-style:dotted</h3>
	<div class="border-test3"></div>

	<h3>border-style:dashed & border-top-color:blue</h3>
	<div class="border-test4"></div>

	<h3>border-style:double & border-color:blue</h3>
	<div class="border-test5"></div>

	<h3>border-style:groove & border-top-left-radius:50px</h3>
	<div class="border-test6"></div>

	<h3>border-style:inset & border-bottom-right-radius:50px</h3>
	<div class="border-test7"></div>

	<h3>border-style:outset & border-bottom-right-radius:50px & border-bottom-right-radius:50px</h3>
	<div class="border-test8"></div>

	<h3>border-style:ridge & box-shadow:5px 5px 3px 2px gray</h3>
	<div class="border-test9"></div>

	<h3>border-style:solid & box-shadow:5px 5px 3px 2px gray</h3>
	<div class="border-test10"></div>

</body>

</html>
```
<br/><br/>

## 14_레이아웃 스타일3

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>14_레이아웃 스타일3</title>
    <style>
        .test{
            /* width/height는 기본적으로 content의 크기를 지정하는 속성 */
            width : 100px;
            height:100px;

            background-color: yellowgreen;
        }

        /* padding 영역 */
        .padding-test1{
            padding : 100px;
        }

        .padding-test2{
            padding-top : 50px;
            padding-bottom : 100px;
            padding-left : 150px;
            padding-right : 200px;

        }

        /*border 영역*/
        .border-test1{
            /* border-width: 10px; 
            border-style : solid;
            border-color:red;*/

            border : 10px solid red;
            /*보더 한번에 작성하기*/
        }

        .ib{
            display:inline-block;
        }

        /*margin 영역*/
        .margin-test1{
            margin : 100px;
        }

        .margin-test2{
            margin-top :50px;
            margin-left : 120px;
        }

        .margin-test3{
            margin : auto;
            /* 브라우저가 자동으로 요소간의 간격을 계산해서 가로 가운데를 맞춰줌 */
        }

    </style>

</head>
<body>

    <h1>여백 관련 속성</h1>
    <h3>기준 div태그</h3>
    <div class="test">content</div>

    <h3>padding 영역</h3>
    <p>
        content와 테두리(border) 사이의 영역
    </p>

    <pre>padding : 100px;</pre>
    <div class="test padding-test1">content</div>

    <pre> padding-top | bottom | left | right </pre>
    <div class="test padding-test2">content</div>

    <hr>
    <h3>border 영역</h3>
    <p> 요소의 가장자리 영역 </p>
    <div class="test border-test1">content</div>

    <h4>content + padding + border</h4>
    <div class="test padding-test1 border-test1">content</div>

    <hr>
    <h3>margin 영역</h3>
    <p>
        요소들 간의 간격을 조절하는 영역
    </p>
    <div class="test border-test1 ib margin-test1">content</div><div class="test border-test1 ib margin-test1">content</div>
    <div class="test border-test1 ib margin-test1">content</div>
    
    <hr>
    
    <div class="test border-test1 ib margin-test2">content</div><div class="test border-test1 ib margin-test2">content</div>
    <div class="test border-test1">content</div>

    <hr>
    <h4>margin : auto</h4>
    <div class="test border-test1 margin-test3">content</div>
</body>
</html>
```


