---
title: "2020년 12월 03일"
excerpt: "CSS-5"
search: true
categories: 
  - Academy
tags: 
  - HTML
  - CSS
toc: true
---

## 05_웹 문서 구조2

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>05_웹 문서 구조2</title>
    <link rel="stylesheet" href="css/style.css" type="text/css">
    <!-- type 은 반드시 작성해야 되는 것은 아니지만, 확실히 하기 위해 작성하는 것이 좋음. -->
</head>
<body>



    <!-- 시맨틱 태그는 아직 많은 사이트에 배포되지 않은 상태,, div로 영역 나누기-->
    <div class="wrapper">
        <div class="header">
            <div id="header-1" class="header-float">
                <a href="05_웹문서구조2.html">
                    <img id="home-logo" src="logo1.jpg">
                </a>
            </div>
            <div id="header-2" class="header-float">
                <form id="search-form" action="#" method="get">
                    <!-- 내부에 작성된 input태그의 값을 지정된 주소로 전달하는 역할 -->
                    <div id="search-text-area">
                        <input type="text" id="search-input" name="search-input">
                    </div>
        
                    <div id="search-btn-area">
                        <button type="submit" id="search-btn">검색</button>
                    </div>
        
                </form>
            </div>
            <div id="header-3" class="header-float"></div>
        </div>





        <div class="nav">

            <!-- 메인 메뉴 목록 -->
        <ul id="main-menu">
            <li> <a href="#">Home</a> </li>

            <li> 
                <a href="#">Front-End</a>
            
                  <!-- 서브 메뉴 -->
                  <ul>
                    <li><a href="#">HTML5</a></li>
                    <li><a href="#">CSS3</a></li>
                    <li><a href="#">Javascript</a></li>
                    <li><a href="#">jQuery</a></li>
                    <li><a href="#">Ajax</a></li>
                </ul>
            </li>

            <li> 
                <a href="#">게시판</a>
                  <!-- 서브 메뉴 -->
                  <ul>
                    <li><a href="#">공지사항</a></li>
                    <li><a href="#">자유게시판</a></li>
                    <li><a href="#">질문게시판</a></li>
                </ul>
            </li>

            <li> <a href="#">Q & A</a></li>
        </ul>

        </div>




        <div class="content">
            <div id=content-1 class="content-float"></div>
            <div id="content-2" class="content-float"></div>

            <div id="content-3" class="content-float">
                <div id="content-login">
                    <form id="login-form" name="login-form" method="get">
                
                        <!-- ID/PWD 입력창을 묶는 div -->
                        <div id="login-input-id-pwd"> 
                            <!-- 아이디 입력 영역 -->
                            <div id="login-input-id">
                                 <input type="text" id="input-id" name="input-id"
                                     placeholder="ID를 입력하세요.">
                             </div>
                
                            <!-- 비밀번호 입력 영역 -->
                            <div id="login-input-pwd">
                                 <input type="password" id="input-pwd" name="input-pwd"
                                    placeholder="PWD를 입력하세요.">
                                </div>
                        </div>
                
                        <!-- 로그인 버튼 영역 -->
                        <div id="login-input-btn">
                                <button type="submit" id="login-btn">로그인</button>
                        </div>
                     </form>
                
                            <!-- 회원가입, ID/PWD 찾기 영역 -->
                        <div id="siginUp-find">
                            <a href="#">회원가입</a>
                        <a href="#">ID/PWD 찾기</a>
                        </div>
                        </div>
                
  
                </div>
            </div>
        </div>   

        <div class="footer"></div>
    </div>


</body>
</html>
```
<br/>


<br/>

## 06_검색창 폼 만들기

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>06_검색창 폼 만들기</title>
    <style>
        div{
            border : 1px solid rebeccapurple;
            box-sizing:border-box;
        }

        #header-2{
            width:650px;
            height:160px;

            position : relative;
            /* 자식요소를 top,bottom,left,right 로 제어할 수 있게 설정 */
        }

        /* 검색창 전체 */
        #search-form{
            border: 1px solid rebeccapurple;
            width:80%;
            height:20%;

            position : absolute;
            margin: auto;
            top:0;
            bottom:0;
            right: 0;
            left: 0;
        }

        /* 검색 입력 영역 */
        #search-text-area{
            width:80%;
            height:100%;
            float: left; /* 수평 분할, 왼쪽 정렬 */
        }

        /* 검색 버튼 영역 */
        #search-btn-area{
            width:20%;
            height:100%;
            float: left; /* 수평 분할, 왼쪽 정렬 */
        }

        /* 검색 input창 */
        #search-input{
            width:100%;
            height:100%;
            /* 부모 크기만큼 꽉 채워진다 */
            /* input태그는 기본적으로 고유한 border, padding 값이 존재하여
               width, height를 그냥 지정할 경우 영역을 초과하게 된다. 
               -> box-sizing : border-box; 를 이용하여 문제 해결 가능하다.*/
            box-sizing : border-box; 
        }

         /* 검색 버튼 */
        #search-btn{
            width:100%;
            height:100%;
            box-sizing:border-box;
        } 

    </style>
</head>
<body>
    <div id="header-2">    
        <form id="search-form" action="#" method="get">
            <!-- 내부에 작성된 input태그의 값을 지정된 주소로 전달하는 역할 -->
            <div id="search-text-area">
                <input type="text" id="search-input" name="search-input">
            </div>

            <div id="search-btn-area">
                <button type="submit" id="search-btn">검색</button>
            </div>

        </form>
    </div>
</body>
</html>
```






<br/><br/>

## 07_로그인 폼 만들기

```html

<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>07_로그인 폼 만들기</title>
    <style>
        div{
            border: 1px solid seagreen;
            box-sizing: border-box;
        }

        #content-3{
            width:200px;
            height:440px;
        }

        #content-login{
            width:100%;
            height:30%;
        }
       
        /* 로그인 영역 */
        #login-form{
            width:100%;
            height: 80%;
            
        }
        /* 회원가입, 아이디/비밀번호 찾기 영역 */
        #siginUp-find{
            width: 100%;
            height: 20%;
            text-align: center;
        }

        /* 아이디/비밀번호 입력 영역 */
        #login-input-id-pwd{
            width: 65%;
            height: 100%;
            float:left;
        }

        /* 로그인 버튼 영역 */
        #login-input-btn{
            width: 35%;
            height: 100%;
            float:left;
        }

        /* 아이디 입력 영역, 비밀번호 입력 영역 */
        #login-input-id, #login-input-pwd{
            width:100%;
            height:50%;
        }

        /* 아이디 입력 input창, 비밀번호 입력 input창 */
        #input-id, #input-pwd {
            width:100%;
            height:100%;
            box-sizing: border-box;
        }
 
        /* 로그인 버튼 */
        #login-btn{
            width:100%;
            height:100%;
        }


    </style>

<body>
    <div id="content-3">
        <div id="content-login">
            <form id="login-form" name="login-form" method="get">
        
                <!-- ID/PWD 입력창을 묶는 div -->
                <div id="login-input-id-pwd"> 
                    <!-- 아이디 입력 영역 -->
                    <div id="login-input-id">
                         <input type="text" id="input-id" name="input-id"
                             placeholder="ID를 입력하세요.">
                     </div>
        
                    <!-- 비밀번호 입력 영역 -->
                    <div id="login-input-pwd">
                         <input type="password" id="input-pwd" name="input-pwd"
                            placeholder="PWD를 입력하세요.">
                        </div>
                </div>
        
                <!-- 로그인 버튼 영역 -->
                    <div id="login-input-btn">
                        <button type="submit" id="login-btn">로그인</button>
                    </div>
             </form>
        
                    <!-- 회원가입, ID/PWD 찾기 영역 -->
                    <div id="siginUp-find">
                        <a href="#">회원가입</a>
                        <a href="#">ID/PWD 찾기</a>
                    </div>
                </div>
        
                
          
            



            
        </div>
    </div>
</body>
</html>
```



<br/><br/>

## 08_nav 메뉴 만들기

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>08_nav 메뉴 만들기</title>

    <style>
        .nav{
            /* 메인 메뉴가 존재하는 영역 크기 지정 */
            width:1000px;
            height: 40px;
            border : 1px solid aquamarine
        }


        /* 메인 메뉴(ul) 스타일 지정 */
        #main-menu{
            /* ul 태그 고유의 margin, padding 제거 */
            padding : 0;
            margin : 0;

            /* nav 전체에 맞는 크기 지정 */
            width: 100%;
            height: 100%;

            list-style-type: none;
        }

        /* 메인 메뉴(li) 스타일 지정 */
        #main-menu>li{

            /* 메인 메뉴 왼쪽 정렬 */
            float:left;

            /* 메인 메뉴 카테고리 사이즈 지정 */
            width:25%;
            height:100%;

            /* 메인 메뉴 배경색 */
            background-color: lightpink;

            /* 메인 메뉴 글씨 상하 가운데로 조정 */
            line-height: 40px;
        }

        #main-menu li a{
            /* #main-menu의 후손 중 li태그의 후손 중 a태그 선택 */
            
            /* a태그의 형식을 inline-block으로 변경하여 width, height 지정을 가능하도록 함. */
            display: inline-block;

            width: 100%;

            /* 글씨 가운데 정렬 */
            text-align : center;

            /* a태그 밑줄 지우기 */
            text-decoration : none;

            /* font 스타일 지정 */
            font-size : 16px;
            font-weight:bold;
            font-family:"굴림";
            color:white;

        }


        #main-menu li:hover{
            /* #main-menu 요소 후손 중 li태그에 마우스가 왈라갔을 때 */
            /* li 태그 배경색을 변경 */

            background-color: lightsalmon;
        }

        /* 서브메뉴 스타일 지정 */
        #main-menu > li > ul{
            /* ul태그 고유 영역 제거 */
            padding: 0px;
            margin: 0px;

            /* 서브메뉴 불렛 제거 */
            list-style-type: none;

            /* 배경색 추가 */
            background-color: rgb(255, 183, 159);

            /* 서브메뉴를 투명하게 */
            opacity: 0;
        }

        /* 서브메뉴에 있는 a태그 숨기기 */
        #main-menu ul >li >a {
            opacity: 0;
            /* opacity : 투명도를 나타냄,   (투명)0~1(불투명) */

        }


        /* 서브메뉴 숨기기 */
        #main-menu ul>li{
            height: 0;
            line-height:0;
        }

                    /* 마우스가 올라 간 메인메뉴에 후손 중 ul태그가 있고, ul의 후손 중 a태그를 선택 */
        #main-menu>li:hover ul, #main-menu > li:hover ul a{
            opacity:1;
        }

        #main-menu li:hover ul li{
            /* 서브메뉴의 영역 크기를 지정하여 눈에 보이게 하기 */
            height: 40px;
            line-height:40px;
        }

        /* 서브메뉴 슬라이드 효과 */
        #main-menu li{
            transition-duration: 0.5s;
        }

        /* 서브메뉴 배경 fade in-out 효과 */
        #main-menu li ul{
            transition-duration: 0.5s;
        }



    </style>
</head>
<body>
    <div class="nav">

        <!-- 메인 메뉴 목록 -->
        <ul id="main-menu">
            <li> <a href="#">Home</a> </li>

            <li> 
                <a href="#">Front-End</a>
            
                  <!-- 서브 메뉴 -->
                  <ul>
                    <li><a href="#">HTML5</a></li>
                    <li><a href="#">CSS3</a></li>
                    <li><a href="#">Javascript</a></li>
                    <li><a href="#">jQuery</a></li>
                    <li><a href="#">Ajax</a></li>
                </ul>
            </li>

            <li> 
                <a href="#">게시판</a>
                  <!-- 서브 메뉴 -->
                  <ul>
                    <li><a href="#">공지사항</a></li>
                    <li><a href="#">자유게시판</a></li>
                    <li><a href="#">질문게시판</a></li>
                    
                </ul>
            
            </li>

            <li> <a href="#">Q & A</a></li>


        </ul>

    </div>
</body>
</html>
```


<br/><br/>

## style.css

```css

/* 05_웹문서구조2.html에 사용할 css 파일 */

/* div 경계선 확인 + box-sizing 조정 */
div{
    /* border : 1px solid black; */

    box-sizing:border-box;
    /* width, height 속상 값의 적용 범위를 border까지로 지정
      --> content + padding + border */
}

/* 전체를 감싸고 있는 div */
.wrapper{
    width: 1000px;
    height:800px;

    /* 해당 요소를 가로 가운데 정렬시키는 방법 */
    margin : auto;
}

/* header */
.header{
    width:100%;
    height:20%;
}

/* header 내부 div 수평 분할 */
.header-float{
    height: 100%;
    float : left;
}


/* header 내부 div 영역 크기 지정 */
#header-1{
    width : 20%;
    position : relative;
    /* 자식 요소에 top, bottom, left, right를 이용한 상대적 위치 지정 가능 */
}

#header-2{
    width : 65%;
    /* 검색창을 가운데 오게 하기 위해서 추가함 */
    position : relative;
}
#header-3{width : 15%;}

/*로고 크기 지정*/
#home-logo{
    width:50%;
    height:auto;
    /* 기존 이미지 크기에서 width가 절반으로 감소
        ->height는 width감소 비율 만큼 자동으로 감소 */
    position : absolute;
    margin: auto; /* 다른 요소와의 간격을 자동으로 맞춰 줌*/
    top:0; /* 부모 요소 상단에서 0px 만큼 떨어짐 */
    bottom:0;
    left:0;
    right:0;
    /* 정확히 한 가운데에 위치하게 된다. */
}   


/* nav */
.nav{
    width : 100%;
    height: 5%;
    clear : both;
}

/* content */
.content{
    width:100%;
    height:55%;
}

/* content 내부 div 수평 분할 */
.content-float{
    height:100%;
    float: left;
}

/* content 영역 크기 지정 */
#content-1{width:15%;}
#content-2{width:65%;}
#content-3{width:20%;}

/* footer */
.footer{
    width:100%;
    height:20%;
}



/*************** 검색창 관련 스타일 ************/

 /* 검색창 전체 */
 #search-form{
    /* border: 1px solid rebeccapurple; */
    width:80%;
    height:20%;

    position : absolute;
    margin: auto;
    top:0;
    bottom:0;
    right: 0;
    left: 0;
}

 /* 검색 입력 영역 */
#search-text-area{
    width:80%;
    height:100%;
    float: left; /* 수평 분할, 왼쪽 정렬 */
}

 /* 검색 버튼 영역 */
#search-btn-area{
    width:20%;
    height:100%;
    float: left; /* 수평 분할, 왼쪽 정렬 */
}

 /* 검색 input창 */
#search-input{
     width:100%;
    height:100%;
    /* 부모 크기만큼 꽉 채워진다 */
    /* input태그는 기본적으로 고유한 border, padding 값이 존재하여
        width, height를 그냥 지정할 경우 영역을 초과하게 된다. 
        -> box-sizing : border-box; 를 이용하여 문제 해결 가능하다.*/
    box-sizing : border-box; 
}

/* 검색 버튼 */
#search-btn{
    width:100%;
    height:100%;
    box-sizing:border-box;
} 



/******** 로그인 영역 ********/
#content-login{
    width:100%;
    height:30%;
}

/* 로그인 영역 */
#login-form{
    width:100%;
    height: 80%;
    
}
/* 회원가입, 아이디/비밀번호 찾기 영역 */
#siginUp-find{
    width: 100%;
    height: 20%;
    text-align: center;
}

/* 아이디/비밀번호 입력 영역 */
#login-input-id-pwd{
    width: 65%;
    height: 100%;
    float:left;
}

/* 로그인 버튼 영역 */
#login-input-btn{
    width: 35%;
    height: 100%;
    float:left;
}

/* 아이디 입력 영역, 비밀번호 입력 영역 */
#login-input-id, #login-input-pwd{
    width:100%;
    height:50%;
}

/* 아이디 입력 input창, 비밀번호 입력 input창 */
#input-id, #input-pwd {
    width:100%;
    height:100%;
    box-sizing: border-box;
}

/* 로그인 버튼 */
#login-btn{
    width:100%;
    height:100%;
}



/* ********* nav 스타일 *********** */
/* 메인 메뉴(ul) 스타일 지정 */
#main-menu{
    /* ul 태그 고유의 margin, padding 제거 */
    padding : 0;
    margin : 0;

    /* nav 전체에 맞는 크기 지정 */
    width: 100%;
    height: 100%;

    list-style-type: none;
}

/* 메인 메뉴(li) 스타일 지정 */
#main-menu>li{

    /* 메인 메뉴 왼쪽 정렬 */
    float:left;

    /* 메인 메뉴 카테고리 사이즈 지정 */
    width:25%;
    height:100%;

    /* 메인 메뉴 배경색 */
    background-color: lightpink;

    /* 메인 메뉴 글씨 상하 가운데로 조정 */
    line-height: 40px;

    position:relative;
}

#main-menu li a{
    /* #main-menu의 후손 중 li태그의 후손 중 a태그 선택 */
    
    /* a태그의 형식을 inline-block으로 변경하여 width, height 지정을 가능하도록 함. */
    display: inline-block;

    width: 100%;

    /* 글씨 가운데 정렬 */
    text-align : center;

    /* a태그 밑줄 지우기 */
    text-decoration : none;

    /* font 스타일 지정 */
    font-size : 16px;
    font-weight:bold;
    font-family:"굴림";
    color:white;

}


#main-menu li:hover{
    /* #main-menu 요소 후손 중 li태그에 마우스가 왈라갔을 때 */
    /* li 태그 배경색을 변경 */

    background-color: lightsalmon;
}

/* 서브메뉴 스타일 지정 */
#main-menu > li > ul{
    /* ul태그 고유 영역 제거 */
    padding: 0px;
    margin: 0px;

    /* 서브메뉴 불렛 제거 */
    list-style-type: none;

    /* 배경색 추가 */
    background-color: rgb(255, 183, 159);

    /* 서브메뉴를 투명하게 */
    opacity: 0;

    position:absolute;
    width:100%;
}

/* 서브메뉴에 있는 a태그 숨기기 */
#main-menu ul >li >a {
    opacity: 0;
    /* opacity : 투명도를 나타냄,   (투명)0~1(불투명) */

}


/* 서브메뉴 숨기기 */
#main-menu ul>li{
    height: 0;
    line-height:0;
}

            /* 마우스가 올라 간 메인메뉴에 후손 중 ul태그가 있고, ul의 후손 중 a태그를 선택 */
#main-menu>li:hover ul, #main-menu > li:hover ul a{
    opacity:1;
}

#main-menu li:hover ul li{
    /* 서브메뉴의 영역 크기를 지정하여 눈에 보이게 하기 */
    height: 40px;
    line-height:40px;
}

/* 서브메뉴 슬라이드 효과 */
#main-menu li{
    transition-duration: 0.5s;
}

/* 서브메뉴 배경 fade in-out 효과 */
#main-menu li ul{
    transition-duration: 0.5s;
}


```
<br/><br/>
출력화면
<br/>


![1](https://user-images.githubusercontent.com/73421820/101022483-caef5500-35b4-11eb-836e-5c3046fb63bd.PNG)


<br/><br/>


## 19_ 트랜지션

```html

<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>19_ 트랜지션</title>
    <style>


       .test{
           width: 100px;
           height: 100px;
           border :1px solid  rgb(33, 86, 129);
           background-color: coral;
       } 

       /* 마우스 오버(:hover) 시 2초의 시간동안 배경색을 노란색으로 변경 */
       .tran1:hover{
           background-color: yellow;
           transition-duration:2s;
           /* duration : 지속시간 */
           
       }

       /* 마우스 오버 해제 시 2초의 시간동안 원래색인 빨간색으로 변경 */
       .tran1{
           transition-duration: 10s;
       }




       /* 마우스 오버시 배경색을 2초 동안 파란색, 
       너비를 3초동안 200px, 높이를 1초동안 200px,
       한바퀴 회전을 3초 동안 변형 */
       .tran2:hover{
            background-color: blue;
            width: 200px;
            height: 200px;
            transform:rotateZ(3600deg);

            /* 트랜지션을 지정할 속성을 선택 */
            transition-property: background-color, width, height, transform;

            /* 지정된 속성 순서대로 시간 지정 */
            transition-duration: 2s, 3s, 1s, 3s;
       }

       .tran2{
            /* 트랜지션을 지정할 속성을 선택 */
            transition-property: background-color, width, height, transform;

            /* 지정된 속성 순서대로 시간 지정 */
            transition-duration: 2s, 3s, 1s, 3s;

       }


       /* 트랜지션 진행 시간별 속도 지정 */
       .tran3:hover{
           width: 300px;
           background-color :cornflowerblue;
           transition-duration: 5s;
           transition-timing-function: ease-out;
       }

        /* 트랜지션 시간 지연 */
       .tran4:hover{
            transition-duration: 2s;
            transform: rotateZ(1800deg);
            transition-delay: 1s;
        }



    </style>
</head>
<body>
    <h1>트랜지션</h1>
    <p>시간에 따라 웹 요소의 스타일 속성이 조금씩 변화하게 하는 속성</p>

    진행 시간 지정
    <div class="test tran1"></div>
    <hr>

    속성별 진행 시간 지정
    <div class="test tran2"></div>
    <hr>

    트랜지션 진행 시간별 속도 지정
    <pre>
   transition-timing-function: 
   linear | ease | ease-in | ease-out | ease-in-out | cublic-bezier(n,n,n,n)
    </pre>
    <div class="test tran3"></div>

    <hr>

    트랜지션 시간 지연
    <div class="test tran4"></div>
    

</body>
</html>
```
<br/><br/>
