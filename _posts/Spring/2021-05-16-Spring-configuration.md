---
title: "[Spring 실습] 환경설정"
excerpt: "Spring Framework"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---



## Spring 개발 도구 설정

> JDK 설치

스프링은 버전에 따라 JDK의 제한이 있다.<br>
5x의 경우 JDK 1.8 이상<br>
4x의 경우 JDK 1.6 이상<br>
3x의 경우 JDK 1.5 이상<br>

[JDK 다운로드](https://www.oracle.com/java/technologies/javase-downloads.html)<br>

JDK 설치 후, 환경 변수에서 JAVA_HOME을 설정하고, JDK가 설치된 경로를 지정한다.<br>

시스템 속성 - 고급 - 환경 변수 <br>
<br>

> Eclipse 혹은 STS 설치

스프링 개발 경험이 없다면 자체적으로 스프링 관련 기능이 추가되어 있는 `STS`를 다운로드 받아서 사용하면 별도의 설정이 필요하지 않아 편리하다.<br>
Eclipse 기반으로 웹 프로젝트를 작성해 본 경헙이 있다면 스프링 관련 플러그인을 추가해서 사용하는 것이 편리하다. 하지만 기타 플러그인들과 호환성 문제가 발생할 수 있다.<br>
<br>

> Eclipse 실행 환경 편집

이클립스는 기본적으로 JDK가 아닌 JRE를 이용해서 실행되기 때문에 미리 이클립스가 설치된 폴더 내에 `eclipse.ini` 혹은 'sts.ini' 파일을 수정한다.<br>

우선 파일을 텍스트 편집기를 이용해 열고 내용을 추가한다.<br>

```java
-vm
C:/Program Files/Java/jdk1.8.0_202/bin/javaw.exe
```

'-vm' 이라는 설정을 추가하고, JDK 설치 경로 내에 bin 폴더와 javaw.exe 파일로 지정하는 내용이다.<br>

파일 수정 후 문제가 발생할 수 있으므로 반드시 기존의 `바로가기`아이콘을 삭제한 후 다시 추가해주는 것이 좋다.<br>
<br>

> workspace UTF-8 설정

Eclipse Preferences에서 `enco`를 검색한 뒤, Workspace, Css, HTML, JSP 인코딩을 UTF-8로 변경한다.<br>
<br>

> Eclipse를 이용하는 경우 스프링 플러그인 설치

Eclipse를 이용하는 경우 스프링 개발을 위해서 추가적인 플러그인을 설치해야만 한다.<br>

Eclipse Marketplace에서 sts3을 설치할 수 있다.<br><br>


