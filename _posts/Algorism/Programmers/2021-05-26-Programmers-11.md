---
title: "[프로그래머스]문자열을 정수로 바꾸기 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명

문자열 s를 숫자로 변환한 결과를 반환하는 함수, solution을 완성하세요.<br><br>

- 제한 조건 

s의 길이는 1 이상 5이하입니다.<br>
s의 맨앞에는 부호(+, -)가 올 수 있습니다.<br>
s는 부호와 숫자로만 이루어져있습니다.<br>
s는 "0"으로 시작하지 않습니다.<br>



<br>

- 예시 

예를들어 str이 "1234"이면 1234를 반환하고, "-1234"이면 -1234를 반환하면 됩니다.<br>
str은 부호(+,-)와 숫자로만 구성되어 있고, 잘못된 값이 입력되는 경우는 없습니다.<br>




<br><br>


### 풀이

```java
class Solution {
    public int solution(String s) {
        int answer = Integer.parseInt(s);
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/119677939-199ded80-be7a-11eb-9adc-1ed2c89a12ae.png)

<br>


**Integer.parseInt()**<br>
문자열 인자를 구문분석하여 특정 진수(수의 진법 체계에 기준이 되는 값)의 정수를 반환한다.<br>
parseInt는 양수를 의미하는 + 기호와 음수를 나타내는 - 기호를 정확히 이해하기 때문에 문자열을 정수로 바꿀 때 부호도 처리해준다.<br>




<br><br>
