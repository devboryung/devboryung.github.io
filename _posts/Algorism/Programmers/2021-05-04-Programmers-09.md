---
title: "[프로그래머스]핸드폰 번호 가리기
(JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---

### 문제 설명

프로그래머스 모바일은 개인정보 보호를 위해 고지서를 보낼 때 고객들의 전화번호의 일부를 가립니다.<br>
전화번호가 문자열 phone_number로 주어졌을 때, <br>전화번호의 뒷 4자리를 제외한 나머지 숫자를 전부 *으로 가린 문자열을 리턴하는 함수, solution을 완성해주세요.<br><br>

- 제한 조건 

s는 길이 4 이상, 20이하인 문자열입니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/116979549-193d8700-ad00-11eb-8c66-d2e1628c7d52.png)



<br><br>


### 풀이

```java
class Solution {
    public String solution(String phone_number) {
        
        String answer = "";
        
        for(int i=0; i<phone_number.length(); i++){
            if(i<phone_number.length()-4){
                answer += "*";
            }else{
                answer += phone_number.charAt(i);
            }
        }
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116979627-31ada180-ad00-11eb-9d16-39f6f4e7f8e3.png)
<br>


수행 시간이 너무 오래 걸린다..<br><br>


<br>
<br>



### 다른 사람의 풀이(char->String)

```java
class Solution {
  public String solution(String phone_number) {
     char[] ch = phone_number.toCharArray();
     for(int i = 0; i < ch.length - 4; i ++){
         ch[i] = '*';
     }
     return String.valueOf(ch);
  }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116979862-7a655a80-ad00-11eb-8b1d-30f8a4ce88f0.png)

<br><br>

String.toCharArray()로 문자열을 char형 배열로 바꾼다.<br>
그 후 배열의 길이-4(마지막 4자리 숫자)보다 작은 경우 값을 '*'로 변경한다.<br>
마지막 return 시 String.valueOf()를 통해 String 타입으로 변경해 준다.<br><br>

이 방법은 수행 시간이 매우 단축된다.<br>

첫 번째 방법으로 풀 경우 문자열을 조합하는 데 시간이 많이 드는 건지, for문 안에 if문을 넣어줘서 시간이 많이 드는 건지 모르겠다.😰😰<br>


### 풀이

```java
class Solution {
    public String solution(String phone_number) {
        
        String answer = "";
        
        for(int i=0; i<phone_number.length()-4; i++){
            answer += "*";
        }

        answer += phone_number.substring(phone_number.length()-4);
        
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116980716-869de780-ad01-11eb-9c48-dcefb246d13e.png)

<br><br>

위에 코드를 참고해서 for문의 반복 횟수를 줄이고 조건문을 없앴더니 <br>
수행 시간이 많이 단축됐다.😉<br>
하지만 아직도 위에 코드보다 수행 시간이 오래 걸리는 것을 보면<br>
아마 문자열 조합 작업에서 시간이 더 오래 소요되는 걸까..?<br>
관련 자료를 한번 찾아봐야겠다.😢

<br><br>