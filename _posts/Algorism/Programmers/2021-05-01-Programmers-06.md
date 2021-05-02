---
title: "[프로그래머스]짝수와 홀수(JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---

### 문제 설명

정수 num이 짝수일 경우 "Even"을 반환하고 <br>홀수인 경우 "Odd"를 반환하는 함수, solution을 완성해주세요.<br><br>

- 제한 조건 

num은 int 범위의 정수입니다.<br>
0은 짝수입니다.<br>


<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/116782694-9e892780-aac5-11eb-8d10-6a3b3c71e234.png)


<br><br>


### 풀이

```java
class Solution {
    public String solution(int num) {
        String answer = "";
        if(num%2==0) answer = "Even";
        else answer = "Odd";
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116782704-ad6fda00-aac5-11eb-92a9-8cc7540aac13.png)
<br><br>

### 풀이 (삼항 연산자 사용)

```java
class Solution {
    public String solution(int num) {
        String answer = num%2==0? "Even" : "Odd";
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116782770-fcb60a80-aac5-11eb-9901-2b79a5701f0a.png)

<br><br>


너무 간단한 문제,,😳

<br>

<br><br>


