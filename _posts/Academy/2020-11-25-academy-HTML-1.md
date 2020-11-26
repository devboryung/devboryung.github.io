---
title: "2020년 11월 25일"
excerpt: "HTML-1"
search: true
categories: 
  - Academy
tags: 
  - HTML
toc: true
---

## 01_글자관련태그
<br/>

```html
<!DOCTYPE html>
<!-- html5 문서의 형식 선언-->

<html lang="ko">
<!-- <html> : html 문서의 내용을 정의하는 부분 -->
<!-- lang="ko" : 페이지가 어떤 언어로 되어있나 검색엔진에 노출시키는 속성 -->

<head>
    <!-- 문서의 제목, 스타일, 스크립트 등의 문서의 일반적인 정보(메타데이터) 를 정의
             <title>, <style>, <script>, <link>, <meta> 등이 작성됨 -->

    <meta charset="UTF-8">
    <!--HTML 문서의 문자 인코딩 방식을 지정 -->
    <!-- <meta> : HTML 문서가 가지고 있는 유용한 정보를 담아두는 곳 -->

    <title>01_글자관련태그</title>
</head>

<body>
    <!-- HTML 문서의 내용(content)를 정의하는 부분 -->
    <!-- <hn> 태그 (n은 1~6 사이 숫자)
            내용의 제목을 입력할 때 사용하는 태그, 폰트의 크기가 태그별로 지정되어 있음.-->
    <h3> &lt;h3&gt; : 제목을 입력하는 태그</h3>
    <!-- &lt; : <        &gt;  : > -->
    <h1>h1: 태그입니다.</h1>
    <h2>h2: 태그입니다.</h2>
    <h3>h3: 태그입니다.</h3>
    <h4>h4: 태그입니다.</h4>
    <h5>h5: 태그입니다.</h5>
    <h6>h6: 태그입니다.</h6>
    그냥 글입니다.

    <h3>&lt;hr&gt; 태그 : 줄을 바꾸면서 수평선을 넣는 태그</h3>
    <hr>
    <h3>&lt;br&gt; 태그 : 줄바꿈 태그</h3>
    마크업 언어에서 연달아 작성된 모든 공백, 줄바꿈은<br>
    브라우저 해석 시 하나의 띄어쓰기로만 인식이 됨.
    <hr>

    <h3>&lt;p&gt; 태그 : 단락을 구분하는 태그</h3>
    <p>첫 번째 단락입니다.</p>
    <p>두 번째 단락입니다.</p>

    <hr>
    <h3>&lt;pre&gt;태그 : 단락을 구분하는 태그 + 입력한 내용을 그대로 표현하는 태그</h3>
    <pre>
pre 태그는 
    여러칸 띄우기 또는 탭키, 줄바꿈 등의 공백을 
        작성한 모양 그대로 표현하는 태그이다.
        </pre>

    <hr>
    <h3>그 밖의 텍스트를 다루는 태그</h3>
    일반 글꼴<br>
    <b>&lt;b&gt; : 단순히 글자를 굵게 표시하는 태그</b>
    <br>
    <strong>&lt;strong&gt; : 글자를 강조하는 태그(웹 접근성을 강화시킨 태그)</strong>
    <br>
    <i>&lt;i&gt; : 단순히 글자를 기울이는 태그</i>
    <br>
    <em>&lt;em&gt; : 글자를 강조하는 태그(웹 접근성을 강화시킨 태그)</em>

    <br><br><br>

    <mark>&lt;mark&gt; : 형광펜 효과를 나타내는 태그</mark>
    <br>
    <u>&lt;u&gt; : 글자에 밑줄을 긋는 태그</u>
    <br>
    <small>&lt;small&gt; : 글자를 작게 표시하는 태그</small>
    <br>
    &lt;sup&gt; : 윗첨자를 나타내는 태그 x<sup>2</sup>
    <br>
    &lt;sub&gt; : 아래첨자를 나타내는 태그 log<sub>2</sub>
    <br>
    &lt;s&gt; : 취소선을 긋는 태그 <s>잘못작성함</s>
    <br>
    &lt;abbr&gt; : 마우스 오버 시 튤립 형태로 글자를 출력하는 태그
    <abbr title="DataBase Management System.">DBMS</abbr>
    <br>
    &lt;code&gt; : 컴퓨터 인식을 위한 소스 코드를 담는 태그<br>
    <pre>
            <code>
                public class test{
                    public static void main(String[] args){

                    }
                }
            </code>   
        </pre>


    &lt;kbd&gt; : 키보드 같은 사용자 입력 내용을 표시하는 태그
    <p>
        복사 단축키 :
        <kbd>ctrl</kbd> + <kbd>c</kbd>
    </p>

    <hr>
    <p>
    <h1>글자 관련 태그 응용</h1>
    태그들은 해당 태그 내에서 <mark>중첩</mark>으로 사용 가능하다.<br>
    <strong ong>굵은</strong>글자를 작성 할 수 도 있고,<br>
    <em>기울이거나</em>,<u>밑줄</u>,<s>취소선</s>을 넣는 등 다양하게 사용할 수 있다.
    </p>
    <hr> 
</body>

</html>
```
<br/>
#출력 화면<br/>

![html1-1](https://user-images.githubusercontent.com/73421820/100327312-7d03ab80-300e-11eb-82ad-a59c34bc323b.PNG)
![html1-2](https://user-images.githubusercontent.com/73421820/100327410-9b69a700-300e-11eb-8a5a-888bdd20560f.PNG)


<br/><br/>

## 02_목록관련태그
<br/>

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>02_목록관련태그</title>
</head>
<body>
    <h1>&lt;ul&gt;: 순서 없는 목록</h1>

    &lt;ul&gt; 태그 : 목록 태그에 들어가는 요소를 작성하는 태그
    <h3>화면 구현 수업 내용</h3>
    <ul>
        <li>HTML5</li>
        <li>CSS3</li>
        <li>Javascript</li>
        <li>jQuery</li>
    </ul>

    <h1>&lt;ol&gt; : 순서가 있는 목록 태그</h1>
    <p>
        순서 있는 목록 태그는 숫자, 알파벳 등을 이용하여 목록의 순서를 부여함.<br>
        순서를 나타내는 기호는 type 속성을 이용해 바꿀 수 있고,<br>
         기본값은 숫자로 되어있음.(1부터 시작)
    </p>
    <ol>
        <li>HTML5</li>
        <li>CSS3</li>
        <li>Javascript</li>
        <li>jQuery</li>
    </ol>
    <h4>영문 소문자로 표기</h4>
    <ol type="a">
        <li>HTML5</li>
        <li>CSS3</li>
        <li>Javascript</li>
        <li>jQuery</li>
    </ol>

    <h4>영문 대문자로 표기</h4>
    <ol type="A">
        <li>HTML5</li>
        <li>CSS3</li>
        <li>Javascript</li>
        <li>jQuery</li>
    </ol>

    <h4>로마 숫자 소문자로 표기</h4>
    <ol type="i">
        <li>HTML5</li>
        <li>CSS3</li>
        <li>Javascript</li>
        <li>jQuery</li>
    </ol>

    <h4>로마 숫자 대문자로 표기</h4>
    <ol type="I">
        <li>HTML5</li>
        <li>CSS3</li>
        <li>Javascript</li>
        <li>jQuery</li>
    </ol>

    <h4>시작 값 변경하기</h4>
    <ol type="1" start="5">
        <li>HTML5</li>
        <li>CSS3</li>
        <li>Javascript</li>
        <li>jQuery</li>
    </ol>

    <h4>숫자 역순</h4>
    <ol type="1" reversed>
        <li>HTML5</li>
        <li>CSS3</li>
        <li>Javascript</li>
        <li>jQuery</li>
    </ol>
</body>
</html>

```
<br/>
#출력 화면<br/>

![html2-1](https://user-images.githubusercontent.com/73421820/100328394-d02a2e00-300f-11eb-88b4-1c0c672d9a55.PNG)
![html2-2](https://user-images.githubusercontent.com/73421820/100328885-5cd4ec00-3010-11eb-87cc-937b8c4623a3.PNG)


<br/><br/>

## 03_글자, 목록 태그 연습문제
<br/>

```html
<!DOCTYPE html>
<html lang="ko">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>03_글자, 목록 태그 연습문제</title>
</head>

<body>
    <h1>[NCS]반응형 스마트웹&콘텐츠 개발자 양성과정</h1>
    <h3>담당 강사 : 백동현</h3>
    이메일 : baekdh@iei.or.kr
    <hr>
    <ul>
        <li>프로그래밍 언어</li>
        <ul>
            <li><s>Java</s></li>
        </ul>
        <li>데이터 베이스</li>
        <ul>
            <li><s>Oracle</s></li>
        </ul>
        <li>화면 구현</li>
        <ol>
            <li><mark>HTMLS</mark></li>
            <li>CSS3</li>
            <li>javaScript</li>
            <li>jQuery</li>
            <li>Ajax</li>
        </ol>
        <li>서버 개발 기술</li>
        <ol type="i">
            <li><s>JDBC</s></li>
            <li>Servlet</li>
            <li>JSP</li>
        </ol>
        <li>프레임워크</li>
        <ol type="a">
            <li><Strong>Spring</Strong></li>
            <li>MyBatis</li>
        </ol>

    </ul>

</body>

</html>


```
<br/>
#출력 화면<br/>

![html3](https://user-images.githubusercontent.com/73421820/100329045-85f57c80-3010-11eb-9ce8-1295b59ae69f.PNG)



<br/><br/>

## 04_표 관련 태그
<br/>

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>04_표 관련 태그</title>

    <style>
        table{
            background-color : rgba(78, 93, 230, 0.123);
        }
        thead{
            background-color: rgba(42, 130, 202, 0.726);
        }
        tbody{
            background-color: rgba(78, 114, 233, 0.541);
            
        }
        tfoot{
            background-color: rgba(132, 78, 233, 0.541);
        }
    </style> 
</head>
<body>
    <h3>&lt;table&gt;</h3>
    <pre>
    웹 문서에서 자료 정리를 위해 주로 사용되는 태그.

    행과 열로 이루어져있고, 행과 열이 만나는 지점을 셀(cell)이라고 한다.

    (border 속성 : table의 테두리를 지정하는 속성)
    </pre>

    <h3>&lt;tr&gt;</h3>  <!--<tr>-->
    <pre>
    한 행(row)을 만드는 태그
    </pre>
    
    <h3>&lt;td&gt;</h3> <!--<td>-->
    <pre>
    하나의 열을 만드는 태그
    </pre>

    <h3>&lt;th&gt;</h3> <!--<th>-->
    <pre>
    열에 대한 제목을 표시하는 태그  
    (가운데 정렬 + 굵은 글씨로 표시됨) 
    </pre>

    <h3>&lt;caption&gt;</h3> <!--<caption>-->
    <pre>
    테이블의 제목 또는 내용을 추가하는 태그
    </pre>

    <hr>
    <h2>기본적인 표 만들기</h2>
    <table border="1">
        <!--border="1" : 테이블에 두께 1px 짜리 테두리 생성-->
        <caption>웹 브라우저의 종류</caption>
        <tr> <!--행-->
            <th> 브라우저 명 </th> <!--열의 제목-->
            <th> 제조사 </th>
            <th> 홈페이지 </th>
        </tr>
        <tr>
            <td>IE</td>
            <td>MS</td>
            <td>https://www.microsoft.com</td>
        </tr>
        <tr>
            <td>Chrome</td>
            <td>Google</td>
            <td>https://www.google.com</td>
        </tr>
        <tr>
            <td>Safari</td>
            <td>Apple</td>
            <td>https://www.apple.com</td>
        </tr>
        <tr>
            <td>FireFox</td>
            <td>Mozilla</td>
            <td>https://www.mozilla.com</td>
        </tr>
    </table>

    <figure>
        <figcaption>테이블 또는 이미지 설명에 사용되는 태그</figcaption>
    </figure>

    <hr>

    <h2>행 병합, 열 병합</h2>

    <p>
        rowspan 속성 : 행 병합 (위 아래 병합)<br>
        colspan 속성 : 열 병합 (양 옆 병합)<br>
    </p>
    <!-- width : 현재 td태그의 폭 지정 
         height : 현재 td 태그의 높이 지정-->
        
    <table border='1'>
        <tr>
            <td colspan="2" rowspan="2" width = "140px" height = "140px">사진</td>
            <td width ="70px"> 이름</td>
            <td width="210px"></td>
        </tr>
        <tr>
            <td>연락처</td>
            <td></td>
        </tr>
        <tr>
            <td width = "70px" height="35px">주소</td>
            <td colspan="3"></td>
          
        </tr>
        <tr>
            <td height="140px">자기소개</td>
            <td colspan="3"></td>
        </tr>

    </table>
    
    <hr>
        <h2>테이블 태그의 구조 설정</h2>
        <pre>
        &lt;thead&gt; : 테이블의 상단 부분영역
        &lt;tbody&gt; : 테이블의 중단 부분영역
        &lt;tfoot&gt; : 테이블의 하단 부분영역
        </pre>

    <table border="1">
        <thead>
            <tr>
                <th>이름</th>
                <th>나이</th>
                <th>주소</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>강보령</td>
                <td>30</td>
                <td>동작</td>
            </tr>
            <tr>
                <td>강보</td>
                <td>30</td>
                <td>사당</td>
            </tr>
            <tr>
                <td>강보보</td>
                <td>30</td>
                <td>이수</td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td colspan="2">총 인원</td>
                <td>3명</td>
            </tr>
        </tfoot>
    </table>
    
    
</body>
</html>


```

<br/>
#출력 화면<br/>

![html4-1](https://user-images.githubusercontent.com/73421820/100329454-04521e80-3011-11eb-8391-0b1c9f0cf0d9.PNG)
![html4-2](https://user-images.githubusercontent.com/73421820/100329464-074d0f00-3011-11eb-828d-1bee7fe38430.PNG)
<br/><br/>

## 05_표 관련 태그 연습문제
<br/>

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>05_표 관련 태그 연습문제</title>
    <style>
        thead{ /* thead 영역에 배경색 지정*/
            background-color: rgba(85, 118, 226, 0.884)
        }
        tbody{ /* tbody 영역에 배경색 지정*/
            background-color: rgb(184, 189, 233)
        }
        tfoot{ /* tfoot 영역에 배경색 지정*/
            background-color:rgb(238, 184, 184)
        }

        tbody th,tfoot th{
            /* tbody태그 내부에 작성된 th 또는
               tfoot태그 내부에 작성된 th */
            background-color: rgba(84, 189, 230, 0.459)


        }
    </style>
</head>
<body>
    <table border="1">
        <thead>
            <tr>
                <th colspan="5" width="250px" height="20px">하수정 보석 컬렉션</th>
            </tr>
            
        </thead>
        <tbody>
            <tr>
                <th rowspan="5">제품리스트</th>
                <th>코드</th>
                <th>분류</th>
                <th>가격</th>
                <th>구매가능 개수</th>
                      
        </tr>
        <tr>
            <td>01-468</td>
            <td>가을</td>
            <td>200,000원</td>
            <td>1068</td>
        </tr>
        <tr>
            <td>01-469</td>
            <td>가을</td>
            <td>150,000원</td>
            <td>1700</td>
        </tr>
        <tr>
            <td>01-470</td>
            <td>여름</td>
            <td>950,000원</td>
            <td>2500</td>
        </tr>
        <tr>
            <td>01-471</td>
            <td>봄</td>
            <td>120,000원</td>
            <td>3200</td>
        </tr>
    </tbody>
    <tfoot>
    <tr>
            <th colspan="3">총합</th>
            <td>1,420,000 원</td>
            <td>8468</td>
            
        </tr>
    </tfoot>
    </table>
</body>
</html>

```
<br/>
#출력 화면<br/>

![html5](https://user-images.githubusercontent.com/73421820/100329642-4b401400-3011-11eb-8277-ea81a841a47a.PNG)

<br/><br/>

## 06_영역 관련 태그
<br/>
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>06_영역 관련 태그</title>
    <style>
        div{
            border : 1px solid black;
            /* div 태그에 1px, 실선,검정색 테두리를 표시*/
        }

        /* # == id속성 */
        #div1{background-color :  rgba(65, 105, 225, 0.705);}
        #div2{background-color : rgba(243, 174, 165, 0.76);}
        #div3{background-color : peachpuff;}

        span{
            border : 1px solid black;
            /* div 태그에 1px, 실선,검정색 테두리를 표시*/
        }

        /* # == id속성 */
        #span1{background-color : rgba(65, 105, 225, 0.705);}
        #span2{background-color : rgba(243, 174, 165, 0.76);}
        #span3{background-color :peachpuff;}
    </style>
</head>
<body>
    <h1>영역 관련 태그</h1>
    <pre>
    영역을 구분하는 태그   
    - &lt;p&gt; : 단락 구분
    - &lt;pre&gt; : 단락 구분 + 입력한 문자 그대로 표시
    - &lt;div&gt; : 영역을 나누는 태그.(block형식)
    - &lt;span&gt; : 영역을 나누는 태그.(inline형식)
    </pre>

    <h3>div 태그</h3>
    <p>
        div 태그 영역은 이미 존재하는 내용의 다음줄에 영역이 설정됨.<br>
    --> block 형식 : 공간을 수직 분할하고, width, height 속성이 사용 가능함.
    </p>

    <div id="div1">첫 번째 영역</div>
    <div id="div2">두 번째 영역</div>
    <div id="div3">세 번째 영역</div>
    <hr>
    <h3>span 태그</h3>
    <p>
        span 태그 영역은 이전 태그와의 줄바꿈을 일으키지 않고<br>
        바로 옆에 영억이 설정됨.<br>
    --> inline 형식 : 공간을 수평으로 분할하고, width, height 속성 사용 X
    </p>
    <span id="span1">첫 번째 영역</span>
    <span id="span2">두 번째 영역</span>
    <span id="span3">세 번째 영역</span>

    <hr>

    <h3>div와 span 태그 영역 지정 방식 차이점</h3>
    <pre>
    div 태그 : 사각형의 박스 형태로 영역을 지정
    span 태그 : 내용(content) 단위로 영역을 지정
    </pre>

    <h4>div 태그 영역 지정 확인</h4>
    <div id="div2">
        👨 🦳찌개가 왜이렇게 짜🥘⁉ 짜긴 뭐강 짱 👵 엄마! 🐷이거 뭐에요?🤷 ♂ 고구마호박👵 고구마호박이🍠 뭐에요?🤷 ♂ 고구마호박 모올랑😒 호박 맛나는 💛노오란 고구망🍠 👩🏻고구마호박이 아니라 ✖ 호박고구마요 어머님 😂 움~ 🥄👅 맛있다 🤭 마트 다녀오셨어요? 👀 👵영기엄마가 텃밭👩 🌾에서 고구마호ㅂ🤐 호박고구마요어머님.🍠 그래...!!😰 호굼!🤭 👩🏻푸흡... 호구마요?🤪 호.박.고.구.ㅁ🤐 호.박.고.구.마. 🍠 호박고구마‼ 호❗박❗고❗구❗마 이제됐냐🤬 ...흡... 흑... 😭 헝헝헝.... 👺 이 할망구가 왜이래 밥상에서‼ .... 😢
    </div>

    <h4>span 태그 영역 지정 확인</h4>
    <span id="span2">
        👨 🦳찌개가 왜이렇게 짜🥘⁉ 짜긴 뭐강 짱 👵 엄마! 🐷이거 뭐에요?🤷 ♂ 고구마호박👵 고구마호박이🍠 뭐에요?🤷 ♂ 고구마호박 모올랑😒 호박 맛나는 💛노오란 고구망🍠 👩🏻고구마호박이 아니라 ✖ 호박고구마요 어머님 😂 움~ 🥄👅 맛있다 🤭 마트 다녀오셨어요? 👀 👵영기엄마가 텃밭👩 🌾에서 고구마호ㅂ🤐 호박고구마요어머님.🍠 그래...!!😰 호굼!🤭 👩🏻푸흡... 호구마요?🤪 호.박.고.구.ㅁ🤐 호.박.고.구.마. 🍠 호박고구마‼ 호❗박❗고❗구❗마 이제됐냐🤬 ...흡... 흑... 😭 헝헝헝.... 👺 이 할망구가 왜이래 밥상에서‼ .... 😢
    </span>

    <hr>
    <h2>iframe 태그</h2>
    <p>웹 문서 안에 다른 웹 문서를 추가하는 태그(inline 형식)</p>
    <iframe width="400" heiht="300"
        src="https://www.youtube.com/embed/qRFcHRIjkOA"
        frameborder="0"></iframe>

</body>
</html>
```
<br/>
#출력 화면<br/>

![html6-1](https://user-images.githubusercontent.com/73421820/100329933-a83bca00-3011-11eb-8357-672201632d52.PNG)
![html6-2](https://user-images.githubusercontent.com/73421820/100329936-a8d46080-3011-11eb-97db-dcb28dc1c8dc.PNG)


<br/><br/>

<br/><br/>