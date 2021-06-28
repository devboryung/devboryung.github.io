---
title: "[Spring] 검색 처리"
excerpt: "Spring Framework - MyBatis의 동적 SQL"
categories: 
  - Spring
tags: 
  - Spring
toc: true
---


## MyBatis의 동적 SQL

검색 조건이 변하면 SQL의 내용 역시 변하기 때문에 XML이나 어노테이션과 같이 고정된 문자열을 작성하는 방식으로는 제대로 처리할 수 없다.<br>

다행히 MyBatis는 동적(Dynamic)태그 기능을 통해서 SQL을 파라미터들의 조건에 맞게 조정할 수 있는 기능을 제공한다.<br>
MyBatis의 동적 태그는 약간의 구문을 이용해서 전달되는 파라미터를 가공해서 경우에 따라 다른 SQL을 만들어서 실행할 수 있다.<br>

<br><br>

### MyBatis의 동적 태그들

MyBatis는 기존의 iBatis에서 발전하면서 복잡했던 동적 SQL을 작성하는 태그들이 많이 정리되어서 다음과 같이 몇 가지의 태그들만을 이용한다.<br>

- if
- choose(when, otherwise)
- trim(where, set)
- foreach

<br><br>


**<if\>**

if는 test라는 속성과 함께 특정한 조건이 true가 되었을 때 포함된 SQL을 사용하고자 할 때 작성한다.<br>
예를 들어, 단일 항목으로 제목(title), 내용(content), 작성자(writer)에 대해서 검색해야 하는 상황일 때<br>

- 검색 조건이 'T'면 제목이 키워드인 항목을 검색
- 검색 조건이 'C'면 내용이 키워드인 항목을 검색
- 검색 조건이 'W'면 작성자가 키워드인 항목을 검색

<br>

위와 같은 경우 MyBatis에서는 XML에서 다음과 같이 작성할 수 있다.<br>

```sql
<if test="type == 'T'.tostring()">
    (title like '%'||#{keywork}||'%')
</if>
<if test="type == 'C'.tostring()">
    (content like '%'||#{keywork}||'%')
</if>
<if test="type == 'W'.tostring()">
    (writer like '%'||#{keywork}||'%')
</if>
```

<br>

If안에 들어가는 표현식은 OGNL표현식이라는 것을 이용한다.<br>

<br>

<br><br>

**<choose\>**

if와 달리 choose는 여러 상황들 중 하나의 상황에서만 동작한다.<br>

```sql
<choose>
<when test="type=='T'.tostring()">
    (title like '%'||#{keyword}||'%')
</when>
<when test="type=='C'.tostring()">
    (content like '%'||#{keyword}||'%')
</when>
<when test="type=='W'.tostring()">
    (writer like '%'||#{keyword}||'%')
</when>
<otherwise>
    (title like '%'||#{keywork}||'%' OR content like '%'||#{keyword}||'%')
</otherwise>
</choose>
```

<br>

<otherwise\>는 모든 위의 조건이 충족되지 않을 경우에 사용한다.<br>

<br><br>

**<trim\>****<where\>****<set\>**

trim,where,set은 단독으로 사용되지 않고 <if\>, <choose\>와 같은 태그들을 내포하여 SQL들을 연결해 주고, 앞 뒤에 필요한 구문들(AND,OR,WHERE 등)을 추가하거나 생략하는 역할을 한다.<br>
SQL을 작성하다 보면 상황에 따라서 WHERE나 AND,OR등이 문제가 되는 상황이 발생할 수도 있다.<br>
예를 들어, 'WHERE ROWNUM <=20'은 문제가 없지만 검색 조건이 들어가면 문제가 될 수 있다.<br>
'WHERE title ='Text' **AND** ROWNUM <=20'<br>

만일 검색 조건이 없다면 AND라는 키워드는 들어갈 필요가 없지만, 검색 조건이 추가되면 AND가 필요한 상황이 된다.<br>
where, trim, set은 이러한 상황에서 필요한 키워드를 붙이거나 빼는 상황에서 사용한다.<br>
<br>

<where\>의 경우 태그 안쪽에서 SQL이 생성될 때는 WHERE구문이 붙고, 그렇지 않는 경우에는 생성되지 않는다.<br>

```sql
select * from tbl_board
    <where>
        <if test="bno != null">
            bno = #{bno}
        </if>
    </where>
```
<br>

위와 같은 경우 bno값이 null인 경우에는 where 구문이 없어지고, bno 값이 존재하는 경우에만 'where bno = xx' 와 같이 생성된다.<br>

<br><br>

<trim\>은 하위에서 만들어지는 SQL문을 조사하여 앞 쪽에 추가적인 SQL을 넣을 수 있다.<br>

```sql
select * from tbl_board
    <where>
        <if test="bno != null">
            bno = ${bno}
        </if>
        <trim prifix = "and">
        rownum =1
        </trim>
    </where>   
```
<br>

trim은 prifix, suffix, prefixOverrides, suffixOverrides 속성을 지정할 수 있다.<br>

- bno 값이 존재하는 경우

```sql
select * from tbl_board where bno =33 and rownum =1
```

- bno가 null인 경우

```sql
select * from tbl_board WHERE rownum =1
```

<br>


<br><br>


**foreach**

foreach는 List, 배열, 맵 등을 이용해서 루프를 처리할 수 있다.<br>
주로 IN조건에서 많이 사용하지만, 경우에 따라서는 복잡한 WHERE 조건을 만들때에도 사용할 수 있다.<br>

예를 들어, 제목('T')은 'TTTT'로 내용('C')은 'CCCC'라는 값을 이용한다면 Map의 형태로 작성이 가능하다.<br>

```java
Map<String,String> map = new HashMap<>();
map.put("T", "TTTT");
map.put("C", "CCCC");
```
<br>

작성된 Map을 파라미터로 전달하고, foreach를 이용하면 다음과 같은 형식이 가능하다.<br>

```sql
select * from tbl_board 
    <trim prefix="where ("suffix=")" prefixOverrides="OR">
        <foreach item="val" index="key" collection ="map">
            <trim prefix ="OR">
                <if test="key == 'C'.tostring()">
                    content = #{val}
                </if>
                <if test="key == 'T'.tostring()">
                    title = #{val}
                </if>
                <if test="key == 'W'.tostring()">
                    writer = #{val}
                </if>
            </trim>
        </foreach>
    </trim>
```

<br>

foreach를 배열이나 List를 이용하는 경우에는 item 속성만을 이용하면 되고, Map의 형태로 key와 value를 이용해야 할 때는 index와 item 속성을 둘 다 이용한다.<br>

전달 된 값에 따라서 다음과 같이 처리된다.<br>

```sql
select * from tbl_board
    where (content = ?
     OR title =?)
```
<br>

<br><br>

관련 서적 : [코드로 배우는 스프링 웹 프로젝트](https://cafe.naver.com/gugucoding)
<br><br>
