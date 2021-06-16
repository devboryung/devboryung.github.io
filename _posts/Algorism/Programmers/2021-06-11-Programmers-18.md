---
title: "[프로그래머스] 모의고사 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
수포자는 수학을 포기한 사람의 준말입니다.<br> 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. <br>수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.<br><br>

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...<br>
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...<br>
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...<br>

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.<br>

<br><br>

- 제한 조건 

시험은 최대 10,000 문제로 구성되어있습니다.<br>
문제의 정답은 1, 2, 3, 4, 5중 하나입니다.<br>
가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/121697676-40118900-cb08-11eb-97fa-5434c9f3d55a.png)<br>





<br><br>


### 풀이



```java
import java.util.*;

class Solution {
    public int[] solution(int[] answers) {
        
        // 수포자1
        int[] a = new int[answers.length];
        int n = 1;
        for(int i=0; i<a.length; i++){
            a[i] = n++;
            if(n == 6){
                n = 1;
            }
        }
        
        // 수포자2
        int[] b = new int[answers.length];
        int[] arr = {2,1,2,3,2,4,2,5};
        n = 0;
        for(int i=0; i<b.length; i++){
            b[i] = arr[n++];
            if(n == 8){
                n = 0;
            }
        }
        
        
        // 수포자3
        int[] c = new int[answers.length];
        int[] arr2 = {3,3,1,1,2,2,4,4,5,5};
        n = 0;
        for(int i=0; i<c.length; i++){
            c[i] = arr2[n++];
            if(n == 10){
                n = 0;
            }
        }
        
        // 정답 수 확인용
        int[] score = {0,0,0};
        
        for(int i = 0; i<answers.length; i++){
            if(a[i] == answers[i]){
                score[0]++;
            }
            if(b[i] == answers[i]){
                score[1]++;
            }
            if(c[i] == answers[i]){
                score[2]++;
            }
        }
        
        // 최대 값 계산
        int max = score[0];
        for(int i=0; i<score.length; i++){
            if(max < score[i]){
                max = score[i];
            }
        }
        
        
        // list를 생성해서 높은 점수를 가진 학생 담음
        List<Integer> list = new ArrayList<>();
        // 최고 점수와 같은 학생인 경우 list에 추가, 이때 점수를 추가하는 것이 아닌 학생(인덱스)를 추가하는 것.  [첫 번째 학생 == 0]
        for(int i=0; i<score.length; i++){
            if(max == score[i]){
                list.add(i);
            }
        }
        
        
        // 출력값은 최고 점수와 같은 학생과 길이가 동일해야 한다.
        int[] answer = new int[list.size()];
        for(int i=0; i<list.size(); i++){
            answer[i] = list.get(i)+1; //인덱스는 0부터 시작.
        }
        
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/121697758-51f32c00-cb08-11eb-91ed-84383104f583.png)

<br>

수포자별로 정답 개수를 계산한 후, 그 개수를 수포자랑 어떻게 연관시켜야하는지 몰라서 답을 찾아봤다.ㅠㅠ<br>
그래서 애초에 in[] score라는 정답 수 확인용 배열을 생성해서<br>
첫번째 인덱스 == 수포자 1 이라는 풀이 방법을 확인했다.<br>


<br>

또한, 수포자들의 정답을 반복문을 이용해서 배열의 길이 만큼 값을 넣는게 아닌 %를 통해 값을 비교하는 것도 알아냈다..<br>


```java
// 수포자1
int[] a = {1,2,3,4,5};

// 수포자2
int[] b = {2,1,2,3,2,4,2,5};

// 수포자3
int[] c ={3,3,1,1,2,2,4,4,5,5};


// 정답 수 확인용
int[] score = {0,0,0};

for(int i = 0; i<answers.length; i++){
    if(a[i%5] == answers[i]){
        score[0]++;
    }
    if(b[i%8] == answers[i]){
        score[1]++;
    }
    if(c[i%10] == answers[i]){
        score[2]++;
    }
}
```
<br>

수포자1 경우 1,2,3,4,5를 반복해서 정답을 찍는다.<br>
0%5 = 0 이 되며, 5%5 = 0으로 계속해서 배열을 반복해서 비교할 수 있다.<br>


<br><br>
