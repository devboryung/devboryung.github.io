---
title: "[Spring] 검색 처리"
excerpt: "Spring Framework - 검색 기능과 SQL"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## 검색 처리

검색 기능은 검색 조건과 키워드로 나눠 생각할 수 있다.<br>

검색 조건은 일반적으로 <select\> 태그를 이용해서 작성하거나 <checkbox\>를 이용하는 경우가 많다.<br>
과거에는 <checkbox\>를 이용하는 경우가 더 많았지만, 최근에는 일반 웹사이트에서 일반 사용자들의 경우에는 <select\>를 관리자용이나 검색 기능이 강한 경우 <checkbox\>를 이용하는 형태가 대부분이다.<br>

<br><br>

### 검색 기능과 SQL

게시물의 검색 기능은 다음과 같이 분류가 가능하다.<br>

- 제목/내용/작성자와 같이 단일 항목 검색
- 제목 or 내용, 제목 or 작성자, 내용 or 작성자, 제목 or 내용 or 작성자와 같은 다중 항목 검색

<br><br>

검색 항목은 제목/내용/작성자와 같은 단일 항목 검색과 제목 or 내용과 같이 복합적인 항목으로 검색하는 방식이 존재한다.<br>

게시물의 검색이 붙으면 가장 신경 쓰이는 부분은 역시 SQL쪽이다.<br>

오라클은 페이징 처리에 인라인뷰를 이용하기 때문에 실제로 검색 조건에 대한 처리는 인라인뷰의 내부에서 이루어져야 한다.<br>
단일 항목의 검색은 검색 조건에 따라서 칼럼이 달라지고, LIKE 처리를 통해서 키워드를 사용하게 된다.<br>
<br>

단일 항목은 인라인뷰 안쪽에서 필요한 데이터를 가져올 때 검색 조건이 적용되어야 하기 때문에 WHERE문 뒤에 검색 조건이 추가되고, ROWNUM 조건이 뒤따르게 하면 문제 없다.<br>

<br><br>

**다중 항목 검색**

문제는 2개 이상의 조건이 붙는 다중 항목의 검색이다.<br>

제목이나 내용중에 'Test'라는 문자열이 있는 게시물들을 검색하고 싶다면 다음과 같이 작성될 것이라고 예상한다.<br>

```sql
select 
*
from 
 ( select /*+INDEX_DESC(tbl_board pk_board)*/ 
    rownum rn, bno, title, content, writer, regdate, updatedate
    from
    tbl_board
    where
    title like '%Test%' or content like '%Test%'
    and rownum <=20>)
where rn > 10;
```

<br>



'title like '%Test%' or content like '%Test' 구문 자체는 이상이 없지만, 실제로 동작보면 10개의 데이터가 아니라 많은 양의 데이터가 나온다.<br>

그 이유는 위 SQL문에서 AND 연산자가 OR 연산자 보다 우선 순위가 높기 때문에 'ROWNUM이 20보다 작거나 같으면서(AND) 내용에 'Test'라는 문자열이 있거나(OR) 제목에 'Test'라는 문자열이 있는' 게시물들을 검색하게 된다.<br>

제목에 'TEST'라는 문자열이 있는 경우는 많기 때문에 많은 양의 데이터를 가져오게 된다.<br>

AND와 OR가 섞여있는 SQL을 작성할 때에는 우선 순위 연산자인 '()'를 이용해서 OR 조건들을 처리해야 한다.<br>

```sql
select
*
from
  (select /*+INDEX_DESC(tbl_board pk_board)*/
   rownum rn, bno, title, content, writer, regdate, updatedate
   from tbl_board
   where (title like '%Test%' or content like '%Test%') and rownum <=20)
where rn >10;
```
<br>

위의 SQL을 실행하면 10개의 데이터가 출력된다.<br>


<br><br>


<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
