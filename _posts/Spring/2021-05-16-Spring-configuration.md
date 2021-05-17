---
title: "[Spring 실습] 환경설정"
excerpt: "Spring Framework"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---



## Spring 개발 도구 설정

> ### JDK 설치

스프링은 버전에 따라 JDK의 제한이 있다.<br>
5x의 경우 JDK 1.8 이상<br>
4x의 경우 JDK 1.6 이상<br>
3x의 경우 JDK 1.5 이상<br>

[JDK 다운로드](https://www.oracle.com/java/technologies/javase-downloads.html)<br>

JDK 설치 후, 환경 변수에서 JAVA_HOME을 설정하고, JDK가 설치된 경로를 지정한다.<br>

시스템 속성 - 고급 - 환경 변수 <br>
<br><br>

> ### Eclipse 혹은 STS 설치

스프링 개발 경험이 없다면 자체적으로 스프링 관련 기능이 추가되어 있는 `STS`를 다운로드 받아서 사용하면 별도의 설정이 필요하지 않아 편리하다.<br>
Eclipse 기반으로 웹 프로젝트를 작성해 본 경헙이 있다면 스프링 관련 플러그인을 추가해서 사용하는 것이 편리하다. 하지만 기타 플러그인들과 호환성 문제가 발생할 수 있다.<br>
<br><br>

> ### Eclipse 실행 환경 편집

이클립스는 기본적으로 JDK가 아닌 JRE를 이용해서 실행되기 때문에 미리 이클립스가 설치된 폴더 내에 `eclipse.ini` 혹은 'sts.ini' 파일을 수정한다.<br>

우선 파일을 텍스트 편집기를 이용해 열고 내용을 추가한다.<br>

```java
-vm
C:/Program Files/Java/jdk1.8.0_202/bin/javaw.exe
```

'-vm' 이라는 설정을 추가하고, JDK 설치 경로 내에 bin 폴더와 javaw.exe 파일로 지정하는 내용이다.<br>

파일 수정 후 문제가 발생할 수 있으므로 반드시 기존의 `바로가기`아이콘을 삭제한 후 다시 추가해주는 것이 좋다.<br>
<br><br>

> ### workspace UTF-8 설정

Eclipse Preferences에서 `enco`를 검색한 뒤, Workspace, Css, HTML, JSP 인코딩을 UTF-8로 변경한다.<br>
<br><br>

> ### Eclipse를 이용하는 경우 스프링 플러그인 설치

Eclipse를 이용하는 경우 스프링 개발을 위해서 추가적인 플러그인을 설치해야만 한다.<br>

Eclipse Marketplace에서 sts3을 설치할 수 있다.<br><br>


> ### Tomcat 서버 설정

Tomcat 다운로드 후, Eclipse의 Preferences 메뉴의 Server Runtime Environments항목을 통해 추가한다.<br><br>
<br><br>

## 스프링 프로젝트 생성

스프링 관련 플러그인을 설치하면 별도의 설정 없이 Maven을 사용하는 스프링 프로젝트를 생성할 수 있다.<br>

`Spring Legacy Project`메뉴를 이용하면 여러 종류의 스프링 기반 프로젝트를 Maven 기반으로 생성할 수 있다.<br>

프로젝트는 `Spring MVC Project`를 이용해서 생성한다.<br>

프로젝트를 최초로 생성하면 필요한 코드와 라이브러리를 다운하게 되는데, 다운로드 하는 라이브러리들은 사용자 폴더 내 `.m2`라는 이름의 폴더를 이용한다.<br>
<br>

> ### 스프링 버전 변경

'Spring Legacy Project'메뉴를 이용해서 생성하는 프로젝트는 스프링의 버전은 3.x이고, JDK환경은 1.6기준으로 작성되어 있다.<br>
앞으로 실습할 내용은 스프링 5버전을 이용하기 위해 pom.xml 파일의 내용을 수정한다.<br>

`Maven Repository`에서 스프링 5버전을 찾아 
Spring Context -> 5.0.7버전으로 변경한다.<br>

수정 후 로딩이 완료되면 'Maven Dependencies'항목을 통해서 스프링 프레임워크 라이브러리들이 제대로 변경되었는지 확인한다.<br><br>

> ### Java version 변경

pom.xml 태그 중 maven-compiler-plugin의 내용을 1.6에서 1.8로 수정한다.<br>
그 후, 프로젝트를 우클릭 후 Maven -> Update Project를 진행한다.<br><br><br>


## Tomcat을 이용한 프로젝트 실행 확인

작성된 프로젝트가 정상적으로 동작하는지 Run As -> Run on Server를 이용해 프로젝트를 실행해서 확인한다.<br><br><br>

## Lombok 라이브러리 설치

Lombok을 이용하면 Java개발 시 자주 사용하는 `getter/setter, toString(), 생성자` 등을 자동으로 생성해줘서 약간의 코드만으로도 필요한 클래스를 설계할 때 유용한다.<br>
Lombok은 다른 jar파일들과 달리 프로젝트의 코드에서만 사용되는 것이 아니라 Eclipse 에디터 내에서도 사용되어야 하기 때문에 별도로 설치한다.<br>

[Lombok 다운로드](https://projectlombok.org/) <br>

1.18.2버전을 다운받은 후, 다운로드된 경로에서 명령 프롬포트창에서 'java -jar lombok.jar' 명령어를 통해서 실행한다.<br>

Eclipse의 설치 경로를 찾지 못하는 경우 지정해서 설치한다.<br>

설치가 끝나면 Eclipse의 실행 경로(폴더)에 lombok.jar 파일이 추가된다.<br>

만약 lombok설치 후 바탕화면의 바로가기가 제대로 동작하지 않는다면 삭제 후 다시 생성한다.<br><br><br>


## Java Configuration을 하는 경우

Eclipse를 통해 생성하는 'Spring legacy Project'는 xml기반으로 스프링 관련 설정을 한다.<br>
스프링 3버전 이후에는 Java 클래스 파일을 이용하는 설정을 지원한다.<br>
최근에는 XML과 별개로 Java를 이용하는 설정도 점점 증가하고 있다.<br>

두 가지 모두 실습하기 위해 Java 설정을 이용하는 예제는 j를 붙여 프로젝트를 생성하겠다.<br><br>

> ### XML파일 삭제

j가 붙은 프로젝트의 web.xml과 servlet-context.xml, root-context.xml 파일을 삭제한다.<br>
servlet-context.xml과 root-context.xml파일은 spring 폴더 내에 있어서 폴더 자체를 삭제한다.<br>

web.xml을 삭제하면 과거의 웹 프로젝트들이 기본적으로 web.xml을 사용하는 것을 기본으로 설정했기에 pom.xml에 에러가 발생한다.<br>
pom.xml 하단부에 아래의 설정을 추가한다.<br>

```java
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-war-plugin</artifactId>
    <version>3.2.0</version>
    <configuration>
        <failOnMissingWebXml>false</failOnMissingWebXml>
    </configuration>
</plugin>
```

pom.xml의 스프링 버전을 5.0.7로 수정하고, 컴파일 관련 버전도 1.8버전으로 수정한 후, Maven > Update Project를 실행한다.<br><br>



> ### @Configuration

Java 설정을 이용하는 경우에는 XML 대신 설정 파일을 직접 작성해야 된다.<br>
@Configuration 어노테이션을 이용해 해당 클래스의 인스턴스를 이용해서 설정 파일을 대신한다.<br>

프로젝트 내에 `org.zerock.config`라는 폴더를 생성하고 RootConfig 클래스를 작성한다.<br>
클래스 내부에 어노테이션을 작성한다.<br>

```java
package org.zerock.config;

import org.springframework.context.annotation.Configuration;

@Configuration
public class RootConfig {
	
}
```
<br><br>

> ### web.xml을 대신하는 클래스 작성

기존 프로젝트에서는 web.xml을 이용해서 스프링을 구동시켰으나, XML을 사용하지 않는 경우에는 이 역할을 대신하는 클래스를 작성해서 처리한다.<br>
org.zerock.config 패키지 내에 WebConfig 클래스를 생성한다.<br>
그 후 해당 클래스가 `AbstractAnnotationConfigDispatcherServletInitializer` 추상 클래스를 상속하도록 하고 추상 메서드를 오버라이드 하도록 작성한다.<br>
생성된 getRootConfig() 클래스는 'root-context.xml'을 대신하는 클래스를 지정한다.<br>
실습에서는 RootConfig 클래스를 사용하므로 메서드의 내용을 변경한다.<br>

```java
package org.zerock.config;

import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

public class WebConfig extends AbstractAnnotationConfigDispatcherServletInitializer
 {

	@Override
	protected Class<?>[] getRootConfigClasses() {
		return new Class[] {RootConfig.class};
	}

	@Override
	protected Class<?>[] getServletConfigClasses() {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	protected String[] getServletMappings() {
		// TODO Auto-generated method stub
		return null;
	}

}
```
<br><br>


관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>