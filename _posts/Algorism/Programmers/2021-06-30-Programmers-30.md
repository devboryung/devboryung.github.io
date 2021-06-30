---
title: "[프로그래머스] 약수의 개수와 덧셈 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
두 정수 left와 right가 매개변수로 주어집니다.<br>
 left부터 right까지의 모든 수들 중에서, 약수의 개수가 짝수인 수는 더하고, 약수의 개수가 홀수인 수는 뺀 수를 return 하도록 solution 함수를 완성해주세요.<br><br>


<br><br>

- 제한 조건 

1 ≤ left ≤ right ≤ 1,000<br>
<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/123979981-22935900-d9fc-11eb-9969-28fc73c3fddf.png)



<br>





<br><br>


### 풀이



```java
class Solution {
    public int solution(int left, int right) {
             
        int answer = 0;
        int count = 0;

        while(left<=right){
            for(int j=1; j<=left; j++){
                if(left%j ==0){
                    count++;
                }
            }

            if(count%2==0)  answer += left;
            else  answer -= left;
            count = 0;
            left++;
            
        }
        
        return answer;
    }
}
```

<br>

약수의 개수를 나타낼 count 변수를 선언한 후 약수가 발생할 때 마다 증가하게 구현했다.<br>
left의 약수 계산이 완료되면 count를 다시 0으로 선언하고, left를 증가시켜 right까지 반복한다.<br> 

<br>


<br><br>