---
title: "[프로그래머스] 같은 숫자는 싫어 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
배열 arr가 주어집니다. 배열 arr의 각 원소는 숫자 0부터 9까지로 이루어져 있습니다.<br> 이때, 배열 arr에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 합니다.<br> 단, 제거된 후 남은 수들을 반환할 때는 배열 arr의 원소들의 순서를 유지해야 합니다.<br> 예를 들면,<br>
arr = [1, 1, 3, 3, 0, 1, 1] 이면 [1, 3, 0, 1] 을 return 합니다.<br>
arr = [4, 4, 4, 3, 3] 이면 [4, 3] 을 return 합니다.<br>
배열 arr에서 연속적으로 나타나는 숫자는 제거하고 남은 수들을 return 하는 solution 함수를 완성해 주세요.<br>

<br><br>

- 제한 조건 

배열 arr의 크기 : 1,000,000 이하의 자연수<br>
배열 arr의 원소의 크기 : 0보다 크거나 같고 9보다 작거나 같은 정수<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/121535331-06c01700-ca3d-11eb-86ef-adc301e770da.png)<br>





<br><br>


### 풀이



```java
import java.util.*;

public class Solution {
    public Integer[] solution(int []arr) {
        
        ArrayList<Integer> list = new ArrayList<>();
        
        
        list.add(arr[0]); // 첫 값은 무조건 넣기
        
        int j = 0;

        for(int i=1; i<arr.length; i++){

            if(list.get(j) != arr[i]){
                list.add(arr[i]);
                j++;
            }

        }
        
        return list.toArray(new Integer[list.size()]);
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121535615-4850c200-ca3d-11eb-9caa-93b7633a75bc.png)

<br>

처음에 비교 값을 list.get(i-1)로 하니까 `IndexOutOfBoundsException`이 발생했다.<br>
아마 i값은 반복이 될 때마다 증가하는데 반복되는 숫자가 나올때는 list의 인덱스는 증가가 안되니까 그 회차에 비교할 인덱스가 없기 때문에 오류가 난 것 같다.<br>
j를 따로 선언 하고 list에 값이 들어갔을 때 j 증가시키니까 정답이 나왔다.<br>

<br>

```java
list.add(arr[0]);
for(int i=1; i<arr.length; i++){
    if(arr[i] != arr[i-1]){
        list.add(arr[i]);
    }
}
```

조건문에서 list대신 arr만으로만 비교를 진행한다면 j는 필요없다.<br>


<br><br>
