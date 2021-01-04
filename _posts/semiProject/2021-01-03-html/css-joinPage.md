---
title: "2020년 01월 03일"
excerpt: "뭉개뭉개 - 일반 회원가입 화면"
search: true
categories: 
  - Academy
  - Semi-Project
tags: 
  - HTML
  - CSS
toc: true
---

## 출력 화면
![회원가입 화면](https://user-images.githubusercontent.com/73421820/103459436-c1dbda00-4d52-11eb-9952-167673a197b2.png)

<br><br>

## HTML

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>일반 회원가입</title>
    <link rel="stylesheet" href="join.css" type="text/css">
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100;300;400;500;700;900&display=swap" rel="stylesheet">
</head>
<body>
    <div class="wrapper">
        <div class="joinTitle">
            <h2> 일반 회원가입</h2>
        </div>

        
        <form action="#" method="POST" onsubmit="return memberJoinvalidate();">
            <div class="memberJoin">
                <hr>

                <!-- 아이디 입력 -->
                <div>
                    <div class="lb">
                        <label for="userId">아이디</label> <br>
                    </div>
                    <div class="ip">
                        <input type="text" class="inputTag" id="userId" name="" autocomplete="off" required>
                    </div>
                    <div>
                        <span id="checkId" >&nbsp;</span>
                    </div>    
                </div>
                
                <!-- 비밀번호 입력 -->
                <div>
                    <div class="lb">
                        <label for="pwd1">비밀번호</label>  <br>
                    </div>
                    <div class="ip">
                        <input type="password" class="inputTag"  id="pwd1" name=""  required>
                    </div>     
                    <div>
                        <span id="chechPwd1" >&nbsp;</span>
                    </div>  
                </div>

                <!-- 비밀번호 재확인 -->
                <div>
                    <div class="lb">
                        <label for="pwd2">비밀번호 재확인</label> <br>
                    </div>
                    <div class="ip">
                        <input type="password" class="inputTag"  id="pwd2" name=""   required>
                    </div>
                    <div>
                        <span id="chechPwd2" >&nbsp;</span>
                    </div>  
                </div>
                
                <!-- 닉네임 -->
                <div>
                    <div class="lb">
                        <label for="userName">닉네임</label> <br>
                    </div>
                    <div class="ip">
                        <input type="text" class="inputTag"  id="userName" name="" required>
                    </div>
                    <div>
                        <span id="checkUserName" >&nbsp;</span>
                    </div> 
                </div>
                
                <!-- 이메일 -->
                <div>
                    <div class="lb">
                        <label for="email">이메일</label> <br>
                    </div>
                    <div class="ip">
                        <input type="text" class="inputTag display-ib email"  id="email1" name="" autocomplete="off" required> 
                        &nbsp;@&nbsp;
                        <select class="inputTag display-ib email" id="email2" name="" required>
                            <option style="color:gray;">이메일 주소 선택</option>
                            <option>daum.net</option>
                            <option>naver.com</option>
                            <option>gmail.com</option>
                            <option>nate.com</option>
                            <option>hanmail.net</option>
                        </select>
                    </div>
                    <br>  
                    <div class="ip display-ib">
                        <input type="text" class="inputTag email" id="verifyEmail" placeholder="인증번호를 입력해주세요." 
                                style="background-color: rgb(196, 195, 195);"    readonly="true" required>
                    </div>
                    <div class= "verifyBtn display-ib">
                        <input type="button" id="emailBtn" value="인증번호 받기">
                    </div>
                    <div>
                        <span id="checkEmail" >&nbsp;</span>
                    </div> 
                </div>

                <!-- 전화번호 -->
                <div>
                    <div class="lb">
                        <label for="phone">전화번호</label>
                    </div>
                    <div class="ip">
                        <select class="display-ib inputTag phone" id="phone1" name="" required> 
                            <option>010</option>
                            <option>011</option>
                            <option>016</option>
                            <option>017</option>
                            <option>019</option>
                        </select>
                        &nbsp;-&nbsp;
                        <input type="number" class="display-ib inputTag phone" id="phone2" name="" required>
                        &nbsp;-&nbsp;
                        <input type="number" class="display-ib inputTag phone" id="phone3" name="" required>
                    </div>
                    <div>
                        <span id="checkPhone" >&nbsp;</span>
                    </div> 
                </div>

                <!-- 성별 -->
                <div>
                    <div class="lb">
                        <label for="gender">성별</label>
                    </div>
                    <div class="ip">
                        <select class="inputTag gender" id="gender" name="" required>
                            <option>성별</option>
                            <option>여자</option>
                            <option>남자</option>
                            <option>선택안함</option>
                        </select>
                    </div>
                </div>

                <br><br>
                <div class="submit">
                    <input id="submitBtn" type="submit" name="" value="회원가입">
                </div>
            </div>
        </form>
    </div>

</body>
</html>
```

<br><br>

## CSS

```css
/* 전체 글씨체 지정 */
*{
    font-family: 'Noto Sans KR', sans-serif;
    /* font-weight: 100; --> 굵기 지정 */
    font-weight: 500;
}

/* 전체 화면 */
.wrapper{
    padding:17px 0 17px;
    background-color : #f7f7f7 ;
}

/* 타이틀 */
.joinTitle{
    text-align : center;
    height: 80px;
    line-height : 60px;
}

/* 회원가입 입력 화면 */
.memberJoin{
    width: 400px;
    padding:0;
    margin : 0 auto;
}

/* 레이블 태그 class명*/
.lb{
    font-weight : bold;
    line-height: 25px;
    font-size: 13px;
    margin: 0 auto;
}

/* 인풋 태그 class명 */
.inputTag{
    margin: 0 auto;
    height:40px;
    width: 390px;
    border : 1px solid #bdbbbb;
}

/* inline-block 스타일 들어가야 하는 곳 전부 class 속성값 추가 */
.display-ib{
    display:inline-block;
}

/* 이메일 1,2번째 칸 */
.email{
    height:40px;
    width:170px;
}

/* 이메일 두번째 칸(옵션)*/
#email2{
    height:45px;
    width: 183px;
}

/* 이메일 인증 버튼 */
.verifyBtn{
    width:120px;
    height : 30px;
    padding-left: 10px;
}

/* 이메일 인증번호 입력하는 인풋창 */
#verifyEmail{
    width:250px;
    height:30px
}


/* number타입의 화살표 지우기 */
input[type="number"]::-webkit-outer-spin-button, input[type="number"]::-webkit-inner-spin-button{
        -webkit-appearance: none;
		margin: 0;
}

/* 핸드폰번호 입력 input창 */
.phone{
    width : 109px;
    height : 45px;   
}

/* 회원가입 버튼 */
#submitBtn{
    margin: 0 auto;
    height:40px;
    width: 390px;
    border : 1px solid  #8bd2d6;
    background-color: #8bd2d6;
}
```

<br><br><br>