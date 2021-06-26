---
title: "[프로그래머스] 콜라츠 추측(JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
1937년 Collatz란 사람에 의해 제기된 이 추측은, 주어진 수가 1이 될때까지 다음 작업을 반복하면, 모든 수를 1로 만들 수 있다는 추측입니다.<br> 작업은 다음과 같습니다.<br><br>

1-1.입력된 수가 짝수라면 2로 나눕니다. <br>
1-2. 입력된 수가 홀수라면 3을 곱하고 1을 더합니다.<br>
2.결과로 나온 수에 같은 작업을 1이 될 때까지 반복합니다.<br>
<br><br>

예를 들어, 입력된 수가 6이라면 6→3→10→5→16→8→4→2→1 이 되어 총 8번 만에 1이 됩니다.<br> 위 작업을 몇 번이나 반복해야하는지 반환하는 함수, solution을 완성해 주세요.<br> 단, 작업을 500번을 반복해도 1이 되지 않는다면 –1을 반환해 주세요.<br>

<br><br>

- 제한 조건 

입력된 수, num은 1 이상 8000000 미만인 정수입니다.<br>
<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/123513310-8bf72d00-d6c7-11eb-8ffe-57ace51cc96e.png)






<br>





<br><br>


### 풀이



```java
class Solution {
    public int solution(long num) {
        
        int answer = 0;
        
        while(num != 1){
            
            if(num%2 == 0){
                num /= 2;      
            }else{
                num = num*3 +1;    
            }
            answer++;

            if(answer == 500){
                return -1;                
            }
        }
     
        return answer;
    }
}
```

<br>

처음 파라미터의 자료형이 int로 되어있었는데, 
5번, 7번 테스트가 계속 실패했다.<br>

자료형을 long으로 변경해보니 모두 통과되었다.<br>






<br><br>