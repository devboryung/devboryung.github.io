---
title: "2020년 11월 26일"
excerpt: "HTML-2"
search: true
categories: 
  - Academy
tags: 
  - HTML
toc: true
---

## 07_이미지 관련 태그
<br/>

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>07_이미지 관련 태그</title>
</head>
<body>
    <pre>
        &lt;img&gt; 태그
        - 웹 페이지에 사진이나 그림을 삽입할 때 사용하는 태그
    </pre>
    <!--
        <img src="이미지의 경로" alt="이미지 설명구문" width="폭" height="높이">
        src : 삽입할 이미지의 경로(로컬 경로 또는 url)
        alt : 이미지를 설명해주는 텍스트를 작성하는 속성
              이미지가 출력이 되지 않는 경우 화면에 표시됨.
              + 웹 접근성(스크린 리더를 통해 이미지의 설명을 읽어들임)
    -->

    <h3>이미지 불러오기</h3>
    <img src="sample/image/river1.PNG" width ="200px" height ="150"><br>
    <img src="https://postfiles.pstatic.net/MjAyMDA5MDRfMTc2/MDAxNTk5MTkyNDcxMzc3.dxI7xjyRn5O6qDt1m4wkNcCkp3mHdku4E3OPIOz6geog.nNzd1py1-A-xdFfeiCFPPiXZzNuT6bP3hnEBHmhDyXEg.JPEG.yebin0429/116435263_3640058069431569_2622091731490368493_o.jpg?type=w773" width ="200px" height ="150">



    <hr>

    <h3>alt속성 확인하기</h3>
    <img src="sample/image/river1.jgp" alt="이미지가 없습니다.">

    <hr>

    <h3>width/height의 고정 크기 단위와 가변 크기 단위</h3>

    <h4>고정 크기 단위 : 화면(창)의 사이즈가 변하여도 지정된 요소의 크기가 
        고정되게 하는 단위(px/픽셀)
    </h4>

    <img src="sample/image/flower1.PNG" width ="200px" height ="150">
    <img src="sample/image/flower2.PNG" width ="200px" height ="150">
    <img src="sample/image/flower3.PNG" width ="200px" height ="150">
    <img src="sample/image/flower4.PNG" width ="200px" height ="150">
    <img src="sample/image/flower5.PNG" width ="200px" height ="150">

    <hr>
    <h4>가변 크기 단위: 화면(창)의 사이즈 또는 기준이된 요소의 사이즈가 
        변하게 되면 해당 요소의 크기도 변하게 됨.(%, em,rm,rem)
    </h4>
    <img src="sample/image/flower1.PNG" width ="15%" height ="150">
    <img src="sample/image/flower2.PNG" width ="15%" height ="150">
    <img src="sample/image/flower3.PNG" width ="15%" height ="150">
    <img src="sample/image/flower4.PNG" width ="15%" height ="150">
    <img src="sample/image/flower5.PNG" width ="15%" height ="150">

    <h4>img태그는 inline 형식이다.</h4>

</body>
</html>
```
<br/>
#출력 화면<br/>


![html7-1](https://user-images.githubusercontent.com/73421820/100330563-74ad6f80-3012-11eb-8008-55905710fe71.PNG)
![html7-2](https://user-images.githubusercontent.com/73421820/100330566-75460600-3012-11eb-98c3-d0e070142186.PNG)

<br/><br/>

<br/>

## 08_미디어 관련 태그
<br/>

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>08_미디어 관련 태그</title>
</head>
<body>

    <!-- 미디어 관련 태그는 HTML5부터 지원함.
        (이전에는 플러그인, ActiveX를 사용했어야 함.-->
    <h2>오디오 관련 태그</h2>
    <!--
        <audio> 태그
        - src 속성 : 오디오파일 경로 또는 url
        - controls : 재생도구  출력 지정
        - autoplay : 자동 재생 여부 지정(모바일x, 크롬x)
        - loop : 반복 재생 여부 지정
        - preload : 오디오파일을 미리 로드할지 지정
        (auto : 전부 다 로드 / metadata : 정보만 로드 / none : 로드X)
    -->
    <audio src="sample/audio/major.mp3" 
    controls loop preload="auto"></audio>

    <hr>

    <h2>비디오 관련 태그</h2>
    <!--
        <video> 태그
        - src 속성 : 비디오파일 경로 또는 url
        - controls : 재생도구  출력 지정
        - autoplay : 자동 재생 여부 지정(모바일x, 크롬x)
        - loop : 반복 재생 여부 지정
        - preload : 비디오파일을 미리 로드할지 지정
        (auto : 전부 다 로드 / metadata : 정보만 로드 / none : 로드X)
        
        - width / height : 넓이, 높이
        - poster : 재생 전 출력할 이미지 경로 지정
    -->

    <video src="sample/video/video1.mp4"
    controls poster="sample/image/flower1.PNG"></video>

</body>
</html>
```
<br/>
#출력 화면<br/>


![html8](https://user-images.githubusercontent.com/73421820/100330714-a8889500-3012-11eb-9ecf-7727729fbe87.PNG)


<br/><br/>

## 09_하이퍼링크 관련 태그
<br/>

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>09_하이퍼링크 관련 태그</title>
</head>
<body>
    <h1>하이퍼 링크</h1>
    <pre>
    하이퍼링크 기능은 웹 문서가 다른 문서와 구분되는 가장 핵심적인 기술
    클릭을 통해 연결된 곳으로 이동하여 사용을 편리하게 해주는 기능

    텍스트 또는 이미지 등을 클릭하여 지정된 링크로 이동하게 함.
    + 페이지 내 이동도 가능
    </pre>

    <h3>a태그를 이용한 하이퍼링크</h3>
    <!-- anchor : 닻, 고정시키다, 묶다-->
    <ul>
        <li><a href=01_글자관련태그.html>01_글자 관련 태그</a></li>
        <li><a href=02_목록관련태그.html>02_목록관련태그.html</a></li>
        <li><a href=03_글자,목록태그연습문제.html>03_글자,목록태그 연습문제.html</a></li>
        <li><a href=04_표관련태그.html>04_표관련태그.html</a></li>
        <li><a href=05_표관련태그연습문제.html>01_글자 관련 태그 연습문제.html</a></li>
        <li><a href=06_영역관련태그.html>06_영역관련태그.html</a></li>
        <li><a href=07_이미지관련태그.html>07_이미지관련태그.html</a></li>
        <li><a href=08_미디어관련태그.html>08_미디어관련태그.html</a></li>
    </ul>

    <h3>웹 페이지 링크 연결</h3>
    <!--target="_blank" : 새 창으로 열림,  target="_self" = 현재 창에서 열림 -->
    <ul>
        <li><a href="https://www.naver.com" target="_blank">네이버</a></li>
        <li><a href="https://www.google.com" target="_self">구글</a></li>
    </ul>

    <h3>이미지 클릭 시 링크 이동</h3>
    <a href="https://www.w3schools.com/">
    <img src="https://search.pstatic.net/common/?src=http%3A%2F%2Fblogfiles.naver.net%2FMjAxOTEyMTVfMjY2%2FMDAxNTc2MzU2Nzk3Njgy.x2BiAI78XeVe4fJBO2d27h05AmLklUNiT5o91t1otB8g.-hHctAMrL2pXbPvv_21Ciw5vwY7LLiuEN-kog2vfvjsg.PNG.jengsuk%2F20191215_054804.png&type=sc960_832" width="150px" height="150px">
    </a>

    <hr>

    <h3>페이지 내에서 점프하는 a태그 만들기</h3>
    <ul id="menu">
        <li><a href="#content1">내용 1번</a></li>
        <li><a href="#content2">내용 2번</a></li>
        <li><a href="#content3">내용 3번</a></li>
    </ul>


    <h4 id="content1"> 내용 1번</h4>
    <pre>
        👨 🦳찌개가 왜이렇게 짜🥘⁉ 짜긴 뭐강 짱 👵 
        엄마! 🐷이거 뭐에요?🤷 ♂ 고구마호박👵 
        고구마호박이🍠 뭐에요?🤷 ♂ 고구마호박 모올랑😒 
        호박 맛나는 💛노오란 고구망🍠 
        👩🏻고구마호박이 아니라 ✖ 호박고구마요 어머님 😂 
        움~ 🥄👅 맛있다 🤭 마트 다녀오셨어요? 👀 
        👵영기엄마가 텃밭👩 🌾에서 고구마호ㅂ🤐 호박고구마요어머님.
        🍠 그래...!!😰 호굼!🤭 👩🏻푸흡... 호구마요?
        🤪 호.박.고.구.ㅁ🤐 호.박.고.구.마. 🍠 호박고구마‼ 호❗박❗고❗구❗마 이제됐냐🤬 
        ...흡... 흑... 😭 헝헝헝.... 👺 이 할망구가 왜이래 밥상에서‼ .... 😢    
    </pre>
    <a href="#menu">메뉴로 돌아가기</a>

    <h4 id="content2"> 내용 2번</h4>
    <pre>
        췤 췤 암더코리안 타카라씨 히파몬 나부라써 뻬이브라써 촤브라써 쿠자스브러 
        게인자쓰러 비트르비트르 제껴봐라 서브미션 솨피아또빠르 운따르따릉 쫘쪼빠따 
        빠쏘바야 파스씨야 흐름닦아 부름닦아 프리빼까 킬로빠짜 표파까봐라 계쏙해나바라 
        계속 계속해봐라 내새까 매끄러부러 케로푸츤 미췬 헤라포스 르 레미키스브러 
        오닥까락 가락기 기트켠 기타큰혓바닥인더따웃  
        ⊂_ヽ
        　 ＼＼ Λ＿Λ
        　　 ＼( ‘ㅅ’ ) 두둠칫
        　　　 >　⌒ヽ
        　　　/ 　 へ＼
        　　 /　　/　＼＼
        　　 ﾚ　ノ　　 ヽ_つ
        　　/　/두둠칫
        　 /　/|
        　(　(ヽ
        　|　|、＼
        　| 丿 ＼ ⌒)
        　| |　　) /
        `ノ )　　Lﾉ
    </pre>
    <a href="#menu">메뉴로 돌아가기</a>

    <h4 id="content3"> 내용 3번</h4>
    <pre>
        📯📯📯뿌〰아이고 깜짝이야😵어?핑핑아🐌오늘이 무슨요일이야? 
        먀~🐌월요일?😲아~월요일🎷월요일🌝좋아〰🙆 ♀최고로 좋아〰🙆 
        ♀난🧀일할때🍔제일 멋지지😎오늘부터💪열심히🧠할거야 오좋아💩
        월요일 좋아〰🛁같이 불러🎙핑핑아🐌냔냔냐냐냐〰냔냔냐냐냐〰
        월요일🎶월요일🎶월요일🥁월요일 좋아〰🦑제발 좀 조용히해🤬
        월요일이 좋아서 난리떠는🤸 ♀멍청이는 이세상에 너뿐🧀일꺼야🗿⭐
        월요일 좋아🦑맙소사🤯진짜 맛있는 날이야💨🦑제발 그만 해💦
        냠냠 게살버거 넌 세🍔개🍔먹어🍔오예 노래하자🎤내월요일🧀좋아〰월요일🦑좋아⭐〰
    </pre>
    <a href="#menu">메뉴로 돌아가기</a>

</body>
</html>

```
<br/>
#출력 화면<br/>


![html9-1](https://user-images.githubusercontent.com/73421820/100330949-ec7b9a00-3012-11eb-8cde-53b2ea329539.PNG)
![html9-2](https://user-images.githubusercontent.com/73421820/100330951-ed143080-3012-11eb-902b-c1a939e9b716.PNG)


<br/><br/>

## 10_폼 관련 태그
<br/>

```html
<!DOCTYPE html>
<html lang="ko">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>10_폼 관련 태그</title>
</head>

<body>
    <h1>폼 관련 태그</h1>
    <p>
        input 태그 : html에서 사용자가 입력할 수 있는 양식을 제공하는 태그.<br>
        form 태그 : 내부에 작성된 input태그를 통해 값을 입력 받고 ,
        해당 값을 서버로 전달하는 역할을 하는 태그
    </p>
    <pre>
    form 태그 속성
    - action : form태그 내부에 입력된 데이터를 전달하여 처리할 페이지 또는 서버 URL 지정.
    - method : 데이터 전송 방식 지정
        1) get : url창(주소)을 이용해 데이터를 전달하는 방식.
        2) post : http request객체의 body부분에 데이터를 담아 전달하는 방식.
    - name : 해당 form태그의 고유한 이름을 지정하는 태그.
            script에서 form태그들을 구분하는 용도로 사용.    
    - target : action속성에 작성된 주소를 어떤 방식으로 열것인지 지정        
    </pre>

    <hr>

    <form action="09_하이퍼링크관련태그.html" method="get" target="_blank">
        <input type="text" size="20" name="search">
        <input type="submit" value="검색">
        <!-- submit : 제출하다 -->
    </form>
    <!--
        form태그 내부에 작성된 submit 버튼 클릭 시
        form태그 action 속성에 작성된 주소로
        input 태그에 작성된 값이 name 속성값 = 입력된 값 형태로 전달됨.
        + method가 get방식으로 설정되어 있으므로 주소창에 전달되는 데이터가 작성됨.
    -->

    <hr>

    <h4>fieldset 태그 : 요소를 그룹으로 묶는 태그</h4>
    <h4>legend 태그 : fieldset으로 묶인 그룹에 명칭을 부여하는 태그</h4>

    <!-- lable태그 : input태그의 명칭을 지정하는 태그-->
    <form>
        <fieldset>
            <label>입력 1: </label> <input type="text"><br>
            <label>입력 2: </label> <input type="text"><br>
            <label>입력 3: </label> <input type="text"><br>
        </fieldset>

        <fieldset>
            <legend>동방한솔</legend>
            <label>입력 1: </label> <input type="text"><br>
            <label>입력 2: </label> <input type="text"><br>
            <label>입력 3: </label> <input type="text"><br>
            <label>입력 4: </label> <input type="text"><br>
        </fieldset>
    </form>

    <hr>

    <h2>text 관련 input 태그</h2>
    <form method="get">
        <!--action 속성이 없을 경우 현재 페이지-->
        <h3> type ="text"</h3>
        <p>한 줄 짜리 텍스트를 입력할 수 있는 텍스트 박스</p>
        <label for="userId"> 아이디 : </label>
        <input type="text" id="userId" name="id" size="20" maxlength="10" placeholder="아이디를 입력하세요" value="user01">
        <!-- input태그의 name 속성은 데이터 전달 시 key값으로 활용됨-->
        <!-- label의 for 속성값과 input의 id속성값이 같으면 두 태그가 연결됨.-->

        <h3>type = "password"</h3>
        <p>입력된 텍스트를 부라우저별로 지정된 문자로 가리게 하는 타입</p>
        <label for="userPwd">비밀번호 : </label>
        <input type="password" id="userPwd" name="pwd" placeholder="비밀번호를 입력하세요">

        <hr>
        <h3>기타 text 타입</h3>
        <p>겉모습은 type="text"와 비슷하지만
            각각에 맞는 정보를 입력받거나, 유효성 검사 기능을 제공해주는 태그<br>
            search, url, email, tel 등이 있음.
        </p>
        <lable for="search">검색 : </lable>
        <input type="search" id="search" name="search"><br>

        <lable for="homepage">홈페이지 주소 : </lable>
        <input type="url" id="homepage" name="homepage" value="https://"><br>

        <lable for="email">이메일 : </lable>
        <input type="email" id="email" name="email"><br>

        <lable for="tel">전화번호 : </lable>
        <input type="tel" id="tel" name="tel"><br>

        <input type="submit" value="제출">
        <input type="reset" value="초기화">
        <!--reset : 해당 form 내부에 작성된 모든 input 값을 초기화-->


    </form>

    <hr>

    <h2>숫자 관련 input 태그</h2>
    <form method="get">
        <h3>type = "number"</h3>
        <p>
            입력창에 숫자만 입력할 수 있음.<br>
            브라우저의 종류에 따라 스핀박스가 표시되기도 함.
        </p>
        수량 : <input type="number" name="amount" min="0" max="100" step="10" value="10">

        <h3>type = "range"</h3>
        <p>
            슬라이드bar를 이용해 숫자 지정
        </p>
        <input type="range" name="point" min="0" max="100" step="10" value="10">

        <input type="submit" value="제출">
        <input type="reset" value="초기화">
    </form>

    <hr>
    <h2>날짜 관련 태그</h2>
    <form method="get">
        <h3>date / month / week / time / datetime / datetime-local</h3>

        date : <input type="date" name="date"><br>
        month : <input type="month" name="month"><br>
        week : <input type="week" name="week"><br>
        time : <input type="time" name="time"><br>
        datetime : <input type="datetime" name="datetime"><br>
        datetime-local : <input type="datetime-local" name="datetime-local"><br>

        <input type="submit" value="제출">
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        <!--&nbsp; : 띄어쓰기-->
        <input type="reset" value="초기화">
    </form>

    <hr>
    <h2>라디오 버튼, 체크박스</h2>
    <p>
        태그를 여러 개 선언하여 하나의 용도로 사용하는 태그.<br>
        하나의 용도로 사용할 라디오 또는 체크박스의 name속성값은 모두 동일해야함
    </p>

    <form method="GET">
        <h3>type="radio"</h3>
        성별 :
        <label for="male">남</label>
        <input type="radio" id="male" value="M" name="gender">
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        <label for="female">여</label>
        <input type="radio" id="female" value="F" name="gender" checked >

        <h3>type = "checkbox"</h3>
        [취미] <br>
        <label for="baseball">야구</label>
        <input type="checkbox" id="baseball" value="baseball" name="hobby">

        <label for="basketball">농구</label>
        <input type="checkbox" id="basketball" value="basketball" name="hobby">

        <label for="football">축구</label>
        <input type="checkbox" id="football" value="football" name="hobby" checked>

        <!--checkbox의 name 속성이 모두 같을 경우
            데이터를 전달받은 서버측에서 배열의 형태로 받아들임.
        -->

        <input type="submit" value="제출">
    </form>

    <hr>
    <hr>

    <h2>추가 input type + 입력 관련 태그</h2>
    <form>
        <!--method 미작성 시 기본값 get-->
        <h3>type="file"</h3>
        파일 선택 : <input type="file" name="upFile"><br>

        <h3>type="color"</h3>
        색상 선택 : <input type="color" name="color"><br>

        <h3>type="hidden"</h3>
        <p>화면에 값이 노출되지 않지만 코드상에는 존재하게 하는 타입</p>
        <input type="hidden" name="val" value="test">

        <input type="submit" value="제출">

        <h3>버튼 관련 타입</h3>
        <input type="submit" value="제출">
        <!--form태그 내용을 action 주소로 제출-->

        <input type="reset" value="초기화">
        <!--form태그 내부 input 데이터를 초기화-->

        <input type="button" value="일반버튼">
        <!-- 아무런 기능이 없는 버튼
            -> Javascript를 이용하여
                버튼 클릭 이벤트를 부여하는 용도로 사용
        -->

    </form>

    <hr>
    <form>


        <h3>select와 option태그</h3>
        전화번호 :
        <select name="phone">
            <option>010</option>
            <option>011</option>
            <option>017</option>
            <option>019</option>
            <option selected>없음</option>
        </select>

        <h3>textarea 태그</h3>
        <p>text를 여러 줄 입력할 수 있는 태그</p>
        <textarea name="ta" rows="10" cols="30" style="resize:none">
        안녕하세요
안녕하세요          
                안녕하세요
        </textarea>
        <!-- %0D%0A = \r\n   개행문자 -->

        <h3>button 태그</h3>
        <p>input의 버튼관련 타입을 쉽게 사용할 수 있도록 만들어진 태그</p>
        <button type="submit">제출</button>
        <button type="reset">초기화</button>
        <button type="button">일반버튼</button>
            
    </form>

</body>

```
<br/>
#출력 화면<br/>


![html10-1](https://user-images.githubusercontent.com/73421820/100331695-d5897780-3013-11eb-875a-dfe484459264.PNG)
![html10-2](https://user-images.githubusercontent.com/73421820/100331697-d6220e00-3013-11eb-80d6-632f4e8da128.PNG)
![html10-3](https://user-images.githubusercontent.com/73421820/100331700-d6baa480-3013-11eb-9154-6f083e2975f2.PNG)
![html10-4](https://user-images.githubusercontent.com/73421820/100331708-d8846800-3013-11eb-8d3b-8378e2dfe981.PNG)


<br/><br/>