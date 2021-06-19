---
title: "[프로그래머스] 이상한 문자 만들기 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
문자열 s는 한 개 이상의 단어로 구성되어 있습니다.<br> 각 단어는 하나 이상의 공백문자로 구분되어 있습니다.<br> 각 단어의 짝수번째 알파벳은 대문자로, 홀수번째 알파벳은 소문자로 바꾼 문자열을 리턴하는 함수, solution을 완성하세요.<br>

<br><br>

- 제한 조건 

문자열 전체의 짝/홀수 인덱스가 아니라, 단어(공백을 기준)별로 짝/홀수 인덱스를 판단해야합니다.<br>
첫 번째 글자는 0번째 인덱스로 보아 짝수번째 알파벳으로 처리해야 합니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/122570418-b2ddaf80-d086-11eb-8ead-2cbf8adee64e.png)



<br>





<br><br>


### 풀이



```java
class Solution {
    public String solution(String s) {
        String answer = "";
        
        String[] arr = s.split(" ",-1);
        char c = 0;
        int idx = 0;
        
        for(String x : arr){
            
            for(int i=0; i<x.length(); i++){
                
                if(i%2==0){
                    c = Character.toUpperCase(x.charAt(i));
                }else{
                    c = Character.toLowerCase(x.charAt(i));
                }
                answer += c;
            }
            idx++;
            
            if(idx != arr.length){
                answer += " ";
            }            
        }
        
        return answer;
    }
}
```

<br>

처음엔 배열을 String[] arr = s.split(" "); 로 나눴는데 
![image](https://user-images.githubusercontent.com/73421820/122570476-c12bcb80-d086-11eb-86aa-6f0cf288ef7e.png)<br>

테스트 4, 5, 8, 9, 11번에서 실패가 나왔다.<br>

알고보니 문자열 마지막에 공백이 있을 경우 
s.split(" ")을 할 경우 마지막 공백이 사라지기 때문에
s.split(" ",-1)을 해서 마지막 공백도 배열에 담기게 해야한다.<br>

**EX)**

s.split(" ")  ->  {"try","hello","world"}<br>
![image](https://user-images.githubusercontent.com/73421820/122571615-dfde9200-d087-11eb-90f4-1e0494a7d5dd.png)<br>

마지막 공백이 사라짐.<br>

s.split(" ",-1)  ->  {"try","hello","world",""}<br>

<br><br>

### 다른 사람 풀이

```java
class Solution {
    public String solution(String s) {
        String answer = "";
        
        String[] arr = s.split("");
        int idx=0;
        
        for(int i=0; i<arr.length; i++){
            if(arr[i].equals(" ")){
                idx=0;
            }else if(idx%2==0){
                arr[i] = arr[i].toUpperCase();
                idx++;
            }else{
                arr[i]= arr[i].toLowerCase();
                idx++;
            }    
            
            answer += arr[i];
        }
        
        return answer;
    }
}
```

<br>

split("")를 이용해 문자열을 한 글자씩 배열에 담는다.<br>
인덱스 홀수,짝수를 알기 위해서 idx라는 변수를 선언한다.<br>

만약 i번째가 공백이라면 새로운 단어로 바뀌는 것이기 때문에 idx를 0으로 선언한다.<br>
공백이 아니라면 idx는 후위연산으로 하나씩 증가하게 되며, idx가 짝수라면 대문자, 홀수라면 소문자로 변경해준다.<br>
변경된 것을 answer에 담아 반환해주면 된다.<br>






<br><br>