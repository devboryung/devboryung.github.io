---
title: "[Spring] 의존성 주입 테스트"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---

## 의존성 주입 테스트

스프링에서는 생성자를 이용한 주입과 setter 메서드를 이용한 주입으로 의존성 주입을 구현한다.<br>
설정 방식은 주로 XML이나 어노테이션을 이용해서 처리한다.<br>
실습 시 Lombok을 이용해서 setter 메서드를 자동으로 구현되도록하여 스프링의 동작을 테스트 할 것이다.<br>

pom.xml에서 Lombok 라이브러리를 추가하고, spring-test 라이브러리를 이용한다.<br>

<br>

>추가되는 라이브러리 <br>

생성된 프로젝트의 Log4j 라이브러리는 1.2.15로 설정되어 있으니 Log4j 1.2.17버전을 추가하고, 기존 부분은 삭제하거나 주석처리 한다.

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-test</artifactId>
  <version>${org.springframework-version}</version>
</dependency>
<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <version>1.18.0</version>
  <scope>provided</scope>
</dependency>
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>1.2.17</version>
</dependency>
```

<br>

>변경되는 라이브러리<br>

junit 4.7 -> 4.12로 변경

```xml
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<scope>test</scope>
		</dependency>     
```

<br><br>

## 실습

- ### 클래스 생성

ex00 프로젝트에 'org.zerock.sample'패키지 생성 후, Restaurant 클래스와 Chef 클래스 생성한다.<br>

일반적으로 스프링에서 의존성 주임은 Chef 클래스가 아닌 인터페이스로 설계하는것이 좋지만, 최소한의 코드만을 이용해서 의존성 주입을 테스트해보기 위한 것이므로 클래스로 설계한다.<br><br>

> Chef 클래스

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

> Restaurant 클래스

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

위 코드가 의미하는 것은 Restaurant 객체는 Chef 타입의 객체를 필요로 한다는 상황이다.<br>
`@Component`는 스프링에게 해당 클래스가 스프링에서 관리해야 하는 대상임을 표시하는 어노테이션이다.<br>
`@Setter`는 자동으로 setChef()를 컴파일 시 생성한다.<br>
`onMethod` 속성은 생성되는 setChef()에 `@Autowired` 어노테이션을 추가하도록 한다.<br>



<br><br>


### XML 을 이용하는 의존성 주입 설정

스프링은 클래스에서 객체를 생성하고 객체들의 의존성에 대한 처리 작업까지 내부에서 모든 것이 처리된다.<br>

스프링에서 관리되는 객체를 흔히 `Bean`이라고 하고, 이에 대한 설정은 XML과 Java를 이용해서 처리할 수 있다.<br>
아직까지는 XML 방식을 선호하지만, 점점 Java를 이용하는 설정도 많이 사용되고 있다.<br>
프로젝트의 src폴더 내의 'root-context.xml'은 스프링 프레임워크에서 관리해야 하는 객체를 설정하는 설정파일이다.<br>


root-context.xml의 아래쪽의 Namespaces 탭의 context 항목을 체크한다.<br>
Source탭을 선택해서 코드를 추가한다.<br>

```xml
<context:component-scan base-package="org.zerock.sample">
</context:component-scan>
```

<br>

Bean Graph탭을 선택해 보면 Restaurant와 Chef 객체가 설정된 것을 확인할 수 있다.<br><br>


## 스프링이 동작하면서 생기는 일

1. 스프링 프레임워크가 시작되면 먼저 스프링이 사용하는 메모리 영역을 만들게 되며 이를 컨텍스트라고 한다.<br> 스프링에서는 ApplicationContext라는 이름의 객체가 만들어진다.<br><br>
2. 스프링은 자신이 객체를 생성하고 관리해야 하는 객체들에 대한 설정이 필요하다.<br> 이 설정이 root-context.xml 파일이다.<br><br>
3. root-context.xml에 설정되어있는 `context:component-scan` 태그의 내용을 통해서 'org.zerock.sample' 패키지를 스캔한다.<br><br>
4. 해당 패키지에 있는 클래스들 중에서 스프링이 사용하는 @Component 어노테이션이 존재하는 클래스의 인스턴스를 생성한다.<br><br>
5. Restaurant 객체는 Chef 객체가 필요하다는 어노테이션(@Autowired) 설정이 있으므로, 스프링은 Chef 객체의 레퍼런스를 Restaurant 객체에 주입한다.<br><br><br>

> ### 테스트 코드를 통한 확인

'src/test/java' 폴더 내에 'org.zerock.sample.SampleTests' 클래스를 추가한다.<br>
이 클래스는 spring-test 모듈을 이용해서 간단하게 스프링을 가동시키고 동작하게 한다.<br>
이 때 Junit은 반드시 4.10 이상의 버전을 사용해야 한다.<br><br>

SampleTests 클래스

```java
package org.zerock.sample;

import static org.junit.Assert.assertNotNull;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import lombok.Setter;
import lombok.extern.log4j.Log4j;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml")
@Log4j
public class SampleTests {
	
	@Setter(onMethod_ = {@Autowired} )
	private Restaurant restaurant;
	
	@Test
	public void testExist() {
		
		assertNotNull(restaurant);
		
		log.info(restaurant);
		log.info("-------------------------------------------------");
		log.info(restaurant.getChef());
	}
}
```

<br>


@Runwith : 현재 테스트 코드가 스프링을 실행하는 역할을 하는 것이라는 표시.<br><br>
@ContextConfiguration : 지정된 클래스나 문자열을 이용해서 필요한 객체들을 스프링 내에 객체(빈)로 등록하게 한다.<br> 'classpath:'나 'file:'을 이용하여 root-context.xml의 경로를 지정할 수 있다.<br><br>
@Log4j : Lombok을 이용해서 로그를 기록하는 Logger를 변수로 생성한다.<br> 별도의 Logger 객체의 선언 없이 Log4j 라이브러리와 설정이 존재한다면 바로 사용할 수 있다.<br>
Spring Legacy Proejct로 생성하는 경우 기본으로 Log4j와 해당 설정이 완료되는 상태이다.<br><br>
@Autowired : 해당 인스턴스 변수가 스프링으로부터 자동으로 주입해 달라는 표시이다.<br><br>
@Test : JUnit에서 테스트 대상을 표시하는 어노테이션이다.<br> 해당 메서드를 선택하고 JUnit Test 기능을 실행한다.<br><br>
assertNotNull() : restaurant 변수가 null이 아니어야만 테스트가 성공한다는 것을 의미한다<br><br><br>


> 테스트 결과

```java
INFO : org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor - JSR-330 'javax.inject.Inject' annotation found and supported for autowiring
INFO : org.zerock.sample.SampleTests - Restaurant(chef=Chef())
INFO : org.zerock.sample.SampleTests - -------------------------------------------------
INFO : org.zerock.sample.SampleTests - Chef()
INFO : org.springframework.context.support.GenericApplicationContext - Closing org.springframework.context.support.GenericApplicationContext@55d56113: startup date [Tue May 18 00:36:27 KST 2021]; root of context hierarchy
```

<br>

1. new Restaurant()와 같이 Restaurant 클래스에서 객체를 생성한 적이 없는데도 객체가 만들어 졌다.<br>
스프링은 관리가 필요한 객체(Bean)를 어노테이션 등을 이용해서 객체를 생성하고 관리하는 일종의 '컨테이너'나 '팩토리' 기능을 가지고 있다.<br><br>
2. Restaurant 클래스의 @Data 어노테이션으로 Lombok을 이용해서 여러 메소드가 만들어 진다.<br>
Lombok은 자동으로 getter/setter 등을 만들어 주는데 스프링은 생성자 주입 혹은 setter 주입을 이용해서 동작한다.<br>
Lombok을 통해서 getter/setter 등을 자동으로 생성하고 'onMethod' 속성을 이용해서 작성된 setter에 @Autowired 어노테이션을 추가한다.<br><br>
3. Restaurant 객체의 Chef 인스턴스 변수에 Chef 타입의 객체가 주입되었다.<br>
스프링은 @Autowired와 같은 어노테이션을 이용해서 개발자가 직접 객체들과의 관계를 관리하지 않고, 자동으로 관리되도록 한다.<br><br><br>


> ### 중요한 점

1. 테스트 코드가 실행되기 위해서 스프링 프레임워크가 동작한다.<br>
2. 동작하는 과정에서 필요한 객체들이 스프링에 등록된다.<br>
3. 의존성 주입이 필요한 객체는 자동으로 주입이 된다.<br>


<br><br>




관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>