---
title: "[프로그래머스] 문자열 내 p와 y의 개수 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
대문자와 소문자가 섞여있는 문자열 s가 주어집니다.<br> s에 'p'의 개수와 'y'의 개수를 비교해 같으면 True, 다르면 False를 return 하는 solution를 완성하세요.<br> 'p', 'y' 모두 하나도 없는 경우는 항상 True를 리턴합니다.<br> 단, 개수를 비교할 때 대문자와 소문자는 구별하지 않습니다.<br><br>

예를 들어 s가 "pPoooyY"면 true를 return하고 "Pyy"라면 false를 return합니다.<br>

<br><br>

- 제한 조건 

문자열 s의 길이 : 50 이하의 자연수<br>
문자열 s는 알파벳으로만 이루어져 있습니다.<br>



<br>A

- 예시 

![image](https://user-images.githubusercontent.com/73421820/121902935-c0cbc180-cd62-11eb-8a0a-e1f86fb67790.png)
<br>





<br><br>


### 풀이



```java
class Solution {
    boolean solution(String s) {
        
        int cntP = count(s.toLowerCase(),'p');
        int cntY = count(s.toLowerCase(),'y');
        
        
        if(cntP != cntY){
            return false;
        }else{
            return true;
        }
    }
    
    public int count(String s, char c){
        int count = 0;
        
        for(int i=0; i<s.length(); i++){
            
            if(s.charAt(i) == c){
                count++;
            }
        }
        return count;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121902709-882be800-cd62-11eb-94bc-4788150c9321.png)


<br>

<br>

대,소문자 모두 비교가 가능해야되기 때문에 비교 문자를 소문자로 놓고 toLowerCase() 메서드를 이용해 문자열을 전부 소문자로 바꾸었다.<br>
그 후에 count() 메서드를 생성해서 문자열의 길이 만큼 반복하며 해당하는 문자를 세는 메서드를 만들어 호출했다.<br>

p와 y의 개수가 다르면 false를 반환하고, 개수가 같거나, 하나도 없는 경우 true를 반환한다.<br>



<br><br>
