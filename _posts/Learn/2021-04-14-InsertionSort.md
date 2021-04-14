---
title: "Insertion Sort(삽입정렬)"
excerpt: "Algorithm"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---


## '삽입 정렬'이란?

배열의 모든 요소를 앞에서부터 차례대로 비교하여 자신의 위치를 찾아 삽입하는 것이다.<br>


<br>


## 시간 복잡도

`O(N²)` 시간 복잡도를 가진다.


## 삽입 정렬 구현


배열의 두 번째 데이터부터 연산을 시작한다<br>
비교 대상은 기준보다 -1된 데이터로, 기준 데이터가 비교 데이터보다 클 때까지 뒤에서부터 앞으로 비교가 진행된다<br>
j가 j>=0조건에 부합하지 않아서 for문을 빠져나왔을 때 `arr[j+1] == arr[0]`이 된다.<br>
temp<arr[j]  조건에 부합하지 않아서 for문을 빠져나왔을 때 arr[j]가 temp보다 작다는 의미이기 때문에 arr[j+1]에 temp를 넣어준다.<br>


- 코드

```java
import java.util.Scanner;

public class Example {

	public static void main(String[] args) {
		
		int[] arr = {9,5,4,7,1};
		
		int temp =0; // 기준값
		int j = 0; // 비교값
		
		for(int i=1; i<arr.length; i++) {
			temp = arr[i];
			for(j=i-1; j>=0 && temp<arr[j]; j--) { // j는 i보다 앞에 있는 index
				arr[j+1] = arr[j]; // arr[j+1] == 기준값을 가지고있는 인덱스
			}
			arr[j+1] = temp;
		}
		
		for(int i=0; i<arr.length; i++) {
			System.out.print(arr[i]);
		}
	}
}
```

<br><br>