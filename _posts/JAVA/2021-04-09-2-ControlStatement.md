---
title: "[Do it! 자바 프로그래밍 입문]제어문"
excerpt: "2021-04-09"
categories: 
  - JAVA
tags: 
  - Learn
  - JAVA
  - Inflearn
  - DoIt
toc: true
---

<br>

[Do it!강의](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9E%85%EB%AC%B8/dashboard)

<br>

## 조건문
주어진 조건에 따라 수행문이 실행되도록 프로그래밍<br>

### if문, if-else문
* 간단한 if-else문은 조건 연산자로 구별할 수 있다.<br>
> `조건문`
  if(a>b) max=a;
  else max =b; 
  `조건 연산자`
  max = (a>b)?a:b;

<br><br>

## switch-case문
조건식의 결과가 정수 또는 문자열의 값이고, 그 값에 따라 수행문이 결정될 때.<br>
break;를 작성하지 않으면 case를 만족한 후 break;를 만나기 전의 모든 코드가 실행된다.<br>
같은 조건일 경우 case를 여러개 쓸 수 있다.<br>

```java
int month = 5;
int day;
switch(month)
    case 1: 
    case 3: 
    case 5: 
       day = 31; break;
    case 2:
        day = 28; break;   
    case 4: case 6:
        day = 30; break;
    default : break;
```

<br><br>



## 반복문
주어진 조건이 만족 할 때까지 수행문을 반복적으로 수행<br>


### while
조건이 참인 동안 수행문을 반복해서 수행<br>
`while(true)` : 무한 루프를 의미한다.<br>

<br>


### do-while
무조건 한 번 실행 후 조건을 판단한다.<br>
조건이 참인 동안 수행문을 반복해서 수행<br>

<br>

### for
조건이 횟수인 경우에 사용<br>
초기화식;조건식;증감식을 한 번에 작성<br>
변수에 초기화 해준 후 초기화식 생략 가능, 조건식 생략 후 수행문에 작성 가능하지만.. 이렇게 작성하지 않음<br>
초기화 할 값, 증가값이 2개라면 `,`로 이용해서 구분한다. 하지만 조건식이 조금 복잡해진다. <br>


`for( ; ; )` : 무한 루프를 의미한다.<br>
하지만 무한 루프는 while을 이용해서 사용한다.<br>

<br>

### 각 반복문의 쓰임

- while문 : 하나의 조건에 대해 반복 수행이 이루어질 때 사용. 조건이 맞지 않으면 반복문이 수행되지 않음
- do-while문 : 하나의 조건에 대해 반복 수행이 이우러질 때 사용. 단, 수행문이 반드시 한 번 이상 수행됨
- for문 : 수의 특정 범위, 횟수와 관련한 반복 수행에서 주로 사용

<br><br>

### 중첩된 반복문
반복문 내부에 또 반복문이 사용되는 것

<br><br>


### continue문
반복문과 함께 쓰이며, 반복문 내부 continue문을 만나면 이후 반복되는 부분을 수행하지 않고 조건식이나 증감식을 수행한다.<br>

```java
int total = 0;
for(int i=0; i<10; i++){
    if(i%2==0){ 
        continue;
    }
    total += i;
}
```

i가 짝수인 경우엔 total += i를 수행하지 않고, 증감식을 수행한다.<br>
결국 total은 홀수들의 합이다.<br>
<br>


### break문
반복문에서 break문을 만나면 더 이상 반복을 수행하지 않고 반복문을 빠져 나온다.<br>
중첩된 반복문 내부에 있는 경우, 가장 가까운 반복문 하나만 빠져 나온다.<br>

<br>


