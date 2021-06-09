---
title: "[프로그래머스] 문자열 내림차순으로 배치하기 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
문자열 s에 나타나는 문자를 큰것부터 작은 순으로 정렬해 새로운 문자열을 리턴하는 함수, solution을 완성해주세요.<br>
s는 영문 대소문자로만 구성되어 있으며, 대문자는 소문자보다 작은 것으로 간주합니다.<br>

<br><br>

- 제한 조건 

str은 길이 1 이상인 문자열입니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/121366162-38b97680-c974-11eb-8e4a-77a5be7cb26f.png)<br>





<br><br>


### 풀이



```java
import java.util.*;

class Solution {
    public String solution(String s) {
        String[] arr = new String[s.length()];
        
        arr = s.split("");
        
        Arrays.sort(arr, Comparator.reverseOrder());
        
        String answer = "";
        
        for(String a : arr){
            answer += a;
        }
       
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121370006-4c1a1100-c977-11eb-88d5-5d134140b327.png)

<br>

String으로는 문자끼리 비교를 할 수 없을 것 같아서 맨 처음에는 toCharArray()를 이용해
char자료형으로 바꾼 뒤 sort를 진행했다. <br>
하지만 기본 자료형은 Comparator객체의 reverseOrder()메서드를 사용할 수 없어서 Integer배열로 받아야 하나 고민하다가 String 배열로 받아봤는데 알아서 내림차순 정렬이 됐다..<br>
각 인덱스에 문자로 들어있으니까 알아서 char형태로 보고 비교하는 건가..?



<br><br>
