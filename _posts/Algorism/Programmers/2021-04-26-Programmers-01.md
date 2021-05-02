---
title: "[프로그래머스]두 정수 사이의 합(JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---

### 문제 설명

두 정수 a, b가 주어졌을 때 a와 b 사이에 속한 모든 정수의 합을 리턴하는 함수, solution을 완성하세요. <br>
예를 들어 a = 3, b = 5인 경우, 3 + 4 + 5 = 12이므로 12를 리턴합니다. <br><br>

- 제한 조건 

a와 b가 같은 경우는 둘 중 아무 수나 리턴하세요.<br>
a와 b는 -10,000,000 이상 10,000,000 이하인 정수입니다.<br>
a와 b의 대소관계는 정해져있지 않습니다.<br>

<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/116088211-9fdcdd80-a6dc-11eb-955e-24ad8e85d609.png)


<br><br>

### 풀이

```java
class Solution {
    public long solution(int a, int b) {
        long answer = 0;
        
        answer = sum(Math.max(a,b), Math.min(a,b));
        
        return answer;
    }
    
    public long sum(int a, int b){
        
        long total = 0;        
        if(a==b){
            total = a;
        }else{
            for(int i=b; i<=a; i++){
                total += i;
            }
        }       
        return total;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116095261-37ddc580-a6e3-11eb-86e3-5d84d8ab5517.png)<br>



<br><br>

### 다른 사람의 풀이

```java
class Solution {

    public long solution(int a, int b) {
        return sumAtoB(Math.min(a, b), Math.max(b, a));
    }

    private long sumAtoB(long a, long b) {
        return (b - a + 1) * (a + b) / 2;
    }
}
```
<br>

![image](https://user-images.githubusercontent.com/73421820/116095538-7a070700-a6e3-11eb-987c-ec2cf3302b3d.png)

<br>

for문을 이용해서 풀면 시간 복잡도가 O(n)이 되지만,<br>
`등차수열의 합`을 이용해서 풀면 시간복잡도가 O(1)로 나온다.<br>

<br><br>

### 등차수열의 합

등차수열의 합은 `연속하는 두 항의 차이가 모두 일정한 수열을 뜻한다`라는 정의를 가지고 있다.<br>제 n 항까지를 기준으로 항의 숫자의 합이 같다면 그 합은 일정하다고 말할 수 있다.<br>
<br>

등차수열 방법 : <br>
1. n(a+l)/2 == (개수(첫 항+끝 항)/2)
2. n(2a+(n-1)d)/2 == (개수(첫 항+(n항)*공차)/2 

지금은 a~b의 정수의 합을 구하는 것이기 때문에 각 항의 공차는 1이다.<br>
`(b - a + 1) * (a + b) / 2` <br>
(b-a+1)은 항의 개수가 되고, (a+b)는 첫 항과 끝 항의 합이 된다.<br><br>