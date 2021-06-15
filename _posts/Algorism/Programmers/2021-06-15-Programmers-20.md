---
title: "[프로그래머스] 문자열 다루기 기본 (JAVA)"
excerpt: "Algorism"
search: true
categories: 
  - Algorism
tags: 
  - Algorism
  - JAVA
---


### 문제 설명
문자열 s의 길이가 4 혹은 6이고, 숫자로만 구성돼있는지 확인해주는 함수, solution을 완성하세요.<br> 예를 들어 s가 "a234"이면 False를 리턴하고 "1234"라면 True를 리턴하면 됩니다.<br>

<br><br>

- 제한 조건 

s는 길이 1 이상, 길이 8 이하인 문자열입니다.<br>



<br>

- 예시 

![image](https://user-images.githubusercontent.com/73421820/122063022-df958b00-ce2a-11eb-9330-86f0ba33c0d9.png)

<br>





<br><br>


### 풀이



```java
class Solution {
    public boolean solution(String s) {
        
        char[] arr = {'1','2','3','4','5','6','7','8','9','0'};
        
        int count = 0;
        
        if(s.length()==4){
                    
            for(int i=0; i<s.length(); i++){ 
                
                for(int j=0; j<arr.length;j++){
                    
                   if(s.charAt(i)==arr[j]){
                       count++;
                   }          
                }                            
            }
            
        }else if(s.length() ==6){
            for(int i=0; i<s.length(); i++){               
                for(int j=0; j<arr.length;j++){
                    
                   if(s.charAt(i)==arr[j]){
                       count++;
                   }                  
                }                            
            }
        }
        
        System.out.println(count);
        
        
        if(count == 4 || count == 6){
            return true;
        }else{
            return false;
        }
        
    }
}
```

<br>

- 실행 결과

![image](https://user-images.githubusercontent.com/73421820/122062879-c42a8000-ce2a-11eb-82b8-e58c346aa8b0.png)



<br>

<br>

숫자가 담긴 배열을 만든 후(문자열에서 자르면 문자 형식이기 때문에 문자로 담는다.), 문자열을 하나씩 비교해서 개수를 세는 방식으로 풀었다.<br>


<br><br>

### 다른 사람 풀이

```java
class Solution {
  public boolean solution(String s) {
    return (s.length() != 4 && s.length() != 6) || (s.split("[0-9]").length > 0) ? false:true;
  }
}
```

<br>


난 40줄이나 나오는 코드를 다른 사람은 5줄만에 끝냈다...😂<br>

우선 문자열의 길이가 4와 6과 같지 않고, 문자열을 숫자로 잘랐을 때 길이가 0보다 크다면(split("[0-9")는 숫자는 다 잘려나가고 숫자가 아닌것만 남게된다.)<br> 
false를 반환하고, 아니라면 true를 반환한다.<br>

<br>

split은 문자열을 공백 기준으로 나눴을 때만 사용했던것 같은데 숫자를 기준으로 잘랐을 때는 숫자는 다 사라지고 문자만 남는다는 것을 배웠다.<br>
<br>

정규표현식 : [0-9] 숫자<br>



<br><br>
<br><br>