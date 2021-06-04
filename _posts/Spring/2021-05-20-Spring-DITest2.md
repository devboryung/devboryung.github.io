---
title: "[Spring] 의존성 주입 테스트(2)"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---






## 코드에 사용된 어노테이션 정리


> Chef클래스

```java
package org.zerock.sample;

import org.springframework.stereotype.Component;

import lombok.Data;

@Component
@Data
public class Chef {

}

```

<br>

> Restaurant클래스

```java
package org.zerock.sample;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import lombok.Data;
import lombok.Setter;

@Component
@Data
public class Restaurant {
	
	@Setter(onMethod_ = @Autowired)
	private Chef chef;

}
```

<br>
<br>

- Lombok 관련 어노테이션
    - @Setter
    - @Data
    - @Log4j

- Spring 관련 어노테이션
    - @Autowired
    - @Component

- 테스트 관련 어노테이션
    - @RunWith
    - @ContextConfiguration
    - @Test

<br>
<br><br>

### Lombok 관련

Lombok은 컴파일 시 흔하게 코드를 작성하는 기능들을 완성해주는 라이브러리이다.<br>

> @Setter

@Setter 어노테이션은 Setter 메서드를 만들어주는 역할을 한다.<br>

@Setter에는 3가지의 속성을 부여할 수 있다.<br>
1. value : 접근 제한 속성을 의미한다.<br>
2. onMethod : setter 메서드 생성 시 메서드에 추가할 어노테이션을 지정한다.<br>
3. onParam : setter 메서드의 파라미터에 어노테이션을 사용하는 경우에 적용한다.<br>
<br>

> @Data

@Data는 Lombok에서 가장 자주 사용되는 어노테이션이다.<br>
@ToString, @EqualsAndHashCode, @Getter/@Setter, @RequiredArgsConstructor를 모두 결합한 형태로 한 번에 자주 사용되는 모든 메서드들을 생성할 수 있다는 장점이 있다.<br>
세부적인 설정이 필요 없는 경우라면 @Data를 주로 이용한다.<br>

<br>

> @Log4j

@Log4j는 로그 객체를 생성한다.<br>
Log4j 설정을 이용하고, Log4j가 존재하지 않을 경우에는 @Log를 이용할 수 있다.<br>
@Log를 클래스 쪽에 붙여주면 내부적으로 static final로 Logger 객체가 생성되기 때문에 별도의 로그를 설정할 필요 없이 필요한 코드를 만들어 낼 수 있다.<br>
Spring Legacy Project로 생성한 경우 기본적으로 Log4j 설정이 있기 때문에 추가적인 설정 없이 @Log4j만으로 로그 객체를 준비할 수 있다.<br>
<br><br><br>


### Spring 관련

> @Component 

@Component는 해당 클래스가 스프링에서 객체로 만들어서 관리하는 대상임을 명시한다.<br>

> @Autowired

자신이 특정한 객체의 의존적이므로 자신에게 해당 타입의 빈을 주입해주라는 의미이다.<br>
필요한 객체가 존재하지 않는다면 스프링은 제대로 객체들을 구성할 수 없기 때문에 에러를 발생한다.<br>
<br>

### 테스트 관련

> @ContextConfiguration

@ContextConfiguration은 테스트 관련 어노테이션 중 가장 중요한 어노테이션이다.<br>
스프링이 실행되면서 어떤 설정 정보를 읽어 들여야 하는지를 명시한다.<br>
속성으로는 locations를 이용해서 문자열을 배열로 XML 설정 파일을 명시할 수 있고, classes 속성으로 @Configuration이 적용된 클래스를 지정해 줄 수도 있다.<br>


> @Runwith

테스트 시 필요한 클래스를 지정한다.<br> 
스프링은 SpringJUnit4ClassRunner 클래스가 대상이 된다.<br>

> @Test

junit에서 해당 메서드가 jUnit상에서 단위 테스트의 대상인지 알려준다.<br>


<br><br><br>


## 단일 생성자의 묵시적 자동 주입

스프링의 의존성 주입은 생성자 주입과 Setter 주입을 사용한다.<br>
`Setter`주입은 앞의 예제와 같이 setXXX()와 같은 메서드를 작성하고 @Autowired와 같은 어노테이션을 통해서 스프링으로부터 자신이 필요한 객체를 주입하도록 한다.<br>
`생성자`주입의 경우 객체 생성 시 의존성 주입이 필요하므로 좀 더 엄격하게 의존성 주입을 체크하는 장점이 있다.<br>
기존에 스프링에서는 생성자 주입을 하기 위해서 생성자를 정의하고, @Autowired오 ㅏ같은 어노테이션을 추가해야만 생성자 주입이 이뤄졌지만 스프링 4.3 이후에는 묵시적으로 생성자 주입이 가능하다.<br>

<br><br>

<br><br>




관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>


