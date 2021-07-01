---
title: "[프로그래머스] 정수 제곱근 판별 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
임의의 양의 정수 n에 대해, n이 어떤 양의 정수 x의 제곱인지 아닌지 판단하려 합니다.<br>
n이 양의 정수 x의 제곱이라면 x+1의 제곱을 리턴하고, n이 양의 정수 x의 제곱이 아니라면 -1을 리턴하는 함수를 완성하세요.<br><br>


<br><br>

- 제한 조건 

n은 1이상, 50000000000000 이하인 양의 정수입니다.<br>
<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/124143866-6fdbfd00-dac6-11eb-8ac2-98efb1d837d9.png)




<br>





<br><br>


### 풀이



```java
class Solution {
    public long solution(long n) {
        
        long answer = 0;
        
        for(long i=1; i<=n; i++){
            

            if( i*i == n){
                answer = (i+1)*(i+1);
                break;
            }else{
                answer = -1;
            }
        }
        
        return answer;
    }
}
```

<br>


반복문을 이용해서 1~n 사이의 수에서 i*i == n인 것을 찾는다.<br>
찾고나면 break;를 통해서 반복문을 종료한다.<br>
n이 121인 경우 121까지 반복문이 실행되는데, i가 11이 됐을 때 11*11 이 121이므로 11의 제곱으로 판단이 돼 반복문이 종료된다.<br>
<br>



<br>


<br><br>