---
title: "[프로그래머스] 제일 작은 수 제거하기 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
정수를 저장한 배열, arr 에서 가장 작은 수를 제거한 배열을 리턴하는 함수, solution을 완성해주세요.<br> 단, 리턴하려는 배열이 빈 배열인 경우엔 배열에 -1을 채워 리턴하세요.<br> 예를들어 arr이 [4,3,2,1]인 경우는 [4,3,2]를 리턴 하고, [10]면 [-1]을 리턴 합니다.<br>

<br><br>

- 제한 조건 

arr은 길이 1 이상인 배열입니다.<br>
인덱스 i, j에 대해 i ≠ j이면 arr[i] ≠ arr[j] 입니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/122637176-2e436d80-d128-11eb-8501-cc76cbe3be66.png)




<br>





<br><br>


### 풀이



```java
import java.util.*;

class Solution {
    public Integer[] solution(int[] arr) {
        
        int min = arr[0]; // 최솟값
        for(int i=0; i<arr.length; i++){       
            if(min>arr[i]){
                min = arr[i];
            }            
        }
        
        List<Integer> list = new ArrayList<>();
        
        for(int i=0;i<arr.length; i++){
            if(min==arr[i]){
                continue;
            }
            list.add(arr[i]);
        }
        
        if(list.isEmpty()){
            list.add(-1);
        }
        
        Integer[] answer = list.toArray(new Integer[list.size()]);

        return answer;
    }
}
```

<br>

내림차순으로 정렬한 후 가장 마지막 인덱스만 제외하고 반환하면 되는 줄 알았는데, 문제상 데이터의 순서가 바뀌면 안 되는거였다.<br>

그래서  min으로 최솟값을 기억해둔 후 최솟값과 같은 데이터는 list에 추가하지 않는 형식으로 문제를 풀었다.<br>

근데 이상하게 처음에 빈 배열을 -1처리해주는 것을 잊어버리고 정답을 제출했는데 정답처리가 됐다..😲<br>
분명히 테스트 케이스로는 실패로 나오는데,, 아마 정답 중에 -1를 반환하는 결과가 없나보다?<br>
혹시 몰라서 빈 배열인 경우 -1를 리턴하는 코드도 추가했다.<br>








<br><br>