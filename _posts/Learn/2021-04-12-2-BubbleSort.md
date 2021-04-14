---
title: "Bubble Sort(버블정렬)"
excerpt: "Algorithm"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---


## '버블 정렬'이란?

일반적으로 사용되는 분류 알고리즘으로, 서로 인접한 데이터들을 비교하며 가장 큰 데이터를 가장 뒤로(오른쪽) 보내며 정렬하는 방식이다.<br>


## 시간 복잡도
Worst :  O(N²) <br> 
Best : 이미 정렬된 숫자를 버블 정렬할 때는 `swap`이 발생하지 않아 O(n)의 시간 복잡도를 가진다.

<br>

## 버블 정렬 구현


두 데이터를 묶어서 비교하기 때문에 입력된 데이터보다 -1 된 반복 횟수를 가진다.<br>
비교 회차가 증가할 때마다 최종 위치가 차례대로 고정되기 때문에 반복 횟수가 줄어들게 된다.<br>

- 코드

```java
import java.util.Scanner;

public class Example1 {
	
	public static void main(String args[]) {
		Scanner sc = new Scanner(System.in);
		
		System.out.print("배열의 크기:");
		int n = sc.nextInt();
		
		int[] arr = new int[n];
		
		
		// 난수를 생성해서 인덱스에 차례대로 값 대입
		for(int i=0; i<arr.length; i++) {
			arr[i] = (int)(Math.random()*n+1);
		}

		
		// 버블 정렬로 오름차순 정렬 
		for(int i=0; i<arr.length-1; i++) {
			for(int j=0; j<arr.length-i-1; j++) {
				if(arr[j]>arr[j+1]) {
					int temp = arr[j+1];
					arr[j+1] = arr[j];
					arr[j] = temp;
				}
			}
		}
	}
}
```

<br><br>