---
title: "[ERROR]Spring Log4j 라이브러리 오류"
excerpt: "org.apache.log4j.Logger cannot be resolved to a type"
search: true
categories: 
  - Error
tags: 
  - Error
---

Spring 실습을 하는 중 Log4j에서 오류가 발생했다.<br>

## 오류 상황

![image](https://user-images.githubusercontent.com/73421820/120070464-9ffa3f80-c0c5-11eb-826e-78571509bc8d.png)

Spring MVC 게시판을 만드는 중 `org.apache.log4j.Logger cannot be resolved to a type` 에러가 발생했다.

<br><br>

## 해결

![image](https://user-images.githubusercontent.com/73421820/120070807-4135c580-c0c7-11eb-8eaf-f6561430deb4.png)

POM.XML에 등록된 Log4j의 scope가 runtime으로 되어있어서 그런거였다.<br>
해당 scope를 주석 처리해서 해결했다.<br><br>

## 오류 해결 체크

![image](https://user-images.githubusercontent.com/73421820/120070834-5ad70d00-c0c7-11eb-8a40-53c8f0692b69.png)


오류가 사라진 것을 확인할 수 있다.<br>

<br><br>
