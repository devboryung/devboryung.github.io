---
title: "[프로그래머스]자릿수 더하기(JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---

### 문제 설명

자연수 N이 주어지면, N의 각 자릿수의 합을 구해서 return 하는 solution 함수를 만들어 주세요.<br>
예를들어 N = 123이면 1 + 2 + 3 = 6을 return 하면 됩니다..<br><br>

- 제한 조건 

N의 범위 : 100,000,000 이하의 자연수<br>


<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/116416914-c4b88880-a875-11eb-9bb1-7dd6651c7959.png)



<br><br>


### 풀이

```java
public class Solution {
    public int solution(int n) {
        int answer = sum(n);
        return answer;
    }
    
    public int sum(int n){
        int sum = 0;
        while(n!=0){
            sum += n%10;
            n = n/10;
        }
        return sum;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116417026-de59d000-a875-11eb-83e6-27ef0139ee0d.png)
<br>

입력받은 숫자를 10으로 나눈 나머지는 맨 마지막 자리수가 된다.<br>
ex) 123456 % 10 = 6<br>
입력받은 숫자를 10으로 나눈 몫은 맨 마지막 숫자를 제외한 수다.<br>
ex) 123456 / 10 = 12345<br>
계속 반복하다 보면 맨 마지막 수부터 처음 수까지 구할 수 있으며, 그 값을 sum에 누적 합산 시켜서 총 합을 구할 수 있다.<br>

<br><br>



<br><br>


