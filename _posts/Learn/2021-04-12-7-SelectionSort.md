---
title: "Selection Sort(선택정렬)"
excerpt: "Algorithm"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---


## '선택 정렬'이란?

정렬되지 않은 전체 자료 중에서 해당 위치에 맞는 자료를 선택하여 위치를 교환하는 정렬 방식이다.<br>
입력 데이터 이외에 다른 추가 메모리를 요구하지 않는다.<br>
알고리즘이 단순하고 사용할 수 있는 메모리가 제한적인 경우에 사용하면 성능상의 이점이 있다.<br>


<br>


## 시간 복잡도


반복문을 두 번 중첩해서 사용하기 때문에 Best,Worst모두 `O(N²)` 시간 복잡도를 가진다.<br>
<br>


## 선택 정렬 구현


첫 번째 자료를 두 번째 자료부터 마지막 자료까지 차례대로 비교해 가장 작은 값을 찾아 첫 번째에 놓는다.<br>
회차가 증가될 때마다 기준값의 인덱스가 증가하고, 제일 작은 값은 앞에서부터 차례대로 정렬된다.<br>
마지막 요소는 자연스럽게 정렬되기 때문에 -1만큼 반복된다.<br>


- 코드

```java
public class Example {
	public static void main(String[] args) {
		
		int[] arr = {5,7,1,8,3};
		int min; // 최소값
		int temp;
		
		for(int i=0; i<arr.length-1; i++) {
			min = i;
			for(int j=i+1; j<arr.length; j++) {
				if(arr[min]>arr[j]) {
					min=j;
				}
			}
			temp = arr[min];
			arr[min] = arr[i];
			arr[i] = temp;
		}
	
		for(int i=0; i<arr.length;i++) {
			System.out.println(arr[i]);
		}
	}

}
```

<br><br>