---
title: "[JavaScript]연산"
excerpt: "JavaScript"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---


## 문자열 연결

```javascript
console.log('my' + ' cat'); // my cat
console.log('1' + 2); // 12
console.log(`string literals: 1+2 = ${1+2}`); //string literals: 1+2 = 3
// ` `를 사용해서 출력할 경우, ``안의 모든 내용이 문자열로 변경돼서 출력된다. ${}를 이용하면 변수 값을 계산해서 String으로 포함해준다.
```
<br><br>

## 산수 연산자

```javascript
console.log(1 + 1); // 2
console.log(1 - 1); // 0
console.log(1 / 1); // 1
console.log(1 * 1); // 1
console.log(5 % 2); // 1
console.log(2 ** 3); // 8
```
<br>

`**`은 지수를 의미한다<br>
2**3 = 2의 3승<br>

<br><br>

## 증가, 감소연산자(++,--)

```javascript
let counter = 2;
const preIncrement = ++counter; // 전위 증가
const postIncrement = counter++; // 후위 증가
const preDecrement = --counter; // 전위 감소
const postDecrement = counter--; // 후위 감소
```


<br><br>

## 대입 연산자(=)

```javascript
let x=3;
let y=6;
x += y;
x -= y;
x *= y;
x /= y;
```

<br><br>

## 비교 연산자

A  `< , > , <= , >=`  B



<br><br>

## 논리 연산자

|| (or), && (and), !(not) <br>

- || (or)

모든 값 중 하나라도 true 값이 있다면 true를 반환한다.<br>
첫 값이 true가 나오면 더 이상 실행하지 않고 무조건 true를 반환한다.<br>
그렇기 때문에 가장 간단한 연산을 맨 처음에 두고, 복잡한 연산은 가장 마지막에 두는 것이 효율적이다.<br>


- && (and)

모든 값이 true여야 true를 반환한다.<br>
첫 값이 false라면 더이상 실행하지 않고 무조건 false를 반환한다.<br>
그렇기 때문에 가장 간단한 연산을 맨 처음에 두고, 복잡한 연산은 가장 마지막에 두는 것이 효율적이다.<br><br>

object가 null 이면 false를 반환하기 때문에 &&연산자는 null 체크 시에도 많이 사용된다.<br>

- ! (not)

값을 반대로 바꿔준다.<br>


<br><br>

## 비교 연산자(==,!=, ===, !==)


```javascript
const stringFive = '5';
const numberFive = 5;

console.log(stringFive == numberFive); // true
console.log(stringFive != numberFive); // false

console.log(stringFive === numberFive); // false
console.log(stringFive !== numberFive); // true
```

<br>

==,!== : 타입을 변경해서 안의 내용만 비교한다. <br>
===, !== : 타입을 그대로 두고 비교한다. <br>
코딩할 때 웬만하면 ===,!== 를 이용해서 비교하는 게 좋다.<br>



<br><br>


## if , else if, else 문

```javascript
const name = 'boryung';

if(name==='boryung'){
    console.log('Welcome, boryung');
}else if(name==='coder'){
    console.log('Welcome, coder');
}else{
    console.log('unknown');
}
```

if()의 조건을 만족하면 {}안의 코드를 수행한다.<br>
if() 조건을 만족하지 않다면 else if()의 조건을 확인하고, 만족하면 {}안의 코드를 수행한다.<br>
if,else if 모두 만족하지 않는다면 else의 {}안의 코드를 수행한다.<br>

<br><br>

## 삼 항 연산자(?)

```javascript
console.log(name==='boryung' ? 'yes' : 'no');
```

?앞의 조건이 true라면 `yes` : no <br>
조건이 false라면 yes : `no`<br>


삼 항 연산자를 많이 사용하면 코드의 가독성이 떨어지게 된다.<br>

<br><br>

## Switch문

```javascript
const browser = 'IE';

switch (browser){
    case 'IE' :
        console.log('go away!');
        break;
    case 'Chrome' :
        console.log('love you!');
        break;
    default :
        console.log('same all');
        break;
}
```
<br>

switch() 의 값이랑 동일한 case를 수행한다.<br>
break가 없으면 동일한 case부터 break가 있는 곳까지 수행된다.<br>
일치하는 값이 없으면 default가 수행된다.<br>

<br><br>

## while문

```javascript
let i = 3;
while(i>0){
    console.log(`while: ${i}`);
    i--;
}
```
<br>

while()의 조건이 false가 될 때까지 반복한다.<br>

<br><br>


## do-while문

```javascript
let i = 0;
do{
    console.log(`do while: ${i}`);
    i--;
}while(i>0);
```
<br>

무조건 반복문을 한번 실행 후 조건을 판단한다.<br>
<br><br>

### for문

for(초기식;조건식;증감식);<br>

처음 초기식으로 시작한 후 , 반복이 될 때마다 증감식을 통해 값을 변경하고 조건을 검사한다.<br>
조건이 true라면 반복하고, false라면 종료한다.<br>

for문 내부에 for문을 작성하게 되면 O(n²)의 복잡도를 갖게 된다.<br>







참고 영상 : [엘리님Youtube](https://www.youtube.com/watch?v=YBjufjBaxHo&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=4)

<br><br>