---
title: "[프로그래머스]K번째수 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.<br>

예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면<br>

1. array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
2. 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
3. 2에서 나온 배열의 3번째 숫자는 5입니다.
배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.<br><br>

- 제한 조건 

array의 길이는 1 이상 100 이하입니다.<br>
array의 각 원소는 1 이상 100 이하입니다.<br>
commands의 길이는 1 이상 50 이하입니다.<br>
commands의 각 원소는 길이가 3입니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/120893776-f16b7700-c64f-11eb-8c41-b9fba95b6a1f.png)<br>





<br><br>


### 풀이

```java
import java.util.*;

class Solution {
    public int[] solution(int[] array, int[][] commands) {
        
        int[] answer = new int[commands.length];
        
        for(int i=0; i<commands.length; i++){
            
            int[] temp = Arrays.copyOfRange(array,commands[i][0]-1, commands[i][1]);
            
            Arrays.sort(temp);
        
            for(int n : temp) System.out.print(n);
            System.out.println();
            answer[i] = temp[commands[i][2]-1];
        }        
        return answer;
    }    
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/120893815-224bac00-c650-11eb-88fc-b632a1ea173d.png)


<br>


우선 답변을 담을 answer 배열의 사이즈를 commands의 행의 수와 동일하게 지정한다.<br>

반복문을 이용해서 commands의 각 행에 접근한다.<br>

Arrays.copyOfRange를 통해 배열을 복사해서 임시 배열에 저장해 둔다.<br>
**Arrays.copyOfRange(array, start, end)** = 특정 범위 배열 복사<br>
array = 원본 배열<br>
start = 시작 인덱스 (1이면 1번부터 복사)<br>
end = 끝 인덱스 (4면 3까지만 복사)<br>

<br>

Arrays.sort()를 이용해서 배열을 정렬한 후, 원하는 번째의 수를 선택해서 answer[] 배열에 저장한다.<br><br>



<br><br>
