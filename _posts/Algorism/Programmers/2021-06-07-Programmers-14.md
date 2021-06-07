---
title: "[프로그래머스] 2016년 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
2016년 1월 1일은 금요일입니다. <br>
2016년 a월 b일은 무슨 요일일까요? <br>
두 수 a ,b를 입력받아 2016년 a월 b일이 무슨 요일인지 리턴하는 함수, solution을 완성하세요. <br>
요일의 이름은 일요일부터 토요일까지 각각 `SUN,MON,TUE,WED,THU,FRI,SAT` 입니다.<br>
예를 들어 a=5, b=24라면 5월 24일은 화요일이므로 문자열 "TUE"를 반환하세요.<br>

<br><br>

- 제한 조건 

2016년은 윤년입니다.<br>
2016년 a월 b일은 실제로 있는 날입니다. (13월 26일이나 2월 45일같은 날짜는 주어지지 않습니다).<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/120959470-cb032400-c794-11eb-8597-aa44f5112188.png)<br>





<br><br>


### 풀이

```java
import java.text.*;
import java.util.*;

class Solution {
    public String solution(int a, int b) {
        
        String m = Integer.toString(a);
        String d = Integer.toString(b);
        if(a<10){
            m = "0"+m;
        }
        
        if(b<10){
            d = "0"+d;
        }
        String day = "2016" + m+ d; // 구할 날짜 
       
    
        DateFormat dateFormat = new SimpleDateFormat("yyyyMMdd");
    
    
        String[] dayOfWeek = {"SUN","MON","TUE","WED",
"THU","FRI","SAT"}; // 요일 출력
        Calendar calendar = Calendar.getInstance();
        
        try{        
        Date date = dateFormat.parse(day);
        calendar.setTime(date);
        }catch(Exception e){
            e.printStackTrace();
        }        
        
        String answer = dayOfWeek[calendar.get(Calendar.DAY_OF_WEEK)-1];
        return answer;
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/120959500-dd7d5d80-c794-11eb-84cc-0eb37116827c.png)



<br>

a, b가 입력받을 때 한 자릿수라면 두 자리로 변환할 수 있도록 if 문으로 처리해 준다.<br>

SimpleDateFormat 클래스를 이용해서 원하는 포맷 형식을 만든다.<br>

요일이 원하는 형식으로 출력될 수 있도록 배열을 선언한다.<br>


Calendar 객체를 생성한 후, 입력받은 날짜를 Date타입으로 변환한 후, Calendar형식에서 날짜를 가져온다.<br>
SimpleDateFormat.parse(String)메소드는 예외를 던지기 때문에 예외처리를 해줘야 된다.<br>

> 바로 new Date로 Date 클래스를 가져오지 않는 이유는 new Date는 특정 시간의 날짜를 대입하여 계산하고, Calendar의 경우는 1900년 1월 1일의 기준으로 오늘 날짜의 차이로 날짜를 계산한다는 차이가 있다고 한다.<br>
출처: [명월 일지](https://nowonbun.tistory.com/502 )
<br>

calendar.get(Calendar.DAY_OF_WEEK)는 calendar가 가르키는 특정 날짜가 무슨 요일인지 알려주며, 일요일부터 토요일까지 1에서 7로 나타낸다.<br>

출력 형식이 담긴 배열은 일요일이 0번째 인덱스이기 때문에 -1을 해줘야 원하는 값이 출력된다.<br>





<br><br>
