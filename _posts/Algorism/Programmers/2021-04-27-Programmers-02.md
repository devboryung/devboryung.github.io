---
title: "[프로그래머스]가운데 글자 가져오기(JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명

단어 s의 가운데 글자를 반환하는 함수, solution을 만들어 보세요.<br> 단어의 길이가 짝수라면 가운데 두글자를 반환하면 됩니다.<br><br>

- 제한 조건 

s는 길이가 1 이상, 100이하인 스트링입니다.<br>


<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/116187459-8a10fc00-a760-11eb-90dc-28cab1728aeb.png)



<br><br>


### 풀이 (charAt 이용)

```java
class Solution {
    public String solution(String s) {
        String answer = mid(s);
        return answer;
    }
    
    public String mid(String s){
        int length = s.length();
        char a;
        char b;
        
       if(length%2==0){ // 짝수
           a= s.charAt(length/2-1);
           b= s.charAt(length/2);
           return Character.toString(a)+Character.toString(b);
        }else{ // 홀수
           a= s.charAt(length/2);
           return Character.toString(a);
       }
    } 
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116188496-7797c200-a762-11eb-879a-22257f3f8718.png)
<br>



<br>


### 풀이 (subString 이용)

Substring 이용

```java
class Solution {
    public String solution(String s) {
        String answer = mid(s);
        return answer;
    }
    
    public String mid(String s){
        int length = s.length();
        
        String a = length%2==0?
        s.substring(length/2-1,length/2)+s.substring(length/2,length/2+1):
        s.substring(length/2,length/2+1);
        return a;
       }   
```
<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116214319-8e014600-a781-11eb-84d9-83831ac7492a.png)


<br><br>