---
title: "[프로그래머스] 문자열 내 마음대로 정렬하기 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
문자열로 구성된 리스트 strings와, 정수 n이 주어졌을 때, 각 문자열의 인덱스 n번째 글자를 기준으로 오름차순 정렬하려 합니다.<br> 예를 들어 strings가 ["sun", "bed", "car"]이고 n이 1이면 각 단어의 인덱스 1의 문자 "u", "e", "a"로 strings를 정렬합니다.<br>

<br><br>

- 제한 조건 

strings는 길이 1 이상, 50이하인 배열입니다.<br>
strings의 원소는 소문자 알파벳으로 이루어져 있습니다.<br>
strings의 원소는 길이 1 이상, 100이하인 문자열입니다.<br>
모든 strings의 원소의 길이는 n보다 큽니다.<br>
인덱스 1의 문자가 같은 문자열이 여럿 일 경우, 사전순으로 앞선 문자열이 앞쪽에 위치합니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/122192065-4324c480-cece-11eb-8073-7c8c4d4d45d8.png)


<br>





<br><br>


### 풀이



```java
import java.util.*;

class Solution {
    public String[] solution(String[] strings, int n) {
        
        String[] answer = new String[strings.length];
        
        for(int i=0; i<strings.length; i++){
            answer[i] = strings[i].substring(n,n+1)+strings[i];
        }
        
        Arrays.sort(answer);
       
        for(int i=0; i<strings.length; i++){
            answer[i] = answer[i].substring(1);
        }
        
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/122192345-897a2380-cece-11eb-93fd-1d55ef0426ea.png)


<br>

<br>

substring을 이용해 n번째 문자를 잘라서 문자열 앞에 붙여준다.<br>
그 상태로 정렬을 하면 n번째 문자를 기준으로 오름차순 정렬이 된다.`{"acar", "ebed", "usun"}`<br>
정렬된 상태에서 맨 앞글자만 자른 후 배열에 넣어주면 정답이 된다.<br><br>




<br><br>

### 다른 사람 풀이 (compare)

```java
import java.util.*;

class Solution {
  public String[] solution(String[] strings, int n) {
      
      Arrays.sort(strings, new Comparator<String>() {

          @Override
          public int compare(String s1, String s2){
              char c1 = s1.charAt(n);
              char c2 = s2.charAt(n);
              
              // n번째 문자가 같을 경우에 원본 스트링을 사전순으로
              if(c1 == c2){
                  return s1.compareTo(s2);
              } else return c1 - c2; // 문자가 같지 않을 경우 c1이 크면 양수, 작으면 음수가 나온다.<br>
          }
      });
      
      return strings;
  }
}
```

<br>

sort(T[] a, Comparator<? super T> c) => 정렬하고자하는 기준을 구현한 클래스를 넣어주면 그 기준대로 정렬이 된다.<br>







<br><br>
<br><br>