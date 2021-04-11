---
title: "SQL-반복 데이터 추가"
excerpt: "파이널프로젝트 - 싱글벙글"
search: true
categories: 
  - FinalProject
tags: 
  - SQL
  - sqldeveloper
toc: true
---

<br>

등록버튼을 통해서 데이터를 추가하기 전에 페이징 처리나 목록 조회 테스트를 위해 임시 데이터를 생성했다.<br>  



## sql문

```sql
-- 반복 샘플 데이터 넣기
BEGIN
    FOR N IN 1..10 LOOP
        INSERT INTO BOARD
        VALUES(SEQ_BNO.NEXTVAL,
                    N || '번째 게시글',
                    N || '번째 게시글 입니다.',
                    DEFAULT, DEFAULT, DEFAULT, 2,
                    FLOOR(DBMS_RANDOM.VALUE(1,4)) + 20, 1);
    END LOOP;
    COMMIT;
END;
```

<br>

1. FOR N IN `1..10` LOOP<br>
1에서 10까지 반복<br><br>
2. `N` ||  번째 게시글<br>
위에 반복되는 숫자와 동일한 숫자가 출력 된다.<br><br>
3. FLOOR(DBMS_RANDOM.VALUE(1,4)) + 20<br>
1부터 4까지 정수 중 랜덤값을 추출해 10을 더한다.<br><br><br>



- 등록된 데이터 <br>
![image](https://user-images.githubusercontent.com/73421820/109422743-4039bd00-7a20-11eb-8210-27eb428b5594.png)

<br><br>


