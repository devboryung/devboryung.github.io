---
title: "2020년 12월 01일"
excerpt: "CSS-4"
search: true
categories: 
  - Academy
tags: 
  - HTML
  - CSS
toc: true
---

## 01_기본 구조

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>01_기본 구조</title>

    <style>
        /*수직 4분할*/
      #wrapper2{
          width : 300px;
          height : 500px;
      }

      .dd{
          width : 100%;
        box-sizing : border-box;
      }

      .div-red{
          height:10%;
          background-color:orangered;
      }

      .div-orange{
          height:20%;
          background-color : orange;
      }

      .div-yellow{
          height:30%;
          background-color : yellow;
      }

      .div-green{
          height:40%;
          background-color : green;
      }


      /* 수평 2분할 */
      #wrapper3{
          width:400px;
          height:300px;
      }

      .test3{
          width:50%;
          height:100%;
          box-sizing : border-box;
          float: left;
      }

      .div3-1{background-color: yellow;}
      .div3-2{background-color: green;}


      /* 4분할 */
      #wrapper4{
          width:400px;
          height: 400px;
      }

      .test4{
          width:50%;
          height:50%;
          box-sizing : border-box;
          float:left;
      }

      .div4-1{ background-color:mediumseagreen;}
      .div4-2{ background-color :lightpink;}
      .div4-3{ background-color: mediumpurple;}
      .div4-4{ background-color:cornflowerblue;}


      /* 4분할 -2 */
      #wrapper5{
            width: 400px;
            height: 400px;
        }
        
        .test5{ box-sizing: border-box; }

        .row{
            width: 100%;
            height: 50%;
        }

        .col{
            width: 50%;
            height: 100%;
            float: left;
        }

        .test5-1{ background-color: yellow;}
        .test5-2{ background-color: pink;}
        .test5-3{ background-color: skyblue;}
        .test5-4{ background-color: greenyellow;}


    </style>
</head>
<body>
     <hr>

     <h3>div요소 수직 4분할(서로 비율 다르게(10%,20%,30%,40%))</h3>

     <div id="wrapper2">
        <div class="dd div-red"></div>
        <div class="dd div-orange"></div>
        <div class="dd div-yellow"></div>
        <div class="dd div-green"></div>
     </div>

     <hr>

     <h3>수평 2분할</h3>

     <div id="wrapper3">
         <div class="test3 div3-1"></div>
         <div class="test3 div3-2"></div>
     </div>

     <hr>

     <h3>4분할</h3>
     <div id="wrapper4">
        <div class="test4 div4-1"></div>
        <div class="test4 div4-2"></div>
        <div class="test4 div4-3"></div>
        <div class="test4 div4-4"></div>
     </div>


    <!--유지보수를 위해 행과 열을 구분하는게 좋다.  
        나중에 test4-1~4-4 바꾸기-->
     <hr>
     <h3>4분할-2</h3>
     <div id="wrapper5">
         <div class="test5 row">
            <div class="test5 col test5-1"></div>
            <div class="test5 col test5-2"></div>
         </div>
         <div class="test5 row">
            <div class="test5 col test5-3"></div>
            <div class="test5 col test5-4"></div>

         </div>
     </div>
</body>
</html>
```
<br/>


<br/>

## 02_웹문서 구조1

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>02_웹문서 구조1</title>

    <style>
        div{
            /* 모든 div에 적용할 스타일 작성 */
            border : 1px solid black;
            /* 테두리를 1px 검은색 실선  */
        
            box-sizing:border-box;
            /* width/heigth 속성 값이 content, padding, boder의 합과 같게 설정 */
        }

        /* 전체를 감싸고 있는 wrapper 설정 */
        .wrapper{
            width:1000px;
            height:800px;

            /* 상위 요소(body==창) 가로 중앙에 배치 */
            margin:auto;
        }

        .header{
            width:100%;
            height:20%;
        }
        .content{
            width:100%;
            height:60%;
        }

        .content-1{
            width:15%;
            height:100%;
            float:left;
        }

        .content-2{
            width:85%;
            height:100%;
            float:left;
        }

        .footer{
            width:100%;
            height:20%;
        }

    </style>
</head>
<body>
    <!-- 재사용을 위해, 영역 구분할때 class 사용하는게 좋음 -->
    <div class="wrapper">
        <div class="header"></div>

        <div class="content">
            <div class="content-1"></div>
            <div class="content-2"></div>
        </div>

        <div class="footer"></div>
    </div>


</body>
</html>
```
<br/>
<br/>

## 03_웹문서 구조 예제.html

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>03_웹문서 구조 예제</title>
    <link rel="stylesheet" href="03_웹문서구조예제.css" type="text/css">

</head>
<body>
    <div class="header">
        <!-- 다른 폴더에 있을경우,,src="img/logo1.jpg" -->
        <img id="logo1" src="logo1.jpg" width="100px" height="100px">
   
        <!-- 오른쪽 상단 로그인 폼 -->
        <table id="login-form">
            <tr>
                <td>아이디</td>
                <td>
                    <input type="text" name="userId" id="userId">
                </td>
            </tr>
            
            <tr>
                <td>패스워드</td>
                <td>
                    <input type="password" name="userPw" id="userPw">
                </td>
            </tr>

            <tr>
                <td colspan="2">
                    <!-- 버튼 생성하는 2가지 방법 -->
                    <input type="submit" value="로그인">
                    <button type="reset">취소</button>
                </td>
            </tr>
        </table>
   
    </div>

    <div class="nav">
        <ul>
            <!-- # : test용으로 임시로 써놓는 값,, 현재 페이지로 이동이 됨 -->
            <li><a href="#">메인</a></li>
            <li><a href="#">공지사항</a></li>
            <li><a href="#">자유게시판</a></li>
            <li><a href="#">갤러리</a></li>
            <li><a href="#">회원관리</a></li>
            <li><a href="#">채팅하기</a></li>
        </ul>
    </div>



    <div class="section">
        <div class="aside">
            <img id="logo2" src="logo2.jpg">
        </div>

        <div class="content"></div>
    </div>


    <div class="footer">
        Copyright ⓒ 1998-2020 KH Information Educational Institute All Right Reserved

    </div>



</body>
</html>
```




<br/><br/>

## 03_웹문서 구조 예제.css

```css

*{
    /* 모든 요소에 margin 을 없앰 */
    margin:0;
}
/* 
div{
    border : 1px solid black;
    box-sizing : border-box;
} */

/* header 영역 */
.header{
    width:100%;
    height:100px;
}

/* navigator 영역 */
.nav{
    width:100%;
    height : 50px;

    background-color: #ccc;
}

/* Section 영역 */
.section{
    width:100%;
    height:400px;
}

/* aside 영역 */
.aside{
    width: 15%;
    height: 100%;
    float: left;

    background-color: #ccc;
}

/* content 영역 */
.content{
    width:85%;
    height:100%;
    float: left;
   
}


/* footer 영역 */
.footer{
    width:100%;
    height:50px;

    text-align : center;
    line-height : 50px;
    background-color: #e1dede;
}


/* header 영역 이미지/ 로그인 창 */
#logo1{float:left;}

#login-form{float:right;}



/* ul태그 선택 */
.nav>ul{
    list-style-type:none; /*불렛없음+*/
}

/* nav에 있는 a태그 선택 */
.nav>ul>li>a{ /*.nav a 도 가능*/
    text-decoration:none;

    /* 글자 20px 흰색 두껍게 */
    font-size:20px;
    color: white;
    font-weight :bold; 
}

/* nav 메뉴(li태그) */
.nav>ul>li{
    display:inline-block;  /* 수평 분할 + width/height 적용 o*/
    width:15%;

    line-height:45px;    

}

/* aside 내부 이미지 */
#logo2{
    width:100%;
    height:100px;
}

/* 선택자 중복 작성 가능 */
.content{
    background-color: gray;
}


```



<br/><br/>

## 04_웹문서 구조 예제2.html

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>04_웹문서 구조 예제2</title>
    <link rel="stylesheet" href="04_웹문서구조예제2.css" type="text/css">
</head>
<body>
    <!-- 시맨틱(Semantic) 태그 
        - 페이지 구조를 특정 기능에 맞는 태그로 사용하여 구분.
        - 페이지 구조를 쉽게 파악 가능하고 좀 더 정확한 정보 검색이 가능하여
          웹 접근성을 높이는 기술이다.

          (시맨틱 태그는 div태그가 이름이 바뀐것 뿐이다.)
     -->

     <header>
         <!-- 로고1 -->
         <img id="logo1"  src="logo1.jpg" width="100px" height="100px"></img>
         <!-- 로그인 -->
         <table id="login-form">
          <tr>
            <td >아이디</td>
            <td><input type="text" id="userId" name="uesrId"></td>
          </tr>
          
          <tr>
            <td>패스워드</td>
            <td><input type="password" id="userPw" name="uesrPw"></td>
              
        </tr>

        <tr >
            <td colspan="2">
            <button type="submit">로그인</button>  
            <button type="reset">취소</button>  
        </td>
        </tr>

         </table>
     </header>

     <nav>
        <!-- 메뉴 작성 -->
        <ul>
            <li>
                <a href="#">메인</a>
            </li>
            <li>
                <a href="#">공지사항</a>
            </li>
            <li>
                <a href="#">자유게시판</a>
            </li>
            <li>
                <a href="#">갤러리</a>
            </li>
            <li>
                <a href="#">회원관리</a>
            </li>
            <li>
                <a href="#">채팅하기</a>
            </li>
        </ul>
        </nav>

     <section>
         <aside>
            <image id="logo2" src="logo2.jpg"></image> 

         </aside>
         <article class="content"></article>
         <!-- article : 웹 페이지 내용이 작성되는 태그 -->
     </section>

     <footer>
        Copyright ⓒ 1998-2020 KH Information Educational Institute All Right Reserved
     </footer>
</body>
</html>
```


<br/><br/>

## 04_웹문서 구조 예제2.css

```css

임시로 테두리 지정해 놓음.
header, nav, section, aside, article, footer{
    border : 1px solid black;
    box-sizing: border-box;
}

/* 모든 요소에 마진을 없앰 */
*{margin : 0}

header{
    width : 100%;
    height : 100px; 

}

nav{
    width: 100%;
    height:50px;

    background-color: #ccc;
}

section{
    width:100%;
    height:400px;
}

aside{
    width:15%;
    height:100%;

    float:left;
    background-color: #ccc;
}
.content{
    width:85%;
    height:100%;

    float:left;

    background-color: gray;
}

footer{
    width:100%;
    height:50px;

    text-align : center;
    line-height: 50px;
    background-color: #e1dede;
}

#logo1{  float:left; }

#login-form{   float:right; }

#logo2{
    width:100%;
    height:100px;
}

nav>ul{
    
    list-style-type: none;
}

nav a{
    text-decoration: none;
    font-size: 20px;
    color:white;
    font-weight:bold;
    
}
nav li{
    display: inline-block;
    width:15%;

    line-height:45px;

}

```
<br/><br/>

