---
title: "[프로그래머스] x만큼 간격이 있는 n개의 숫자 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
함수 solution은 정수 x와 자연수 n을 입력 받아, x부터 시작해 x씩 증가하는 숫자를 n개 지니는 리스트를 리턴해야 합니다.<br> 다음 제한 조건을 보고, 조건을 만족하는 함수, solution을 완성해주세요.<br>

<br><br>

- 제한 조건 

x는 -10000000 이상, 10000000 이하인 정수입니다.<br>
n은 1000 이하인 자연수입니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/122662290-b67b4e80-d1cc-11eb-92c4-1ffc16b06b92.png)





<br>





<br><br>


### 풀이



```java
import java.util.*;

class Solution {
    public long[] solution(long x, int n) {

        long answer[] = new long[n];
        
        int i=0; // 반복 횟수
        long num = x; //x의 초기값 넣어둠
        
        while(i<n){            
            answer[i] = x;
            x +=num;
            i++;
        }

        return answer;
    }
}
```

<br>

배열의 길이를 지정할 수 없을 것 같아서 List<Integer\>를 만든 후 배열로 변경해야되나 싶었는데, 숫자 n개를 가지는 배열을 반환하기 때문에 배열의 길이도 n이었다..<br><br>

원래 매개변수 x를 받아올 때 int로 선언되어 있었으나 테스트 13,14번이 통과가 안돼서 찾아보니 x가 큰 수 일때 int의 범위를 넘어서서 long으로 변경해주어야 된다.<br>







<br><br>