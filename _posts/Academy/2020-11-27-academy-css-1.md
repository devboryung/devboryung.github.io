---
title: "2020년 11월 27일"
excerpt: "CSS-1"
search: true
categories: 
  - Academy
tags: 
  - HTML
  - CSS
toc: true
---

## 01_CSS 선택자 1

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>01_CSS 선택자 1</title>

    <Style>
        /*Style태그는 head 영역 안에 작성한다.*/
        /*CSS 주석 : 범위 주석 모양으로만 작성 가능하다.*/
        /*Style 태그는 CSS를 작성하기 위한 영역으로 
            HTML태그에서 작성하는 방법과 다른 형태의 주석을 사용한다.*/
        
        /*모든 요소 선택자  *{color: red;} */
            * {  /* HTML 문서 내에 있는 모든 요소의 글자색을 변경한다.*/
            color : rgb(62, 85, 160);
        }

        /*태그 선택자*/
        p { color : rgba(43, 87, 34, 0.89);}

        /*아이디 선택자*/
        #id2 { color :rgb(228, 194, 0);}
        #id3 { color :rgba(255, 255, 255, 0.932);}
        #id2,#id3{ background-color :rgb(41, 37, 37)}
        /* 여러 선택자를 연달아 사용할 경우 콤마(,) 기호로 구분 */
        #id6 {color :rgba(228, 62, 62, 0.822);}

        /*클래스 선택자*/
        .class1{
            background-color :rgba(255, 175, 164, 0.781);
        }
        .class2{
            background-color :rgba(125, 218, 221, 0.705);
        }
    </Style>

</head>

<body>
    <h1>CSS란?</h1>
    <p>
        Cascading Style Sheets의 약자로<br>
        마크업 언어가 실제 표시되는 방법을 기술하는 언어.
        (웹 페이지 스타일 지정에 사용되는 언어의 표준 (W3C에서 표준으로 지정))
    </p>

    <hr>
    <h1>CSS 기본 선택자</h1>
    <p>
        선택자란?<br> 
        특정한 HTML태그를 선택할 때 사용하는 기능으로,<br>
        태그를 선택하여 원하는 스타일과 기능을 적용할 수 있다.
    </p>

    <hr>

    <h2>모든 요소 선택자 (*)</h2>
    <p>선택된 요소 내 모든 태그를 선택할 때 사용한다.</p>

    <hr>
    <h2>태그 선택자 (태그명)</h2>
    <p>HTML 문서 내에 같은 태그명을 가진 모든 태그를 선택</p>
    <pre> p{color : red;} 와 같은 형태로 사용함.</pre>

    <hr>
    <h2>아이디 선택자 (#)</h2>
    <p>HTML 문서 내에 해당하는 id 속성 값이 일치하는 태그를 선택</p>
    <pre> #id속성값 { color blue; } </pre>
    <ol>
        <li id="id1">아이디 선택자 테스트1</li>
        <li id="id2">아이디 선택자 테스트2</li>
        <li id="id3">아이디 선택자 테스트3</li>
        <li id="id4">아이디 선택자 테스트4</li>
        <li id="id5">아이디 선택자 테스트5</li>
    </ol>
    <h3 id="id6">id 속성의 값은 HTML문서 내에서 유일해야함. 중복 불가 (식별자 역할)</h3>

    <hr>
    <h2>클래스 선택자(.)</h2>
    <p>HTML 문서 내에서 동일한 class 속성 값을 가진 태그를 선택</p>
    <pre> .class속성값 { color : red; } </pre>
    <ul>
        <li class="class1">클래스 선택자 테스트1</li>
        <li class="class2">클래스 선택자 테스트2</li>
        <li class="class1">클래스 선택자 테스트1</li>
        <li class="class2">클래스 선택자 테스트2</li>
        <li class="class1">클래스 선택자 테스트1</li>
    </ul>
</body>
</html>
```
<br/>
#출력 화면<br/>


![css-1](https://user-images.githubusercontent.com/73421820/100424831-7e49dc80-30d1-11eb-8671-e8c8f2ea7bd1.PNG)

<br/><br/>

<br/>

## 02_CSS 선택자 2

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>02_CSS 선택자 2</title>
    
    <Style>
        /*기본 속성 선택자*/
        div[name=name2]{ background-color : rgb(117, 178, 202); }
        label[for=test]{ color :rgba(185, 48, 48, 0.877) }

        /*자식 선택자*/
        #test1>h4{ /*id가 test1인 태그의 자식 중 h4태그들 선택*/
            background-color :rgba(185, 178, 224, 0.829);
        }
        #test1>ul>li{
            background-color: rgba(136, 156, 223, 0.897);
        }

        /*후손 선택자*/
        #test1 ul{/*id가 test1인 태그의 후손 중 ul태그 모두 선택*/
            background-color: rgba(223, 168, 200, 0.897);
        }
        #test1 li{color : rgb(239, 243, 18);}

        /*동위 선택자*/
        #div-test2 + div{ /*id가 div-test2인 요소 뒤에 오는 div태그 선택*/
            background-color: rgb(117, 178, 202);
        }
      /*  #div-test3 ~ div{ id가 div-test3인 요소 뒤에 오는 모든 div태그 선택
            color : snow;
            background-color:  rgba(223, 168, 200, 0.897);
        } */

        /* 반응 선택자 */
        .reaction-test {
             width : 200px; /* 가로 폭 200px */
             height: 200px; /* 세로 높이 200px */
             background-color: yellow;
             margin-bottom: 10px;
             /* 요소간의 간격(아랫부분)을 10px 띄워줌 */
         }

        #active-test:active{
            /*id가 active-test인 요소가 클릭되었을 때*/
            background-color: black;
            color : yellow;
        }

        #hover-test:hover{
            /*id가 hover-test인 요소에 마우스가 올라갔을 때*/
            width: 300px;
            height: 300px;
        }

        /*상태 선택자*/
        input[name="fruits"]:checked{
        /*input 태그 중에서 name 속성이 fruits인 요소가 체크되었을 때*/
            width:30px;
            height:30px;
        }

        .focus-test:focus{
            /*클래스가 focus-test인 요소에 포커스가 맞추어 졌을 때*/
            background-color :rgba(185, 178, 224, 0.829);
        }

        option:disabled{
            /*option태그 중 비활성화된 태그*/
            background-color : gray;
            color:white;
        }
        option:enabled{
            /*option태그 중 활성화된 태그*/
            background-color:rgba(185, 178, 224, 0.829);
        }

    </Style>
</head>

<body>
    <h2>기본 속성 선택자</h2>
    <p>선택자 뒤에 [속성명=속성값] 형태로 작성하여<br>
        원하는 속성을 지닌 태그를 선택
    </p>

    <pre>선택자[속성명=속성값] { color : red; } </pre>
    <div name="name1">div 1</div>
    <div name="name2">div 2</div>
    <label for="test">테스트</label>

    <hr>
    <h2>자식 선택자, 후손 선택자</h2>
    <pre>
    자식선택자 : 선택자>선택자
    후손선택자 : 선택자 선택자
    </pre>
    <div id="test1">
        <h4>자식입니다.</h4>
        <h4>나도 자식입니다.</h4>
        <ul id="testul"> 리스트
            <li>ul태그의 자식이자 div 태그의 후손입니다.</li>
            <li>ul태그의 자식이자 div 태그의 후손입니다.</li>
        </ul>
    </div>

    <hr>
    <h2>동위 선택자</h2>
    <p>동위 관계에서 뒤에 위치한 태그를 선택할 때 사용</p>
    <pre>
    바로 뒤에 오는 요소(b) 선택 : 선택자a + 선택자b
    뒤에 오는 모든 요소 선택 :  선택자a ~ 선택자
    </pre>
    <div id="div-test1">동위 선택자 테스트1</div>
    <div id="div-test2">동위 선택자 테스트2</div>
    <div id="div-test3">동위 선택자 테스트3</div>
    <div id="div-test4">동위 선택자 테스트4</div>
    <div id="div-test5">동위 선택자 테스트5</div>

    <hr>
    <h2>반응 선택자</h2>
    <p>사용자의 움직임에 따라 스타일이 달라지는 선택자</p>
    <pre>
    사용자가 요소를 클릭할 때 : 선택자:active
    사용자가 요소에 마우스를 올렸을 때 : 선택자:hover 
    </pre>
    <div class="reaction-test" id="active-test">active 테스트</div>
    <div class="reaction-test" id="hover-test">hover 테스트</div>

    <hr>
    <h2>상태 선택자</h2>
    <p>입력 양식의 상태에 따라 선택되는 선택자</p>
    <pre>
    checkbox에 체크가 되었는지, 안되었는지에 따른 상태 지정
        -> input태그:checked
    </pre>
    <input type="checkbox" name="fruits"> 사과
    <input type="checkbox" name="fruits"> 바나나
    <input type="checkbox" name="fruits"> 딸기

    <pre>
    input 태그에 초점이 맞추어진 태그 선택
        -> input태그:focus    
    </pre>
    아이디 : <input type="text" id="userId" class="focus-test">
    <br>
    비밀번호 : <input type="password" id="password" class="focus-test">

    <pre>
    사용 가능한 input태그 선택 : input태그선택:enabled
    사용 불가능한 input태그 선택 : input태그선택:disabled
    </pre>
    <h4>당신의 연령대는?</h4>
    <select>
        <option disabled>10</option>
        <option>20</option>
        <option>30</option>
        <option>40</option>
        <option>50</option>
        <option>60</option>
    </select>

</body>
</html>
```
<br/>
#출력 화면<br/>


![css2-1](https://user-images.githubusercontent.com/73421820/100425259-35465800-30d2-11eb-9467-6a1b3a39da12.PNG)
![css2-2](https://user-images.githubusercontent.com/73421820/100425260-35deee80-30d2-11eb-920d-760b62ab8001.PNG)
![css2-3](https://user-images.githubusercontent.com/73421820/100425263-37101b80-30d2-11eb-8a84-b85065180553.PNG)




<br/><br/>

## 03_CSS 선택자 3

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>03_CSS 선택자 3</title>
	<style>
		/* 문자열 속성 선택자 */
		/* 
			~=
			띄어쓰기로 구분된 여러 속성값이 작성된 속성 중
			속성값이 특정값을 단어로 포함하는 태그 선택 
		*/
        div[name~="aaa"]{ background-color :rgba(149, 209, 147, 0.815);}


		/* 
			|=
			속성값이 특정값과 같은 속성 또는 
			여러 속성값 중 맨앞 이면서,
			"-" 기호가 포함된 속성값에 "-"기준 앞쪽 문자열이 특정값과 같은 경우
		 */
        div[class |= str] {color:rgb(207, 73, 63);}


		/* 
			^=
			속성값이 특정 값으로 시작하는 태그 선택
		*/
        div[class^=class]{background-color:rgba(59, 143, 143, 0.747);}



		/* 
			$=
			속성값이 특정 값으로 끝나는 태그 선택
		 */
        div[class$="class2"]{background-color:rgba(99, 116, 194, 0.87);}



		/*
			*=
			속성값이 특정 값을 포함하는 태그 선택
		*/
        div[class*="str"]{font-size : 40px}
    </style>
    
</head>

<body>
	<h3>문자열 속성 선택자</h3>
	<p>특정 문자열을 확인하여 스타일을 적용하는 방식으로 특별한 경우에만 사용</p>
	<pre>
	선택자[속성~=특정값]{설정내용} 
	- 띄어쓰기로 구분된 여러 속성값이 작성된 속성 중
	속성값이 특정값을 단어로 포함하는 태그 선택  
		
	선택자[속성|=특정값]{설정내용}
	- 속성값이 특정 값을 단어로 포함하는 태그 선택
	- "-"으로 구분, - 앞의 내용의 값이 동일해야 함.

	선택자[속성^=특정값]{설정내용}
	- 속성값이 특정 값으로 시작하는 태그 선택

	선택자[속성$=특정값]{설정내용}
	- 속성값이 특정 값으로 끝나는 태그 선택

	선택자[속성*=특정값]{설정내용}
	- 속성값이 특정 값으로 포함하는 태그 선택
	</pre>
	
	<div name='aaa bbb str-1' class='str-class'>div1</div>
	<div name='str-2 aaa' class='str-class'>div2</div>
	<div name='str' class='class-str'>div3</div>
	<div name='aaa str-3 ' class='str-class2'>div4</div>

	<hr>
</body>

</html>
```
<br/>
#출력 화면<br/>


![css3](https://user-images.githubusercontent.com/73421820/100425358-658df680-30d2-11eb-9c8d-881e736b95cd.PNG)





<br/><br/>

## 04_CSS 선택자 4.html

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>04_CSS 선택자 4</title>

    <link rel="stylesheet" href="04_CSS선택자4.css" type="text/css">
</head>
<body>
    <h2>일반 구조 선택자</h2>
    <p>특정한 위치에 있는 태그 선택(위치로 구분)</p>
    <div id="test1">
        <p>테스트1</p>
        <p>테스트2</p>
        <p>테스트3</p>
        <p>테스트4</p>
        <p>테스트5</p>
        <pre>테스트6(pre)</pre>
    </div>
    
    <hr>
    <h2>형태 구조 선택자(태그 별로 구분)</h2>
    <p>특정한 위치에 있는 태그 선택</p>
    <div id="test2">
        <p>테스트1</p>
        <p>테스트2</p>
        <p>테스트3</p>
        <p>테스트4</p>
        <p>테스트5</p>
        <pre>테스트6(pre)</pre>
    </div>

    <hr>
    <h2>문자 선택자</h2>
    <p>특정 문자 또는 문자열을 선택</p>
    <div id="test3">
        <p>
        CSS란 Cascading Style Sheets의 약자로
        마크업 언어가 실제 표시되는 방법을 기술하는 언어.<br>
        (웹 페이지 스타일 지정에 사용되는 언어의 표준 (W3C에서 표준으로 지정))
        </p>
        
        <p>
            CSS란 Cascading Style Sheets의 약자로
            마크업 언어가 실제 표시되는 방법을 기술하는 언어.<br>
            (웹 페이지 스타일 지정에 사용되는 언어의 표준 (W3C에서 표준으로 지정))
        </p>

    </div>

    <hr>
    <h2>부정 선택자</h2>
    <p>선택된 요소를 제외하여 선택 </p>
    <div id="test4">
        <p>테스트1</p>
        <p>테스트2</p>
        <p>테스트3</p>
        <p>테스트4</p>
        <p>테스트5</p>
        <pre>테스트6(pre)</pre>
    </div>


</body>
</html>
```
<br/><br/>

## 04_CSS 선택자 4.css

```css
/*일반 구조 선택자*/
/*형제 관계 태그 중 첫 번째 태그 선택*/
#test1 p:first-child{
    background-color: rgba(59, 143, 143, 0.747);
    color:white;
}

/*형제 관계 태그 중 마지막 태그 선택*/
#test1 p:last-child{
    /*p:last-child : test1의 마지막 자식이 p태그가 맞는지? 맞으면 바꾸라는 것*/
    background-color: orange;
    color: white;
}

/*형제 관계 태그 중 앞에서부터 지정된 수열번째 태그 선택*/
#test1 p:nth-child(2n){
    /*#test1의 후손 중 짝수번 째 p태그 선택*/
    background-color: rgba(99, 116, 194, 0.87);
}

/*형제 관계 태그 중 뒤에서부터 지정된 수열번째 태그 선택*/
#test1 p:nth-last-child(4){
    /*#test1의 후손 중 짝수번 째 p태그 선택*/
    background-color: rgba(74, 127, 197, 0.87);
    color:white;
}

/*형태 구조 선택자*/

/*형제 관계 태그 중 첫 번째 태그 선택*/
#test2 p:first-of-type{
    background-color: rgba(235, 111, 169, 0.856);
    color: white;
}

/*형제 관계 태그 중 마지막 태그 선택*/
#test2 p:last-of-type{
    background-color: rgba(236, 129, 96, 0.877);
    color:white;
}

/*형제 관계 태그 중 앞에서 부터 지정된 수열번째 태그 선택*/
#test2 p:nth-of-type(2n){
    background-color: rgb(231, 170, 170);
}
/*형제 관계 태그 중 뒤에서 부터 지정된 수열번째 태그 선택*/
#test2 p:nth-last-of-type(3n){
    background-color: rgba(241, 190, 22, 0.884);
    color:white;
}

/*문자 선택자*/

/*첫 번째 글자 선택*/
#test3 >p:first-letter{
    font-size : 28px;
}

/*첫 번째 줄 선택*/
#test3 >p:first-line{
    background-color:  rgba(238, 123, 88, 0.877);
}

/*태그 요소 마지막 부분 공간을 선택*/
#test3 > p:after{
    content : "@@@@@@@@@@@@@@@"
}

/*태그별로 숫자를 부여하는 속성*/
#test3 > p{
    counter-increment: rint;
} 

#test3>p:before{
    content : counter(rint) ">>";

}

/*사용자가 드래그한 글자 선택*/
#test3 p::selection{
    background-color: rgb(231, 170, 170);
}

/*부정 선택자*/
#test4 p:nth-of-type(2n-1){
    /*1,3,5의 색이 변경됨.*/
    background-color: rgba(99, 194, 186, 0.87);
}
#test4 p:not(:nth-of-type(2n-1)){
    /*not을 작성하면 2,4의 색이 변경됨*/
    background-color: rgba(99, 116, 194, 0.87);
}
```
<br/>
#출력 화면<br/>


![css4-1](https://user-images.githubusercontent.com/73421820/100425573-d503e600-30d2-11eb-8a71-6cd830f40a07.PNG)
![css4-2](https://user-images.githubusercontent.com/73421820/100425575-d59c7c80-30d2-11eb-904a-db937d0d3b17.PNG)


<br/><br/>

## 05_선택자 우선 순위

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>05_선택자 우선 순위</title>
    <Style>
        /*5순위 태그 선택자*/
        div{
            width:300px;
            /*최우선 순위 선택자 !important*/
            height:300px !important;
            background-color: rgba(99, 116, 194, 0.87);
        }

        /*4순위 클래스 선택자*/
        .class1{
            background-color:rgba(216, 125, 160, 0.719);
            color:white;
        }

        /*3순위 아이디 선택자*/
        #test1{
            height:100px;
            color:black;
            font-size : 5px;
        }

        
    </Style>

</head>
<body>
    <h1>선택자 우선 순위</h1>
    <p>
        기본적으로 CSS속성은 위에서부터 아래로 차례대로 적용되지만,<br>
        같은 태그에 여러 CSS속성이 설정된 경우에는 <br>
        선택자 우선 순위에 따라 스타일이 적용된다.
    </p>
    <pre>
    1순위 : !important 키워드가 붙은 CSS 속성.
    2순위 : inline style 속성.(태그에 직접 작성하는 CSS 속성)
    3순위 : id선택자로 부여된 속성.
    4순위 : class 선택자
    5순위 : tag 선택자
    </pre>
                                 <!--2순위 inline style속성-->
    <div id="test1" class="class1" style="font-size:28px">우선 순위 테스트</div>
    
</body>
</html>
```
<br/>
#출력 화면<br/>

![css5](https://user-images.githubusercontent.com/73421820/100425782-2c09bb00-30d3-11eb-9bf3-f4b7764aca4a.PNG)



<br/><br/>

## 06_글꼴 관련 스타일.html

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>06_글꼴 관련 스타일</title>
    <link rel="sytlesheet" href="06_글꼴관련스타일.css" type="text/css">

    /*구글 웹 폰트 */
    <link rel="preconnect" href="https://fonts.gstatic.com">
    <link href="https://fonts.googleapis.com/css2?family=Dokdo&display=swap" rel="stylesheet">

</head>
<body>
    <h2>font-family 속성 : 텍스트의 글꼴 지정 속성</h2>
    <pre>선택자{font-family:"글꼴이름";}</pre>
    <p id="ff1">ABCD abcd 가나다라 1234</p>
    <p id="ff2">ABCD abcd 가나다라 1234</p>
    <p id="ff3">ABCD abcd 가나다라 1234</p>

    <h4>웹 폰트 사용 방법</h4>
    <p>웹 폰트 제공 사이트 예시 :
        <a href="https://fonts.google.com" target="_blank">
        구글 무료 웹폰트 사이트
        </a>
    </p>
    <p id="wf1">ABCD abcd 가나다라 1234</p>

    <p>웹 폰트 제공 사이트 예시 :
        <a href="https://noonnu.cc" target="_blank">
        눈누 상업용 무료 한글 폰트 사이트
        </a>
    </p>
    <p id="wf2">ABCD abcd 가나다라 1234</p>

    <hr>
    <h3>font-size 속성 : 텍스트 크기 변경 속성</h3>
    <pre> 선택자 {font-size:크기;} </pre>
    <ul>
        <li id="size1">고정크기 px 테스트</li>
        <li id="size2">가변크기 em 테스트</li>
        <li id="size3">가변크기 % 테스트</li>
    </ul>

    <hr>
    <h3>font-weight : 글꼴의 두께 표현</h3>
    <pre>
    선택자 {font-weight : normal(기본값) | bold | bolder | lighter | 100~900;}
    </pre>
    <ul>
        <li id="fw1">굵은 글꼴</li>
        <li id="fw2">상위 요소에서 상속된 굵기보다 굵은 글꼴</li>
        <li id="fw3">상위 요소에서 상속된 굵기보다 얇은 글꼴</li>
    </ul>

    <hr>
    <h3>font-variant : 입력된 영어 알파벳 중 소문자를 작은 대문자로 표시</h3>
    <p>Hello World!</p>
    <p id="fv">Hello World!</p>

    <hr>
    <h3>font-style : 텍스트의 기울임 지정</h3>
    <pre>선택자{font-style : normal | italic | oblique}</pre>
    <ul>
        <li id="fs1">abcd ABCD 1234 가나다라</li>
        <li id="fs2">abcd ABCD 1234 가나다라</li>
        <!--글자를 기울이는 태그 <i><em>보다 css를 활용하는게 더 좋다.-->
    </ul>

    <hr>
    <h3>font 속성 : 글꼴 관련 스타일을 한번에 지정함</h3>
    <pre>
        선택자{
            font :font-style font-variant font-weight
            font-size/line-height font-family;
        }
    </pre>
    <ul>
        <li id="f1">글꼴 관련 스타일1</li>
        <li id="f2">글꼴 관련 스타일2</li>
        <li id="f3">글꼴 관련 스타일3</li>
    </ul>

</body>
</html>
```
<br/>

## 06_글꼴 관련 스타일.css

```css
@font-face {
    font-family: 'Binggrae-Bold';
    src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_one@1.0/Binggrae-Bold.woff') format('woff');
    font-weight: normal;
    font-style: normal;
}


/*font-family : 글꼴 지정*/
#ff1{
    font-family: "궁서체";
    /*컴퓨터에 설치된 글꼴 또는 웹*/
}
#ff2{
    font-family:"굴림";
}
#ff3{
    font-family:"kh체","굴림";
    /*폰트를 여러 개 작성하여 앞에서 부터 적용해보고, 없으면 다음 폰트 적용*/
}

#wf1{
    font-family :"Dokdo";
}
#wf2{
    font-family: "Binggrae-Bold";
}

/*font-size : 폰트 크기 조절*/
#size1{font-size:25px;}
#size2{font-size : 2em;}
/*em : 부모 요소의 폰트 크기를 1em으로 지정하고 몇 배씩 늘어남*/
#size3{font-size:200%;}

/*font-weight : 글씨 굵기*/
#fw1{font-weight:900;} /*600이상 숫자 또는 bold*/
#fw2{font-weight:bolder;}
/* 부모 요소(ul)의 글꼴 굵기가 기본상태 이므로, fw2는 기본 보다 굵은 bold로 출력됨.*/
#fw3{font-weight:}
/* 부모 요소(ul)의 글꼴 굵기가 기본상태 이므로, fw3는 기본 보다 얇게 출력됨.*/

/*font-vatiant*/
#fv{font-variant: small-caps;}

/*font-style*/
#fs1{font-style: italic;}
/*italic : 필기체 느낌으로 기울어진 폰트*/

#fs2{font-style: oblique;}
/*oblique : 보통 모양을 그대로 기울인 것*/

/*font*/
#f1{
    font : 20px/40px "궁서체"
    /*크기 20px, 장평 40px, 글씨체 궁서체*/
}
#f2{
    font : italic bold 30px/30px "고딕체";
}
#f3{
    font : italic small-caps 900 2em/50px "바탕"
}
```
<br/>
#출력 화면<br/>


![css6-1](https://user-images.githubusercontent.com/73421820/100426274-eef1f880-30d3-11eb-94e4-fb6eda7aeaad.PNG)
![css6-2](https://user-images.githubusercontent.com/73421820/100426276-f0232580-30d3-11eb-97a8-6f17a9ce2456.PNG)

<br/><br/>

## 07_텍스트 관련 스타일.html


```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>07_텍스트 관련 스타일</title>
    <link rel="stylesheet" href="07_텍스트관련스타일.css" type="text/css">
</head>
<body>
    <h3>color : 텍스트의 색상 지정 </h3>
    <pre>
        선택자 {
            color : 색상명 | rgb16진수 | rgb10진수 | rgba | hsl | hsla;
        }
    </pre>
    <ul>
        <li id="c-name">색상명으로 지정</li>
        <li id="c-16">16진수 값으로 지정</li>
        <li id="c-rgb">rgb 값으로 지정</li>
        <li id="c-rgba">rgba 값으로 지정</li>
        <li id="c-hsl">hsl 값으로 지정</li>
        <li id="c-hsla">hsla 값으로 지정</li>
    </ul>

    <hr>
    <h3>text-decoration : 텍스트에 줄을 긋거나 없애는 속성</h3>
    <pre>
        선택자{text-decoration : none | underline | overline | line-through;}
    </pre>
    <ul>
        <li><a id="td1" href="https://www.naver.com">네이버로 이동</a></li>
        <li id="td2">밑줄</li>
        <li id="td3">윗줄</li>
        <li id="td4">삭제선</li>

    </ul>

    <hr>
    <h3>text-shadow : 텍스트에 그림자 효과를 줄 때 사용</h3>
    <pre>
    선택자{text-shadow : none | 가로거리 세로거리 번짐정도 그림자색,
        가로거리 세로거리 번짐정도 그림자색,
        가로거리 세로거리 번짐정도 그림자색,} /*그림자 여러개로 섞이게 할 수 있다.*/
    </pre>

    <div id="bg">
        <span id="ts1" class="shadow">HTML5</span>
        &nbsp;&nbsp;&nbsp;&nbsp;
        <span id="ts2" class="shadow">HTML5</span>
        &nbsp;&nbsp;&nbsp;&nbsp;
        <span id="ts3" class="shadow">HTML</span>
        &nbsp;&nbsp;&nbsp;&nbsp;
        <span id="ts4" class="shadow">HTML5</span>
    </div>
</body>
</html>
```
<br/>


## 07_텍스트 관련 스타일.css
<br/>

```css
/* color : 텍스트 색상*/
#c-name{color:rgba(255, 99, 71, 0.726);}
#c-16{color:#7aaa3b;}
#c-rgb{color:rgb(107, 125, 182);}
#c-rgba{color: rgba(180, 92, 9, 0.658);}
/*rgba에서 a는 불투명(0~1, 1이 제일 불투명하다.)*/
#c-hsl{color: hsla(345, 82%, 62%, 0.781)}
/*h(0~360) : 색상, s(0~100%): 채도, l(0~100%): 명도*/
#c-hsla{color: hsl(320, 76%, 77%,0.8)}


/*text-decoration*/
#td1{
    text-decoration : none;
    color:black;
    font-weight : bold;
    font-size:1.2em;
}
#td2{
    text-decoration : underline;
}
#td3{
    text-decoration : overline;
}
#td4{
    text-decoration : line-through;
}

/*text-shadow*/
#bg{
    background-color : black;
    margin:30px;
    padding:50px;
}
/*shadow 클래스에게 공통된 폰트 사이즈와 두께 지정*/
.shadow{
    font-size:100px;
    font-weight:bold;
}

#ts1{
    color:orangered;
    text-shadow: 5px 5px orange;
}
#ts2{
    color:white;
    text-shadow:0px 1px 20px white;
}
#ts3{
    color:white;
    text-shadow:1px 1px 20px magenta,
                1px 1px 30px rgb(255, 0, 0);
}
#ts4 {
    color : black;
    text-shadow : 0px 0px 4px #ccc, 0px -5px 4px #ff3, 2px -10px 6px #fd3, 
    -2px -15px 11px #f80, 2px -19px 18px #f20;
}

```
<br/>
#출력 화면<br/>


![css7-1](https://user-images.githubusercontent.com/73421820/100426526-58720700-30d4-11eb-9ba8-c57b859e9d2f.PNG)
![css7-2](https://user-images.githubusercontent.com/73421820/100426528-59a33400-30d4-11eb-8f15-82c7c92c5066.PNG)
