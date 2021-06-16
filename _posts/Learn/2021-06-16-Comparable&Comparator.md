---
title: "[JAVA] Comparable과 Comparator"
excerpt: "JAVA"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---


## Comparable과 Comparator

Comparable과 Comparator는 모두 인터페이스이기 때문에 사용하고자 한다면 인터페이스 내에 선언된 메소드를 '반드시' 구현해야 한다.<br>



<br><br>

### 공식 API

[Comparable] <https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html#method.summary>

<br>

`int compareTo(T o)` 메소드가 선언되어있으며, Comparable을 사용할 때 compareTo 메소드를 구현해야 한다.<br>



<br>

[comparator] <https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#method.summary>


<br>

선언된 메서드가 많지만, 실질적으로 `compare(T o1, T o2)`만을 구현하면 된다.<br>



<br><br>

### Comparable과 Comparator의 차이

두 인터페이스는 객체를 비교할 수 있도록 만드는 역할을 한다.<br>

기본 타입은 비교 연산자를 이용해 서로 비교할 수 있지만, 객체같은 경우 서로 비교할 수 없다.<br>

본질적으로 객체는 사용자가 기준을 정해주지 않는 이상 어떤 객체가 더 높은 우선 순위를 갖는지 판단 할 수 없어서, Comparable / Comparator를 이용해 비교한다.<br>

<br>


- Comparable

**ComparateTo(T o)** -> 자기 자신과 매개변수 객체를 비교 <br>
자기 자신을 기준으로 상대방과의 차이 값을 비교하여 반환<br>
lang 패키지에 속해있어 import를 해줄 필요가 없다.<br>


<br>

- Comparator

**Compare(T o1, T o2)** -> 두 매개변수 객체를 비교<br>
자기 자신의 상태와 상관없이 파라미터로 들어오는 두 객체를 비교하여 반환<br>
util 패키지에 속해있어 import 해줘야 한다.<br>


<br><br>

[참고 사이트] <https://st-lab.tistory.com/243>