---
title: "[프로그래머스] 완주하지 못한 선수
 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
수많은 마라톤 선수들이 마라톤에 참여하였습니다.<br> 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.<br><br>

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.<br>

<br><br>

- 제한 조건 

마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.<br>
completion의 길이는 participant의 길이보다 1 작습니다.<br>
참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.<br>
참가자 중에는 동명이인이 있을 수 있습니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/121148372-af734880-c87c-11eb-8477-aee7a9ffd94b.png)
<br>





<br><br>

이번 문제는 두 배열을 정렬해서 동일하다면 가장 마지막 인덱스, 일치하지 않는다면 일치하지 않는 인덱스를 return하는 방식으로 풀어봤는데 
String answer에 값을 담아 return하려고 하니 반복문이 종료된 후 if문을 잘못썼던건지 테스트 실패가 떠서 인터넷에서 다른 사람의 풀이 방법을 확인해봤다.<br>

answer에 값을 넣고 return해주는 방식이 아니라  값을 바로 return해주는 방식으로 하니까 정답이 나왔다.<br>
내가 if문을 잘못 쓴 건지, 아니면 answer에 마지막 값이 담겨서 넘어간건지 모르겠다..<br>

하지만 이 방법은 sort를 포함해서 for문이 3번이나 실행돼서 효율적이지 않은 코드라고 한다.<br>

### 풀이

```java
import java.util.*;

class Solution {
    public String solution(String[] participant, String[] completion) {
        
        
        Arrays.sort(participant);
        Arrays.sort(completion);
        
        int i;
        
        for(i=0; i<completion.length; i++){
            
            if(!participant[i].equals(completion[i])){
               return participant[i];
            }
        }
        
            return participant[i];

    }
    
}
```

<br>


<br>


## 다른 사람 풀이

HashMap이용 <br>


```java
import java.util.HashMap;

class Solution {
    public String solution(String[] participant, String[] completion) {
        
        String answer = "";
        
        HashMap<String, Integer> hm = new HashMap<>();
        
        for (String player : participant) {
            hm.put(player, hm.getOrDefault(player, 0) + 1);
        }
        
        for (String player : completion) {
            hm.put(player, hm.get(player) - 1);
        }
        
        for (String key : hm.keySet()) {
            if (hm.get(key) != 0){
                answer = key;
            }
        }
        return answer;
    }
}
```

HashMap은 키와 값으로 구성된 Entry객체를 저장하는 구조를 가지고 있으며, 값은 중복 저장될 수 있지만 키는 중복 저장될 수 없다.<br>
동일한 키로 값을 저장하면 기존의 값은 없어지고 새로운 값으로 대치된다.<br>


참가자 배열을 향상된 for문을 이용해 hm(HaspMap)에 담는다.<br>
키는 참가자 배열에 담긴 값이고, 값은 `hm.getOrDefault(player, 0) + 1`로 동일한 key가(동일한 이름) 맵에 존재하지 않는다면 0+1 = 1; 존재한다면 1+1 =2;로 저장된다. <br>
`getOrDefault(Object key, V defaultValue)` : 찾는 key 가 맵에 존재한다면 찾는 key의 value를 반환, 없다면 value를 defulatValue 반환<br>
<br>

완주자 배열을 향상된 for문을 이용해 hm에 담는다.<br>
이때 이미 hm에 담긴 것과 동일한 key(선수 이름)이 들어가게되면 값이 교체된다.<br>
만약 동일한 key가 있다면 그 선수의 값은 0이 될 것이다.<br>
(이미 참가자 배열을 통해 값이 1로 지정되어 있었는데 그 값에서 -1을 하면 0)<br><br>
만약 동명이인일 경우 그 값은 1보다 큰 수 일 테니 0이 될 수 없고, 완주하지 못했다면 0이 될 수 없으니 키값을 이용해 향상된 for문을 돌려 해당 key의 값이 0이 아닌 것을 반환한다. <br> 

``hm.keySet()` : hm에 저장된 키값 확인 <br>




<br>


<br><br>
