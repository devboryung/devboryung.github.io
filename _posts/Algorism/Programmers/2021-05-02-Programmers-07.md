---
title: "[프로그래머스]자연수 뒤집어 배열로 만들기(JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---

### 문제 설명

자연수 n을 뒤집어 각 자리 숫자를 원소로 가지는 배열 형태로 리턴해주세요.<br>예를들어 n이 12345이면 [5,4,3,2,1]을 리턴합니다.<br><br>

- 제한 조건 

n은 10,000,000,000이하인 자연수입니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/116813247-e5daeb00-ab8d-11eb-84d1-3b25f156b04f.png)


<br><br>


### 풀이

```java
import java.util.*;

class Solution {
    public int[] solution(long n) {
        int num = Long.toString(n).length();
        int[] answer = new int[num];
        int cnt = 0;
        
        while(n>0){
            answer[cnt++] = (int)(n%10);
            n/=10;            
        }
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116813261-f2f7da00-ab8d-11eb-8e0c-3fd5fc7ff39e.png)
<br>

정수를 String으로 변환한 후, 문자열의 길이를 반환받아 배열의 크기를 지정한다.<br>
정수%10 = 마지막 자리 수가 되기 때문에 순서대로 배열에 담아두면 자동으로 숫자를 거꾸로 반환받을 수 있다.<br>


<br>
<br>



### 풀이 (""을 더해서 문자열로 변환)

```java
import java.util.*;

class Solution {
    public int[] solution(long n) {
        String a = ""+n;
        int[] answer = new int[a.length()];
        int cnt = 0;
        
        while(n>0){
            answer[cnt++] = (int)(n%10);
            n/=10;            
        }
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116813399-91843b00-ab8e-11eb-9b88-8ae44d237464.png)

<br><br>


정수에 ""를 더해 문자열로 바꾼 뒤, 배열의 크기를 지정한 후 반복문을 돌린다.<br>
이 경우 위에 코드보다 실행 시간이 오래 걸린다..<br>
찾아보니 ""+n 할 경우 자바 내부적으로 StringBuffer가 생성eho서, new StringBuffer 후 append 한 거랑 똑같은 거라고 한다.😵<br>


<br>

<br><br>



