---
title: "[프로그래머스] x만큼 간격이 있는 n개의 숫자 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
당신은 폰켓몬을 잡기 위한 오랜 여행 끝에, 홍 박사님의 연구실에 도착했습니다.<br>
홍 박사님은 당신에게 자신의 연구실에 있는 총 N 마리의 폰켓몬 중에서 N/2마리를 가져가도 좋다고 했습니다.<br>

홍 박사님 연구실의 폰켓몬은 종류에 따라 번호를 붙여 구분합니다. 따라서 같은 종류의 폰켓몬은 같은 번호를 가지고 있습니다.<br>
예를 들어 연구실에 총 4마리의 폰켓몬이 있고, 각 폰켓몬의 종류 번호가 [3번, 1번, 2번, 3번]이라면 이는 3번 폰켓몬 두 마리, 1번 폰켓몬 한 마리, 2번 폰켓몬 한 마리가 있음을 나타냅니다.<br>
이때, 4마리의 폰켓몬 중 2마리를 고르는 방법은 다음과 같이 6가지가 있습니다.<br>


첫 번째(3번), 두 번째(1번) 폰켓몬을 선택<br>

첫 번째(3번), 세 번째(2번) 폰켓몬을 선택<br>

첫 번째(3번), 네 번째(3번) 폰켓몬을 선택<br>

두 번째(1번), 세 번째(2번) 폰켓몬을 선택<br>

두 번째(1번), 네 번째(3번) 폰켓몬을 선택<br>

세 번째(2번), 네 번째(3번) 폰켓몬을 선택<br>

이때, 첫 번째(3번) 폰켓몬과 네 번째(3번) 폰켓몬을 선택하는 방법은 한 종류(3번 폰켓몬 두 마리)의 폰켓몬만 가질 수 있지만,<br> 다른 방법들은 모두 두 종류의 폰켓몬을 가질 수 있습니다.<br> 따라서 위 예시에서 가질 수 있는 폰켓몬 종류 수의 최댓값은 2가 됩니다.<br>
당신은 최대한 다양한 종류의 폰켓몬을 가지길 원하기 때문에, 최대한 많은 종류의 폰켓몬을 포함해서 N/2마리를 선택하려 합니다.<br> N마리 폰켓몬의 종류 번호가 담긴 배열 nums가 매개변수로 주어질 때, N/2마리의 폰켓몬을 선택하는 방법 중, 가장 많은 종류의 폰켓몬을 선택하는 방법을 찾아, 그때의 폰켓몬 종류 번호의 개수를 return 하도록 solution 함수를 완성해주세요.<br>

<br><br>

- 제한 조건 

nums는 폰켓몬의 종류 번호가 담긴 1차원 배열입니다.<br>
nums의 길이(N)는 1 이상 10,000 이하의 자연수이며, 항상 짝수로 주어집니다.<br>
폰켓몬의 종류 번호는 1 이상 200,000 이하의 자연수로 나타냅니다.<br>
가장 많은 종류의 폰켓몬을 선택하는 방법이 여러 가지인 경우에도, 선택할 수 있는 폰켓몬 종류 개수의 최댓값 하나만 return 하면 됩니다.<br>
<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/122916283-ae233f00-d397-11eb-93a9-14bc78d9020e.png)





<br>





<br><br>


### 풀이



```java
import java.util.*;

class Solution {
    public int solution(int[] nums) {
        
        int num = nums.length/2;
        
        List<Integer> list = new ArrayList<>();
        
        for(int i=0; i<nums.length; i++){
            if(list.indexOf(nums[i]) < 0){
                list.add(nums[i]);
            }
        }
        

        int answer = 0;
        
        if(list.size()>num){
            answer = num;
        }else{
            answer = list.size();
        }
        
    
        return answer;
    }
}
```

<br>

이번 문제는 문제 자체가 너무 길었다..<br>
일단 몇 마리가 있던, N/2 마리보다 초과해서 가질 수 없고 종류를 고르는 것이기 때문에 중복되는 종류는 1개로 카운트가 된다.<br>

그래서 일단 nums.length/2로 가질 수 있는 최대 개수를 먼저 저장해둔 후,<br>
중복되지 않은 종류들만 list에 담아 가질 수 있는 개수보다 list의 size()가 크다면
가질 수 있는 개수만 반환하고, 아니라면 list의 size()를 반환하는 것으로 코드를 짰다.<br>

3항 연산자를 이용해서 코드를 더 짧게 작성할 수도 있다.<br>

```java
import java.util.*;

class Solution {
    public int solution(int[] nums) {
        
        
        List<Integer> list = new ArrayList<>();
        
        for(int i=0; i<nums.length; i++){
            if(list.indexOf(nums[i]) < 0){
                list.add(nums[i]);
            }
        }

        return list.size()>nums.length/2 ? nums.length/2 : list.size();
    }
}
```

<br><br>

List가 아닌 Set을 이용한다면, Set은 중복값을 허용하지 않기 때문에 if문을 사용하지 않고 바로 set.add(nums[i])를 하면 된다.<br>


```java
import java.util.*;

class Solution {
    public int solution(int[] nums) {
        
        
        Set<Integer> set = new HashSet<>();
        
        for(int i=0; i<nums.length; i++){
            set.add(nums[i]);
        }

        return set.size()>nums.length/2 ? nums.length/2 : set.size();
    }
}
```

<br>

<br><br>