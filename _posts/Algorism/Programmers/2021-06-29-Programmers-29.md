---
title: "[프로그래머스] 약수의 합 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
정수 n을 입력받아 n의 약수를 모두 더한 값을 리턴하는 함수, solution을 완성해주세요.<br><br>


<br><br>

- 제한 조건 

n은 0 이상 3000이하인 정수입니다.<br>
<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/123816384-3c686980-d932-11eb-8130-80a07ad3debc.png)







<br>





<br><br>


### 풀이



```java
class Solution {
    public int solution(int n) {
        
        int answer = 0;
        
        for(int i=1; i<=n; i++){
            
            if(n%i==0){
                answer += i;
            }
        }
        
        return answer;
    }
}
```

<br>

약수는 어떤 수를 나누었을 때 나머지가 없이 떨어지는 것을 의미하기 때문에<br> 주어진 n을 1부터 차례대로 나눈 후 나머지가 0인 것을 합해주면 된다.<br>


<br><br>

### 다른 사람 풀이

```java
class Solution {
    public int solution(int n) {
        
        int answer = 0;
        
        for(int i=1; i<=n/2; i++){
            
            if(n%i==0){
                answer += i;
            }
            
        }
        
        return answer+n;
    }
}
```

주어진 n값의 절반만 반복해도 된다.<br>
왜?<br>
8의 약수 = 1,2,4,8<br>
12의 약수 = 1,2,4,6,8,12<br>
48의 약수 = 1,2,3,4,6,8,12,16,24,48<br>

<br><br>

하지만 for문의 i값이 1부터 시작하기 때문에, <br>마지막 값(주어진 값)은 answer에 더해지지 않기 때문에 return할 때 n값을 더해줘야한다.<br>

<br>


<br><br>