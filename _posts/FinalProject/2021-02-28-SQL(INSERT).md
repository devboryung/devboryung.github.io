---
title: "2020년 02월 28일"
excerpt: "파이널프로젝트 (SQL-반복 데이터 추가)"
search: true
categories: 
  - Academy
tags: 
  - SQL
  - sqldeveloper
toc: true
---

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


