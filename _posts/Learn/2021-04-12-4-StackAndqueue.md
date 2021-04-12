---
title: "Stack 과 Queue"
excerpt: "DataStructure"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---


## Stack이란?
스택은 후입선출(LIFO) 형태의 자료구조이다.<br>

### Stack이 지원하는 기능

pop() : 가장 마지막의 데이터를 가져오면서 지운다. <br>
push() : 새로운 데이터를 마지막에 추가한다. <br>
peek() : 가장 마지막 데이터를 조회한다. <br>
isEmpty() : 스택에 데이터가 있는지 없는지 확인한다.<br>

<br>

```java
import java.util.Stack;

public class Example {
	
	public static void main(String[] args) {
		Stack<Integer> s = new Stack<Integer>();
		
		// s에 1,2,3,4,5를 차례대로 넣는다.
		s.push(1);
		s.push(2);
		s.push(3);
		s.push(4);
		s.push(5);
		
		System.out.println(s.pop()); // 5 가져온 후 지우기
		System.out.println(s.pop()); // 4 가져온 후 지우기
		System.out.println(s.peek()); // 3 확인하기
		System.out.println(s.pop()); // 3 가져온 후 지우기
		System.out.println(s.isEmpty()); // false(1,2남아있음)
		System.out.println(s.pop()); //2 가져온 후 지우기
		System.out.println(s.pop()); //1 가져온 후 지우기
		System.out.println(s.isEmpty()); //true
	}
}
```

<br>

### Stack을 이용한 문자 역순 출력

```java
import java.util.Arrays;
import java.util.Scanner;
import java.util.Stack;

public class Example {
	
	public static void main(String[] args) {
		
		String str = "apple";
		
		Stack<Character> s = new Stack<>();
		
		for(int i=0; i<str.length(); i++) {
			s.push(str.charAt(i));
		}
		
		String str2 ="";
		
		while(!s.isEmpty()) {
			str2 += s.pop();
		}
		
		System.out.println(str2);
	}
}

```

<br><br>

## Queue란?
큐는 선입선출(FIFO) 형태의 자료구조이다.<br>


### Queue가 지원하는 기능

offer() : 가장 마지막에 데이터를 넣는다.<br>
poll() : 가장 앞에서 데이터를 꺼낸다.<br>
peek() : 가장 앞 데이터를 조회한다.<br>
isEmpty() : 큐에 데이터가 있는지 없는지 확인한다.<br>

<br>

보통 JAVA에서 Queue는 LinkedList를 이용해서 생성한다.<br>

```java
import java.util.LinkedList;
import java.util.Queue;

public class Example {

	public static void main(String[] args) {
		
		Queue<Integer> q = new LinkedList<>();
		
		q.offer(1);
		q.offer(2);
		q.offer(3);
		q.offer(4);
		q.offer(5);
		
		System.out.println(q.poll()); // 1 가져온 후 지우기
		System.out.println(q.poll()); // 2 가져온 후 지우기
		System.out.println(q.poll()); // 3 가져온 후 지우기
		System.out.println(q.peek()); // 4 확인하기
		System.out.println(q.isEmpty()); // false(4,5 남아 있음)
		System.out.println(q.poll()); // 4 가져온 후 지우기
		System.out.println(q.poll()); // 5 가져온 후 지우기
		System.out.println(q.isEmpty()); // true
	}

}
```


<br><br>

## Stack과 Queue의 차이

stack은 데이터의 삽입,삭제가 한 방향에서만 이루어진다.<br>
queue는 데이터의 입구와 출구가 각각 나뉘어져있다.<br>
스택과 큐의 가장 큰 차이점은 item을 삭제하는 것에 있다.<br>
stack은 가장 마지막에 추가된 item을 삭제하고, queue는 가장 처음에 들어온 item을 삭제한다.

<br><br>

