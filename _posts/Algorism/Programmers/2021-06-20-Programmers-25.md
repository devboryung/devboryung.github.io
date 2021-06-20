---
title: "[프로그래머스] 정수 내림차순으로 배치하기 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
함수 solution은 정수 n을 매개변수로 입력받습니다.<br> n의 각 자릿수를 큰것부터 작은 순으로 정렬한 새로운 정수를 리턴해주세요.<br> 예를들어 n이 118372면 873211을 리턴하면 됩니다.<br>

<br><br>

- 제한 조건 

n은 1이상 8000000000 이하인 자연수입니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/122661765-e45e9400-d1c8-11eb-8d65-d1c642e7932b.png)





<br>





<br><br>


### 풀이



```java
import java.util.*;

class Solution {
    public long solution(long n) {
        
        String[] arr = String.valueOf(n).split("");
        
        Arrays.sort(arr,Collections.reverseOrder());

        
        String answer = "";
        
        for(String a : arr){

            answer += a;
        }

        return Long.parseLong(answer);
    }
}
```

<br>

**String.valueOf(a);** -> 숫자 자료형 String 타입으로 변경하기.<br>
**Long.parseLong(a);** -> String자료형에서 Long자료형으로 변경하기.<br>

<br>

우선 long자료형인 정수 n을 String으로 변경한 후 하나씩 잘라서 배열에 넣는다.<br>
그 후 Arrays.sort메서드를 이용해 Collections.reverseOrder()로 배열을 내림차순으로 정렬한다.<br>

정렬 후 바로 Long 타입에 데이터를 하나씩 담게되면 총 합이 나오기 때문에<br>
먼저 String에 데이터를 누적시킨 후, String을 Long 타입으로 변환시켜 반환한다.<br>







<br><br>