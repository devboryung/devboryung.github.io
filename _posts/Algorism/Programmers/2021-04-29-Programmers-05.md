---
title: "[프로그래머스]나누어 떨어지는 숫자 배열"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---

### 문제 설명

array의 각 element 중 divisor로 나누어 떨어지는 값을 오름차순으로 정렬한 배열을 반환하는 함수, solution을 작성해주세요.<br>
divisor로 나누어 떨어지는 element가 하나도 없다면 배열에 -1을 담아 반환하세요.<br><br>

- 제한 조건 

arr은 자연수를 담은 배열입니다.<br>
정수 i, j에 대해 i ≠ j 이면 arr[i] ≠ arr[j] 입니다.<br>
divisor는 자연수입니다.<br>
array는 길이 1 이상인 배열입니다.<br>


<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/116545862-e70cdd80-a92b-11eb-8b47-e03558afe86c.png)



<br><br>


### 풀이

```java
import java.util.*;

class Solution {

    public int[] solution(int[] arr, int divisor) {
        int[] answer = division(arr,divisor);
        return answer;
    }
    
    public int[] division(int[] arr, int divisor){

        ArrayList<Integer> number = new ArrayList<>();

        int count = 0;
        for(int i:arr){
            if(i%divisor==0){
                number.add(i);
                count++;
            }
        }

        if(count ==0){
            number.add(-1);
        }

        Collections.sort(number);

        int[] arr2 = new int[number.size()];

        count = 0;
        for(int i: number){
            arr2[count++] = i;
        }
 
        return arr2;
        
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116545929-f8ee8080-a92b-11eb-86fc-b0c9381ef24f.png)
<br>


실행 성공했는데 코드가 너무 복잡한 것 같다...😨

<br>

<br><br>


