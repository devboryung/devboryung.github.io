---
title: "Garbage Collection란?"
excerpt: "JAVA-GC"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---


## GC란?

JVM의 Heap 영역에서 사용하지 않는 객체를 삭제하는 프로세스이다.<br>

Heap영역에는 Object타입의 데이터들이 등록되는데, 어떤 객체에 유효한 참조가 존재한다면 Reachable객체, 참조가 존재하지 않는다면 UnReachable 객체라고 한다.<br>
GC는 UnReachable객체를 수거한다.<br>

<br>

## GC의 동작순서

Mark and Sweep 알고리즘<br>

1. 모든 변수를 스캔하면서 어떤 객체를 참조하고 있는지 `Mark`한다.
2. Unreachable객체를 Heap영역에서 제거한다.
3. 제거 후 사이 사이 공간이 남아있다면 분산된 객체들을 Heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 나누어주어 메모리의 단편화를 막아준다. (compact과정)

<br>


## JVM의 메모리 관리

JVM에는 `Young Generation` / `Old Generation`이라는 두 가지 물리적인 공간이 존재한다<br>
Young Generation은 새롭게 생성한 객체가 위치하는 영역으로  Eden, Survivor1, Survivor2로 나눠진다.<br>
객체가 처음 생성되면 Eden영역에 위치하게 된다.<br>
Eden 영역이 가득 차서 새로운 객체가 들어오지 못할 때 GC가 발생하는데, 이때 발생하는 GC를 `Minor GC`라고 한다.<br>
이 때 살아남은 reachable객체는 servivor영역으로 옮겨지게 되는데, minor GC가 발생할 때마다 servivor1영역이 채워져있다면 2로, 2가 채워져있다면 1로 옮겨진다 (둘 중에 한 영역은 무조건 비워져있어야 한다.)<br>
servivor영역에 살아남은 객체들은 age값이 증가하게 된다.<br>
만약 age값이 설정된 임계점에 도달하게 되면 Old Generation영역으로 옮겨지게 되며, Old Generation영역이 가득차서 발생하는 GC를 `Major GC`라고 한다.<br>


## Stop The World

GC를 실행시키기 위해 JVM이 어플리케이션 실행을 멈춰 GC 쓰레드 이외의 쓰레드들의 작업이 멈춘다.<br>


<br><br>




