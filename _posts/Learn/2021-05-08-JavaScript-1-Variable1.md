---
title: "[JavaScript]JS의 데이터 타입"
excerpt: "JavaScript"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---



## 변수(Variable)

변경될 수 있는 값<br>
Mutable DataType<br><br>
메모리의 값을 읽고/쓰는 것이 가능하다.<br>

### let (ES6에 추가됨)
JavaScript에서 변수를 생성할 때 `let` 키워드를 사용해서 선언한다.<br>
<br>

```javascript
let name = 'boryung';
console.log(name);
name = 'hello';
console.log(name);
```

<br>

let 키워드를 이용해 name 변수를 선언과 동시에 'boryung'이라는 값을 할당한다<br>
name이라는 변수에 'hello'라는 값을 다시 할당한다.<br>

![image](https://user-images.githubusercontent.com/73421820/117480432-8ec58380-af9c-11eb-8609-4cf7dcbf4dce.png)

<br>
<br>

#### 메모리

애플리케이션이 실행되면 애플리케이션마다  사용 가능한 메모리가 제한적으로 할당된다.<br>
변수를 정의하면 메모리 한 칸을 가리키는 포인터가 생기고, 그 포인터가 가리키는 메모리 칸에 'boryung'이라는 값을 저장할 수 있다.<br>
추후에 name 변수가 가리키고 있는 곳에 다른 값을 저장할 수도 있다.<br><br>


#### 지역 변수(Block Scope)

```javascript
{
  let name = 'boryung';
  console.log(name);
  name = 'hello';
  console.log(name);
}
console.log(name); // 출력되지 않음
```

<br>

`{}` 블럭 안에서 선언된 함수는 해당 블럭 안에서만 사용 가능하며, 블럭 밖에서 접근할 수 없다.<br><br>

#### 전역 변수(Global Scope)

```javascript
var globalName = 'global name';

{
  let name = 'boryung';
  console.log(name);
  name = 'hello';
  console.log(name);
  console.log(globalName); //global name출력
}

console.log(globalName);  //global name출력
```

<br>

`{}` 블럭 안에 들어있지 않고, 파일 안에 바로 정의하는 변수를 전역 변수라고 한다.<br>
전역 변수는 어느 곳에서나 접근 가능하다.<br><br>
전역 변수는 애플리케이션이 실행되는 순간부터 끝날 때까지 항상 메모리에 탑재되기 때문에 최소한으로 사용하는 것이 좋으며, 가능하면 클래스,함수,if,for 등 필요한 곳에서 정의해서 사용하는 것이 좋다.<br><br><br>


### var(ES6 전에 사용)

var의 문제점!<br>

대부분 프로그래밍 언어에서 변수 선언 후 값을 할당하는 것이 정상이다.<br>
하지만 var는 선언도 하기 전 값 할당이 가능하고, 값 할당 전에도 console로 출력이 된다.<br>
출력값은 `undefined`로 나오는데, undefined는 변수는 정의되어 있지만, 값은 없는 정의되지 않음을 뜻한다.<br><br>

이런 문제를 var hoisting이라고 한다<br>
hoisting: 어디에 선언했는지와 상관없이 항상 선언을 제일 위로 끌어올려 주는 것이다.<br>
파일 안에 var로 선언된 변수가 있다면 맨 첫 줄에서 console로 출력해도 undefined 값이 출력된다.<br>

또한, var는 지역 변수가 없기 때문에 블럭을 무시한다.<br>
즉, 블럭 안에서 선언한 함수를 블럭 밖에서도 접근할 수 있다.<br><br><br>


## 상수(Constant)

한 번 할당하면 값이 절대 변하지 않는 값.<br>
Immutable DataType<br><br>
const 키워드를 사용한다.<br>
메모리의 값을 읽을 수 있다.<br>

- Immutable data types / Mutable data types
premitive types + frozen objects : 데이터를 변경할 수 없음.<br>
all objects : object 선언 후 내부의 값 변경 가능(Ex. 배열)<br><br>


### 상수를 사용하는 이유

1. 보안
한 번 작성한 값을 해커들이 코드를 삽입해 값을 변경하지 못하도록 한다. 
2. 스레드 안전성
애플리케이션이 실행되면 하나의 프로세스가 할당되며, 프로세스 안에서 다양한 스레드가 동시에 작업하며 애플리케이션을 효율적으로 실행할 수 있도록 도와준다.<br> 다양한 스레드가 동시에 변수에 접근해서 값을 변경할 수 있는데, 동시에 값을 변경하면 위험이 높아 값이 변경되지 않는 게 좋다.<br>  
3. 실수 방지
추후에 있을 코드 변경 시 실수를 방지할 수 있다.<br>  <br><br>  




## 변수 자료형(Variable types)

- 기본 자료형 (primitive)
number, string, boolean, null, undefined, symbol<br>

  - number
  모든 숫자 자료형<br>

  ```javascript
  const infinity = 1/0;
  const negativeInfinity = -1/0;
  const nAn = 'not a number' / 2;
  ```

  숫자를 0으로 나눌 경우 무한대의 숫자 값이 나온다.<br>그것을 infinity, 숫자가 음수일 경우 -infinity값이 나오게 된다.<br>
  숫자가 아닌 것을 숫자로 나누게 되면 NaN이 나온다.<br><br>

  - symbol
  고유한 식별자가 필요할 때 사용<br>
  ```javascript
  const symbol1 = Symbol('id');
  const symbol2 = Symbol('id');
  console.log(symbol1 === symbol2); //false
  ```
  string을 이용해 식별할 경우 다른 파일에서 동일한 String을 썼을 때 동일한 식별자로 간주된다.<br>
  Symbol은 'id'라는 동일한 String을 작성했어도 두 가지 Symbol을 다르게 취급한다.<br><br>
  

  ```javascript
  const gSymbol1 = Symbol.for('id');
  const gSymbol2 = Symbol.for('id');
  console.log(gSymbol1 === gSymbol2 ); //true
  ```
  위 방법은 String이 똑같다면 동일한 Symbol을 만드는 방법이다.<br><br>

  Symbol은 출력할 때 `.description`을 이용해서 String으로 변환 후 출력해야 된다.<br>

  ```javascript
  console.log(`value: ${symbol1.description}`);
  ```
<br><br>
  

- 참조 자료형 (object)
- function(first-class function)
함수가 변수에 할당이 가능하고, 함수의 인자로 전달, return타입으로 지정 가능.


<br><br>

## Dynamic typing

변수를 선언할 때 어떤 타입인지 선언하지 않고, 프로그램이 동작할 때 할당된 값에 따라 변경될 수 있다.<br>


```javascript
const text = 'hello'; // type : string
text = 1; // type : number
text = '7' + 5; // 75 type : string (문자열 + 숫자 = 문자열)
text = '8' / '2'; // 4  type : number
```

JS는 런타임에서 타입이 정해지기 때문에 에러가 발생하는 경우가 많다.<br>
그래서 TypeScript가 출시되었다.(자바스크립트 위에 타입이 올려진 언어이다.)<br>







참고 영상 : [엘리님Youtube](https://www.youtube.com/watch?v=OCCpGh4ujb8&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=3)

<br><br>