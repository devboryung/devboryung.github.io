---
title: "라이브러리와 프레임워크"
excerpt: "IT"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---


## 라이브러리란?

애플리케이션 개발을 위해 사용되는 함수 모음이다.<br>
개발을 할 때 반복되는 기능들이 많은데, 반복되는 기능들을 재활용 가능하도록 따로 파일을 만든 후 원하는 프로젝트에 include해서 쉽게 사용할 수 있다.<br>

<br>

### 정적 라이브러리

프로그램을 컴파일하는 과정에서 포함시키는 오브젝트 파일이다.<br>
컴파일 시 링커가 프로그램이 필요로 하는 부분을 라이브러리에서 찾아 실행 파일에 복사 한다.<br>
실행파일이 전부 들어가기 때문에 실행 시 라이브러리가 필요 없지만, 프로그램이 늘어날수록 크기가 커지고 코드 중복이 생길 수 있어 메모리를 낭비할 수 있다.<br>


<br>

### 동적 라이브러리

완성된 프로그램을 실행할 때 포함시키는 라이브러리이다.<br>

필요시 사용할 수 있도록 최소한의 정보만 포함하여 링크한다.<br>

프로그램이 한번 메모리에 올려진 것을 공유하므로 메모리 사용 공간이 정적 라이브러리에 비해 적다.<br>

<br>

## 프레임워크란?

만들고자 하는 프로그램의 전체적인 뼈대를 제공해주고, 개발자가 필요한 코드를 추가하는 것이다.<br>

프레임워크는 개발에 필수적이고 표준적인 부분에 해당하는 설계와 구현을 재사용할 수 있도록 하고, 응용별 특정 기능을 추가적인 사용자 작성 코드에 의해 선택적으로 구현 가능하도록 클래스들로 제공한다.<br>
개발에 드는 시간을 줄이고 세부 요구사항 구현에 집중할 수 있도록 하는것을 목표한다.<br>
하지만 프레임워크 내의 API가 복잡하게 얽혀있어 사용 시 코드가 비대화 되고, 초기 학습 시간이 많이 소요되는 점과, 프레임워크에 의존하여 개발해야 된다는 단점을 가지고 있다<br>



## 라이브러리와 프레임워크의 차이

라이브러리와 프레임워크 모두 소프트웨어 개발을 쉽게 할 수 있도록 돕는 요소이지만 주도권이 어디에 있는지에서 차이가 있다.<br>
프레임워크는 전체적인 흐름을 자체적으로 가지고 있으며, 개발자가 프레임 워크의 규칙에 맞춰서 그 안에 필요한 코드를 작성하는 반면 라이브러리는 사용자가 흐름에 대해 제어를 하며 필요한 상황에 가져다 쓰는 것에서 차이가 있다.<br>

<br><br>



