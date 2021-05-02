---
title: "[프로그래머스]평균 구하기(JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---

### 문제 설명

정수를 담고 있는 배열 arr의 평균값을 return하는 함수, solution을 완성해보세요.<br><br>

- 제한 조건 

arr은 길이 1 이상, 100 이하인 배열입니다.<br>
arr의 원소는 -10,000 이상 10,000 이하인 정수입니다.<br>


<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/116402310-3806ce00-a867-11eb-9b56-b21d47cf173e.png)



<br><br>


### 풀이

```java
class Solution {
    public double solution(int[] arr) {
        double answer = countAvg(arr);
        return answer;
    }
    
    public double countAvg(int[] arr){
          
        double sum = 0;
        
        for(int i : arr){
            sum+= i;
        }
            
        return sum/arr.length;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116402692-b1062580-a867-11eb-9f7b-55da74d1a150.png)
<br>
<br><br>

### 다른 사람의 풀이

```java
import java.util.Arrays;

class Solution {
    public double solution(int[] arr) {
        double answer = countAvg(arr);
        return answer;
    }
    
    public double countAvg(int[] arr){
          
        return (double) Arrays.stream(arr).average().orElse(0);
    }
}
```



<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116404562-b9f7f680-a869-11eb-8bb5-7ca64890db72.png)

<br>


라이브러리를 사용해서 푸는 방법이다.<br>
라이브러리를 사용한 코드의 실행 시간이 훨씬 더 오래 걸린다.<br><br>


stream = 배열이나 컬렉션으로 원하는 값을 얻을 때 for문 도배를 방지하기 위해 나온 개념<br>
average() = 평균 구하는 메소드 <br>
orElse() => 값이 저장되어 있지 않을 경우 디폴트 값 지정<br>

<br><br>