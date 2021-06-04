---
title: "[Spring] 각 영역의 명명 규칙"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 각 영역의 Naming Convention(명명 규칙)

프로젝트를 3-tier로 구성하는 가장 일반적인 설명은 '유지 보수'에 대한 필요성 때문이다.<br>
각 영역은 독립적으로 설계되어 나중에 특정한 기술이 변하더라도 필요한 부분을 전자제품의 부품처럼 쉽게 교환할 수 있게 하자는 방식이다.<br>
따라서 각 영역은 설계 당시부터 영역을 구분하고, 해당 연결 부위는 인터페이스를 이용해서 설계하는 것이 일반적인 구성방식이다.<br>
<br>

프로젝트를 진행할 때에는 다음과 같은 네이밍 규칙을 가지고 작성한다.<br>
- **xxController** : 스프링 MVC에서 동작하는 Controller 클래스를 설계할 때 사용한다.

- **xxService**, **xxServiceImpl** : 비즈니스 영역을 담당하는 인터페이스는 'xxxService'라는 방식을 사용하고, 인터페이스를 구현한 클래스는 'xxxServiceImpl'이라는 이름을 사용한다.

- **xxxDAO**, **xxxRepository** : DAO(Data-Access-Object)나 Repository(저장소)라는 이름으로 영역을 따로 구성하는 것이 보편적이다.

- **VO**, **DTO** : VO와 DTO는 일반적으로 유사한 의미로 사용하는 용어로, 데이터를 담고 있는 객체를 의미한다라는 공통점이 있다.<br>
다만, VO의 경우는 주로 Read Only의 목적이 강하고, 데이터 자체도 Immutable(불변)하게 설계하는 것이 정석이다.<br>
DTO는 주로 데이터 수집의 용도가 좀 더 강하다.<br>

<br><br>


### 패키지의 Naming Convention

패키지의 구성은 프로젝트의 크기나 구성원들의 성향으로 결정한다.<br>
규모가 작은 프로젝트는 Controller 영역을 별도의 패키지로 설계하고, Service 영역 등을 하나의 패키지로 설계할 수 있다.<br>

반면에, 프로젝트의 규모가 커져서 많은 Service 클래스와 Controller들이 혼재할 수 있다면 비즈니스를 단위별로 패키지를 작성하고 다시 내부에서 Controller 패키지, Service 패키지 등으로 다시 나누는 방식을 이용한다.<br>
이런 방식은 담당자가 명확해지고, 독립적인 설정을 가지는 형태로 개발하기 때문에 큰 규모의 프로젝트에 적합하다.<br> 
다만, 패키지가 많아지고, 구성이 복잡하게 느껴지는 단점이 있다.<br>

<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>