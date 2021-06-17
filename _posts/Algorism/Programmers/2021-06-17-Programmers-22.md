---
title: "[프로그래머스] 서울에서 김서방 찾기 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
String형 배열 seoul의 element중 "Kim"의 위치 x를 찾아, "김서방은 x에 있다"는 String을 반환하는 함수, solution을 완성하세요.<br> seoul에 "Kim"은 오직 한 번만 나타나며 잘못된 값이 입력되는 경우는 없습니다.<br>

<br><br>

- 제한 조건 

seoul은 길이 1 이상, 1000 이하인 배열입니다.<br>
seoul의 원소는 길이 1 이상, 20 이하인 문자열입니다.<br>
"Kim"은 반드시 seoul 안에 포함되어 있습니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/122404651-5e6cfe00-cfba-11eb-9754-c6402f05240d.png)



<br>





<br><br>


### 풀이

- for문을 이용해서 배열에서 Kim과 동일한 글자 찾는 방법.<br>


```java
class Solution {
    public String solution(String[] seoul) {
        

        String answer = "";
        
        for(int i=0; i<seoul.length; i++){
            if(seoul[i].equals("Kim")){
                answer = "김서방은 " + Integer.toString(i)+"에 있다";
                break;
            }
        }
        return answer;
    }
}
```

<br><br><br>


- ArrayList.indexOf를 이용해서 Kim을 찾는 방법.

ArrayList.indexOf(Object o)는 그 객체가 배열에 있으면 해당 배열 번호를 리턴한다.<br>
(찾지 못할 경우 -1 반환)<br>

```java
import java.util.Arrays;

class Solution {
    public String solution(String[] seoul) {
        
        return "김서방은 " + Arrays.asList(seoul).indexOf("Kim") +"에 있다" ;
    }
}
```






<br><br>








<br><br>