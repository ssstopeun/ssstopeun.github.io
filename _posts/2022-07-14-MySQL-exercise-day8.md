---
title: Day8.MySQL 실습 (SELECT 편)
date: 2022-07-14 14:54:00 +0900
categories: [Backend, Database]
tags: [Database, Backend, SW, MySQL] 
author: author_id 
---

# [DAY8] MySQL 실습 (1)SELECT
---
**channel class**

|id|channel|
|:---|:----------|
|1|Instagram|
|2|Naver|
|3|Youtube|
|4|Google|
|5|Facebook|
|6|Tiktok|
|7|Unknown|

<br>

**session class**

|id|user_id|created|channel_id|
|:--|:----|:-----------------|:------|
|1|779|2019-05-01 00:36:00|5|
|2|230|2019-05-01 02:53:49|3|
|3|369|2019-05-01 12:18:27|6|
|4|248|2019-05-01 13:41:29|5|
|5|676|2019-05-01 14:17:54|2|
|6|40|2019-05-01 14:42:50|3|
|7|468|2019-05-01 15:08:16|1|
|8|69|2019-05-01 15:20:27|4|

다음의 두 클래스를 예제로 실습을 진행해 보려고 한다.

## SELECT
---
> 테이블에서 레코드들을 읽어오는데 사용한다.  
WHERE을 사용해 조건을 만족하는 레코드이다.

```sql
SELECT 필드이름1, 필드이름2, ...
FROM 테이블이름
WHERE 선택조건
GROUP BY 필드이름1, 필드이름2, ...
ORDER BY 필드이름 [ASC|DESC] -- 필드 이름 대신에 숫자 사용가능
LIMIT N;
```
와 같이 사용한다. 몇가지 예시를 보자.
<br>

"session class 모든 것을 보여달라" 

```sql
SELECT *        -- *는 모든 필드를 지칭하는 표현
FROM prod.session;  -- 앞서 USER prod;를 수행했다면 FROM session도 사용 가능
```

<br>

"session class에서 id, user_id, channel_id만 10개 보여달라"
```sql
SELECT id, user_id, channel_id
FROM prod.session
LIMIT 10;
```

<BR>

"session class에서 channel_id 값들의 요소들을 보여라. (중복 X)"
```sql
SELECT DISTINCT channel_id  --유일한 channel_id를 알고 싶은 경우
FROM prod.session;
```

<BR>

"prod.session 의 레코드 수를 하나씩 count해라"
```sql
SELECT COUNT(1)     -- 테이블의 모든 레코드 수를 카운트, COUNT(*)
FROM prod.session;
```
<bR>

"channel_id가 5인 경우만 count해라"
```sql
SELECT COUNT(1)
FROM prod.session
WHERE channel_id = 5;
```

## CASE WHEN
> 필드 값의 변환을 위해 사용 가능  
- CASE WHEN 조건 THEN 참일때 값 ELSE 거짓일때 값 END 필드이름  
- 여러 조건을 사용하여 변환하는 것도 가능

```sql
CASE
    WHEN 조건1 THEN 값1
    WHEN 조건2 THEN 값2
    ELSE 값3
END 필드이름

EX.
SELECT channel_id, CASE
    WHEN channel_id in (1, 5, 6) THEN 'Social-Media'
    WHEN channel_id in (2, 4) THEN 'Search-Engine'
    ELSE 'Something-Else'
END channel_type
FROM prod.session;
```
예시의 경우 "chaneel_id필드의 1, 5, 6은 'Social-Media'로 2, 4는 'Search-Engine'으로, 나머지는 'Something-Else'로 이름을 바꾸고 필드이름도 channel_type로 변경해라" 는 뜻입니다.  
따라서 제일 처음 나왔던 session 테이블 사진에서 channel_id 필드이름은 channel_type으로 변경되고 값들은 대응되는 type이름으로 변경되게 됩니다.

## NULL
---
> 값이 존재하지 않음을 나타내는 상수  
**0혹은 ""과는 다르다.**

- 필드 지정시 값이 없는 경우 NULL로 지정이 가능하다.
    - 테이블 정의시 디폴트 값으로도 지정 가능
- 어떤 필드의 값이 NULL인지 아닌지 비교는 특수한 문법으로 한다.
    - field1 is NULL
    - field1 is not NULL
- NULL이 사칙연산에 사용된다면
    - SELECT 0 + NULL
    - 0 - NULL
    - 0 * NULL
    - 0/NULL

## COUNT
---
**COUNT()는 괄호안의 NULL이 아니면 하나씩 카운트 하고 NULL이면 카운트를 안한다.** 
COUNT함수는 예시를 통해 알아보자.

| value |
|:-|
|NULL|
|1|
|1|
|0|
|0|
|4|
|3|

다음과 같은 prod.count_test가 있다. 

1. SELECT COUNT(**1**) FROM prod.count_test **-> 7**
2. SELECT COUNT(**0**) FROM prod.count_test **-> 7**
3. SELECT COUNT(**NULL**) FROM prod.count_test **-> 0**
4. SELECT COUNT(**value**) FROM prod.count_test **-> 6**
5. SELECT COUNT(**DISTINCT value**) FROM prod.count_test **-> 4**

**value일 경우는 NULL이 아닌 값만 카운트 한다.**

## WHERE
---
### IN
- WHERE channel_id in (3, 4) : channel_id 의 3,4
    - WHERE channel_id = 3 OR channel_id = 4
- NOT IN : ~ 빼고 다

- EX. "channel_id가 4, 5인 것만 COUNT해라"

```sql
SELECT COUNT(1)
FROM prod.session
WHERE channel_id IN (4, 5);
```


### LIKE
- LIKE : 대소문자 구별없이 문자열 매칭 기능 제공
- WHERE channel LIKE 'G%' -> 'G*'
- WHERE channel LIKE '%o%' -> '* o *'
- NOR LIKE
- EX. "channel의 중간에 G가 들어가는 것을 COUNT해라"
""
```sql
SELECT COUNT(1)
FROM prod.cannel
WHERE channel LIKE '%G%';
```

## STRING Functions
---
- LEFT(str, N) : str을 왼쪽에서 부터 N만큼
- REPLACE(str, exp1, exp2) : str의 exp1을 exp2로 대체
- UPPER(str) : str을 모두 대문자로 바꾸기
- LOWER(str) : str을 모두 소문자로 바꾸기
- LENGTH(str) : str의 길이
- LPAD, RPAD
- SUBSTRING : str 특정부분 추출
- CONCAT 

## ORDER BY
---
- 디폴트 순서는 오름차순 (작은 값부터)
    - ORDER BY 1 ASC
- 내림차순(Descending)을 원하면 "DESC"
    - ORDER BY 1 DESC
- 여러개의 필드를 사용해서 정렬하려면
    - ORDER BY 1 DESC, 2, 3
- NULL 값 순서는?
    - 오름차순 일 경우, 처음
    - 내림차순 일 경우, 마지막

```sql
SELECT value
FROM prod.count_test
ORDER BY value DESC / ASC ;
```

## 타입 변환
---

- DATE Conversion
    - NOW : 현재시각 return
    - 타임존 관련 변환
        - **CONVERT_TZ(now(), 'GMT', 'Asia/Seoul')**
    - DATE, WEEK, MONTH, YEAR, HOUR, MINUTE, SECOND, QUARTER, MONTHNAME
    - DATEDIFF : 날 사이의 차이 (GAP)
    - DATE_ADD : N일뒤의 날짜 return

- STR_TO_DATE
- DATE_FORMAT

예를 몇가지 보자.

|created|
|:-|
|2019-01-01 00:06:48|

이런 class를 다음 형변환들로 필드를 추가할 수 있다.
```sql
SELECT
    created, CONVERT_TZ(created, 'GMT', 'Asia/Seoul') seoul_time,
    YEAR(created) y, QUARTER(created) q, MONTH(crated) m, MONTHNAME(created) mnn,
    DATE(created) d, HOUR(created) h, MINUTE(created) m, SECOND(created) s
    from session
    LIMIT 10;
```
이렇게 되면

|created|seoul_time|y|q|m|mn|d|h|m|s|
|:-|:-|:-|:-|:-|:-|:-|:-|:-|:-|
|2019-01-01 00:06:48|2019-01-01 09:06:48|2019|1|1|January|2019-01-01|0|6|48|

이렇게 형변환이 되어 필드값에 저장되는 것을 볼 수있다.

<br>

다음은 현재시점에서 created와의 gap을 계산해주는 예시이다.
```sql
SELECT created,
    DATEDIFF(now(), created) gap_in_days,
    DATE_ADD(created, INTERVAL 10 DAY) ten_days_after_created
FROM session
LIMIT 10;
```
|created|gap_in_days|ten_days_after_created|
|:-|:-|:-|
|2019-01-01 00:06:48|936|2019-01-11 00:06:48|

gap_in_days는 현재와 created와의 날짜 차이를, ten_days_after_created는 created로 부터 10일 뒤의 날을 필드에 저장해 보여주고 있다.

## Type Casting
---
- 1/2의 결과
    - 0이다. 정수간의 연산은 정수가 되어야 하기 때문이다.
        - 분자나 분모 중의 하나를 float로 캐스팅해야 0.5가 나온다.
        - 이는 프로그래밍 언어에서도 일반적으로 동일하게 동작한다.
- cast 함수를 사용한다.
    - cast (category as float)
    - convert (expression, float)
- 문자열을 float로
    - SELECT cast('100.0' as float)
    - SELECT convert('100.0', float);