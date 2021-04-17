---
title: "Process와 Thread"
excerpt: "OS"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---

## 프로세스란?


정적 상태였던 프로그램을 실행하게되면 컴퓨터 메모리에 올라가며 동적인 상태가 되며, 이 상태의 프로그램을 프로세스라고 한다.<br>

간단하게 얘기해 실행되고 있는 컴퓨터 프로그램으로, 메모리에 올라와 실행되고 있는 프로그램의 인스턴스(독립적인 개체)를 의미한다. <br>
운영체제로부터 자원을 할당받은 작업이다.<br>
<br>

현재 여러 작업을 하는 프로그램이 많아지면서 프로세스 하나만을 사용해서 프로그램을 실행하기가 어려워졌다.<br>
하지만 운영체제는 안전성을 위해서 프로세스마다 자신에게 할당된 메모리 내의 정보에만 접근 가능하도록 제약을 두고있어서 이를 벗어나는 정보에 접근하려면 오류가 발생하기 때문에 프로세스 여러 개를 사용하는 방법을 사용하긴 어렵다.<br>

프로세스는 Code,Data,Stack,Heap의 구조로 되어있는 독립된 메모리영역을 할당받으며, 기본적으로 프로세스당 최소 1개의 스레드(메인스레드)를 가지고 있다.<br>




## 스레드란?

스레드는 프로세스 특성의 한계를 해결하기 위해 만들어진 개념으로, 프로세스의 자원을 공유하면서 프로세스 실행 흐름의 일부가 된다.<br>

스레드는 프로세스 내에서 Stack만 따로 할당받고 Code,Data,Heap영역은 공유하기 때문에 한 프로세스 내에서 동작되면 프로세스 내의 주소 공간이나 자원들을 스레드끼리 공유할 수 있다.<br>
한 스레드가 프로세스 자원을 변경하면, 다른 이웃 스레드도 그 변경 결과를 즉시 볼 수 있다.<br>



<br>


## 프로세스와 스레드의 차이점

프로세스는 다른 프로세스의 메모리에 직접 접근할 수 없지만, 스레드는 같은 프로세스 내에서 프로세스의 주소 공간, 자원을 공유할 수 있다.<br>

<br><br>


[`참고자료`](https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html)

<br><br>