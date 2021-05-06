---
title: "[JavaScript]JS동작 순서,script async와 defer의 차이"
excerpt: "JavaScript"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---

<br>

## HTML에 자바스크립트를 포함할 때 동작 순서

사용자가 HTML 파일을 다운로드 받았을 때 브라우저가 위에서부터 한 줄씩 분석(parsing)한다. <br>
분석한 것을 CSS와 병합해서 DOM요소로 변환한다.<br>
<br>

### head안에 스크립트 포함시키기


```javascript
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <script src="main.js"></script>
    </head>
    <body>
        <div></div>
    </body>
</html>
```

<br>

- 동작 과정



위에서부터 한 줄씩 분석하다가 script 코드가 보이면 HTML parsing을 잠시 멈추고 필요한 javaScript 파일을 서버에서 다운받아서 실행한 후 다시 HTML을 parsing한다. <br>

단점 : js파일이 크거나 인터넷이 느릴 경우 웹 사이트를 보기까지 많은 시간이 소요된다.<br>



### body 가장 마지막에 스크립트 포함시키기

```javascript
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <div></div>
        <script src="main.js"></script>
    </body>
</html>
```

<br>

- 동작 과정

HTML을 전체적으로 parsing 한 후 페이지가 준비된 다음에 필요한 javaScript파일을 서버에서 다운받아서 실행한다.<br>

단점 : js를 받기 전에 기본적인 html 페이지가 보여지지만 js에 의존적인 페이지인 경우 사용자가 정상적인 페이지를 보기 위해서 대기해야 된다. <br>

<br>

### head안에 async 속성값을 이용해 스크립트 포함시키기

async = boolean 타입이기 때문에 선언만 해도 true값을 가진다.

```javascript
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <script asyn src="main.js"></script>
    </head>
    <body>
        <div></div>
    </body>
</html>
```

<br>

- 동작 과정

HTML을 parsing하다가 asyn 속성이 있는 js를 만나면 병렬로 js를 다운로드 받으며 HTML parsing을 계속한다.<br> js가 다운로드 완료되면 parsing을 멈추고 다운로드된 js파일을 실행한다. js파일 실행이 완료되면 나머지 html을 parsing한다. <br>


장점 : js다운이 html parsing과 병렬적으로 이뤄지기 때문에 다운받는 시간을 절약할 수 있다. <br>
단점 : js실행할 때 필요한 html이 그 시점에 parsing되지 않을 수가 있다. js를 실행할 때 html parsing이 멈추기 때문에 사용자가 페이지를 보는 시간이 소요될 수 있다.<br>

<br>

### head안에 defer 속성값을 이용해 스크립트 포함시키기


```javascript
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <script defer src="main.js"></script>
    </head>
    <body>
        <div></div>
    </body>
</html>
```

<br>

- 동작 과정

HTML을 parsing하다가 defer 속성이 있는 js를 만나면 병렬로 js를 다운로드 받으며 나머지 HTML을 끝까지 parsing한다.<br> HTML parsing이 끝나고 난 후 다운로드 된 js를 실행한다. <br>


장점 : HTML이 parsing하는 동안 필요한 js를 모두 다운로드 받아 놓고, html parsing을 완료해 사용자에게 페이지를 보여준 후 바로 js를 실행하기 때문에 가장 좋은 방법이다.<br>

<br>


### async와 defer의 차이


- async

작성된 순서와 상관없이 먼저 다운로드 된 js를 실행하기 때문에 순서에 의존하는 코드인 경우 오류가 발생할 수 있다.


- defer

필요한 js를 모두 다운로드 후, HTML parsing이 완료된 후 코드 순서대로 실행하기 때문에 개발자가 정의한 순서를 지켜서 실행된다.

<br><br>



참고 영상 : [엘리님Youtube](https://www.youtube.com/watch?v=okHGRlgR8ps)



<br>
<br>