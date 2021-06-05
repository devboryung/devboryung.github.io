---
title: "[프로그래머스]두 개 뽑아서 더하기 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
정수 배열 numbers가 주어집니다.<br> numbers에서 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해서 만들 수 있는 모든 수를 배열에 오름차순으로 담아 return 하도록 solution 함수를 완성해주세요.<br>

<br><br>

- 제한 조건 

numbers의 길이는 2 이상 100 이하입니다.<br>
numbers의 모든 수는 0 이상 100 이하입니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/120894007-1dd3c300-c651-11eb-9938-35e839017d43.png)<br>





<br><br>


### 풀이

```java
import java.util.*;

class Solution {
    public int[] solution(int[] numbers) {
        
        HashSet<Integer> set = new HashSet<Integer>();
        
        for(int i=0; i<numbers.length; i++){
            for(int j=i+1; j<numbers.length; j++){
                set.add(numbers[i]+numbers[j]);
            }
        }
        
        ArrayList<Integer> list = new ArrayList<Integer>(set);
        
        Collections.sort(list);
        
        int[] answer = new int[list.size()];
        
        for(int i=0; i<answer.length; i++){
            answer[i] = list.get(i);
        }
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/120894021-3512b080-c651-11eb-90b8-5bb0e6f747b6.png)



<br>


중복된 값을 없애기 위해서 더한 값을 HashSet에 담는다.<br>

정렬을 위해서 Set을 다시 ArrayList로 변경한 후 Colletcions.sort()로 정렬을 해준다.<br>

답을 위한 answer[]을 생성해 준 후, answer 배열에 list에 담긴 값을 옮겨 담는다.<br>

옮겨 담을 때 for문이 아닌 toArray를 이용하려면 메서드의 반환형을 Integer[]로 변경하고 answer[]의 자료형도 Integer로 변경해야 한다.<br>

```java
Integer[] answer = list.toArray(new Integer[list.size()]);
```

<br>

toArray() 코드를 이용한다면 반복문을 통해 하나하나 배열에 넣는 방법보다 속도와 효율성이 좋다고 한다.<br>



<br><br>
