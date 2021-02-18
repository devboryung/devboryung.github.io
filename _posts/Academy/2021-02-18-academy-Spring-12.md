---
title: "2020년 02월 18일"
excerpt: "Spring 9 (스프링 스케쥴링)"
search: true
categories: 
  - Academy
tags: 
  - FrameWork
  - Java
  - Spring
  - SummerNote
toc: true
---


## Spring Scheduling
스프링에서 제공하는 스케줄러로써 
지정된 시간 또는 특정 간격 마다 작업(job)을 진행하도록 하는 기능. <br><br>

게시글 등록 시
주기적으로  DB와 서버의 파일을 비교해 데이터 불일치 현상을 없앤다.<br><br>

<br>

- 사용조건
1. dispatcher servlet (servlet-context.xml)파일에 task namespace를 추가 + `<task:annotation-driven/>` 추가<br>
2. 작업이 작성된 클래스를 Bean으로 등록<br>
(Bean 등록 == 스프링 컨테이너가 제어 가능(IOC)<br>
bean 등록 어노테이션 : @Controller, @Service , @Repository, @Component<br>
3. 지정된 작업이 기록된 메소드를 작성 + 지정된 시간마다 동작하기 위해 @Scheduled 어노테이션 작성<br>  
> @Scheduled 어노테이션 속성<br> 
>- fixedDelay : 이전 작업이 끝난 시점으로 부터 고정된 시간(ms)만큼 지난 후 수행<br> 
>- fixedRate : 이전 작업이 시작된 시점으로 부터 고정된 시간(ms)만큼 지난 후 수행<br> 
일정 간격마다 하려면 fixedRate가 더 정확하다 , fixedDelay는 뒤로갈수록 밀릴 수 있음.<br> 
> - cron : UNIX 계열 잡 스케쥴러 표현식<br> 
>>cron="초 분 시 일 월 요일 [년도]"<br> 
>>(요일: 일요일==1, 토요일==7)<br> 
>>특수문자 <br> 
>>`*` : 모든 수<br> 
>>`-` : 두 수 사이의 값, ex) 10-15 : 10이상 15이하<br> 
>>, : 특정 값 지정. ex) 3,4,7 : 3,4,7 지정<br> 
>>/ : 값의 증가.  ex) 0/5 : 0부터 시작해서 5씩 증가할 때 마다.<br> 
>>? : 특별한 값이 없음 (월, 요일만 해당)<br> 
>>EX)<br> 
>>2021년 2월 18일 목요일 10시 20분 30초 -> cron="30 20 10 18 2 5 2021"<br> 
>>매년 2월 18일 10시 20분 30초 -> cron="30 20 10 18 2 * [*]"<br> 
>>매일 10시 20분 30초 -> cron="30 20 10 * * *"<br> 
>>매일 자정(0)시 -> cron="0 0 0 * * *"<br> 

<br> <br> 


### 서버와 DB 파일 검사
최근 3일이내 저장된 파일을 제외한 나머지 파일 중 서버에는 있지만 DB에는 존재하지 않는 이미지 파일을 매 시간마다 검사하여 삭제

<br>

```xml
<!-- servlet-context.xml -->

<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:beans="http://www.springframework.org/schema/beans"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:task="http://www.springframework.org/schema/task"
xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
	http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task-4.3.xsd
	">

<!-- 스프링 스케쥴러 : 스케쥴러 관련 어노테이션 활성화  -->
<task:annotation-driven/>

```
<br>

```java
//ImageDeleteScheduling.java

package com.kh.spring.common.scheduling;

import java.io.File;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import javax.servlet.ServletContext;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

import com.kh.spring.board.model.service.BoardService;

@Component
public class ImageDeleteScheduling {
	
	@Autowired
	private ServletContext servletContext;
	
	@Autowired
	private BoardService boardService;
	
	//@Scheduled(cron="0 0 * * * *")
	@Scheduled(cron="0 * * * * *") // 매 분마다 진행하는 걸로 테스트
	public void deleteimage() {
		// 최근 3일이내 저장된 파일을 제외한 나머지 파일 중
		// 서버에는 있지만 DB에는 존재하지 않는 이미지 파일을 매 시간마다 검사하여 삭제
		
		// 1. 서버와  DB에 있는 파일 리스트를 조회 (최근 3일 이내 데이터만)
		// 1) 서버 이미지 파일 리스트
		
		// 이미지 파일이 저장된 실제 경로를 얻어옴
		String savePath = servletContext.getRealPath("/resources/infoImages");
		
		// 해당 경로에 있는 모든 파일을 읽어옴
		File folder = new File(savePath);
		
		File[] fileList = folder.listFiles(); // savePath 폴더에 있는 모든 이미지 파일이 배열로 반환됨
		
		// fileList에서 최근 3일이내를 제외한 나머지 파일만 새로운 List에 저장
		List<File> serverFileList = new ArrayList<File>();
		
		// 3일 전 날짜
		Date threeDaysAgo = new Date(System.currentTimeMillis()-(3*24*60*60*1000));
		SimpleDateFormat sdf = new SimpleDateFormat("yyMMddHH");

		String standard = sdf.format(threeDaysAgo);
		// 3일 전 날짜가 진정된 패턴 모양의 문자열 반환
		
		// fileList가 존재하고  파일이 1개 이상 있을 경우
		for(File file : fileList) {
			// fileList 배열에 반복 접근하여 파일명만 얻어옴
			String fileName = file.toString().substring(file.toString().lastIndexOf("\\")+1);
			// 가장 마지막 \의 다음칸 부터 자름.
			//System.out.println(file.toString());
			//System.out.println(fileName);
			
			// 기준(3일 전 시간)보다 더 이전에 만들어진 파일일 경우
			// a.compareTo(B) : A가 B보다 크면 양수, A보다 B보다 작으면 음수, A와  B가 같다면 0
			if(standard.compareTo(fileName.substring(0,8))>=0){
				// 서버 파일 리스트에 추가
				serverFileList.add(file);
			}
		}
		
		// 2) DB 이미지 정보 파일 리스트 조회
		List<String> dbFileList = boardService.selectDBFileList();
				
		
		// 2. 두 리스트를 비교하여 불일치 되는 서버 이미지 파일을 삭제
		if(dbFileList!=null && !serverFileList.isEmpty() ) {
			
			// serverfileList 반복 접근
			for(File serverFile : serverFileList) {
				String fileName = serverFile.toString().substring(serverFile.toString().lastIndexOf("\\")+1);
				
				// indexOf(비교값) : 비교값이 List에 존재하면 해당 index 반환, 없으면 -1 반환
				if(dbFileList.indexOf(fileName)<0) {
					serverFile.delete();
					System.out.println(fileName+"삭제");
				}
			}
		}
	}
}
```

<br>

```java
// BoardService.java

/** DB에 저장된 최근 3일이내를 제외한 파일 정보를 조회하는 Service
	* @return dbFileList
	*/
public abstract List<String> selectDBFileList();
```

<br>

```java
//BoardServiceImpl.java

// DB에 저장된 최근 3일이내를 제외한 파일 정보를 조회하는 Service 구현
@Override
public List<String> selectDBFileList() {
	return dao.selectDBFileList();
}
```

<br>

```java
//BoardDAO.java

/** DB에 저장된 최근 3일이내를 제외한 파일 정보를 조회하는 DAO
	* @return dbFileList
	*/
public List<String> selectDBFileList() {
	return sqlSession.selectList("boardMapper.selectDBFileList");
}
```

<br>

```sql
/* board-mapper.xml */

<!-- DB에 저장된 최근 3일이내를 제외한 파일 정보를 조회 -->
<!-- 
<![CDATA[ 부등호 포함 SQL 구문]]>
CDATA : markUp 언어에서  <, > 태그 기호를 태그가 아닌 글자로 인식하게 하는 구문
-->
<select id="selectDBFileList" resultType="string">
SELECT FILE_NAME
FROM ATTACHMENT

<![CDATA[
WHERE TO_DATE(SUBSTR(FILE_NAME,0,8), 'YYMMDDHH24') < SYSDATE -3
]]>
</select>
```
<br><br><br>