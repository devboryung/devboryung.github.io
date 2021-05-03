---
title: "[프로그래머스]수박수박수박수박수박수?
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

길이가 n이고, "수박수박수박수...."와 같은 패턴을 유지하는 문자열을 리턴하는 함수, solution을 완성하세요.<br> 예를들어 n이 4이면 "수박수박"을 리턴하고 3이라면 "수박수"를 리턴하면 됩니다.<br><br>

- 제한 조건 

n은 길이 10,000이하인 자연수입니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/116888726-c237a380-ac66-11eb-9479-703556c71db3.png)


<br><br>


### 풀이

```java
class Solution {
    public String solution(int n) {
        
        String answer = "";

        for(int i=1; i<=n; i++){
           
         if(i%2==1){
                answer += "수";
            }else{
                answer += "박";
            } 
        }
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116888792-d54a7380-ac66-11eb-95a8-35b1f3990097.png)
<br>


길이에 따라 문자열 출력이 다르게 되어야 하기 때문에<br>
길이만큼 반복해 복합 연산자를 이용해 홀 수일 때는 `수`를, 짝 수일 때는 `박`을 문자열에 추가해 준다. <br>


<br>
<br>



### 풀이 (삼항 연산자 풀이)

```java
class Solution {
    public String solution(int n) {
        
        String answer = "";

        for(int i=1; i<=n; i++){
         answer += i%2==1? "수":"박";
        }
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/116888957-02972180-ac67-11eb-9b7a-7d7a0b592b5e.png)

<br><br>

