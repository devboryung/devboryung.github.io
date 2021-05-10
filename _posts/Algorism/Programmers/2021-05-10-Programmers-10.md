---
title: "[프로그래머스]하샤드 수 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---

### 문제 설명

양의 정수 x가 하샤드 수이려면 x의 자릿수의 합으로 x가 나누어져야 합니다.<br> 예를 들어 18의 자릿수 합은 1+8=9이고, 18은 9로 나누어 떨어지므로 18은 하샤드 수입니다.<br> 자연수 x를 입력받아 x가 하샤드 수인지 아닌지 검사하는 함수, solution을 완성해주세요.<br><br>

- 제한 조건 

x는 1 이상, 10000 이하인 정수입니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/117665613-fd455400-b1dd-11eb-8d4c-7d66877e7b55.png)




<br><br>


### 풀이

```java
class Solution {
    public boolean solution(int x) {
             
       boolean answer = true;
        
        int num = x;
        int sum = 0;
        while(num>0){
            sum += num%10;
            num /= 10;
        }

        if(x%sum != 0){
            answer = false;
        }
        
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/117665693-1221e780-b1de-11eb-96cb-069c44855e05.png)
<br>


answer이 기본적으로 true로 선언되어 있어서 나누어 떨어지지 않는 것만 false로 처리해주었다.<br><br>





<br><br>
