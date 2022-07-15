---
title: Day8-2.MySQL 실습 (GROUP BY 편)
date: 2022-07-15 14:24:00 +0900
categories: [Backend, Database]
tags: [Database, Backend, SW, MySQL] 
author: author_id 
---

# [DAY8] MySQL 실습 (2)GROUP BY
> SELECT 실행시 결과를 특정 그룹별로 묶는 방법

## GROUP BY & Aggregate 함수
---
> 테이블의 레코드를 그룹핑하여 그룹별로 다양한 정보를 계산하는 것이다.

- 두 단계로 이루어진다.  
1. 그룹핑 할 필드를 결정한다. (하나 이상)
    - 필드이름이나 필드 일련번호를 사용해 GROUP BY로 지정한다.
2. 다음 그룹별로 계산할 내용을 결정한다.
    - 여기서 **Aggregate**함수를 사용한다.
    - COUNT, SUM, AVG, MIN, MAX, GROUP_CONCAT ...
        - 보통 필드 이름을 지정하는 것이 일반적이다. (alias)
<br>

GROUP BY부터는 MySQL Workbench를 통해 문제를 풀어보며 공부하고자 한다.

![Desktop View](/assets/img/2022.07/15-1.PNG){: width="70%" }
---

### 1. 월별 세션수를 계산하라
- prod.session을 사용한다. (id, created 필드)
```sql
SELECT
    LEFT(created, 7) AS mon,    -- created의 왼쪽부터 7글자를 이제 mon이라 부르겠다.
    COUNT(1) AS session_count
FROM prod.session
GROUP BY 1  -- = GROUP BY mon, GROUP BY LEFT(created, 7)
ORDER BY 1;
```
![Desktop View](/assets/img/2022.07/15-2.PNG)
GROUP BY 1 이라는 것은 SELECT문의 첫번째 줄을 그룹화 하겠다는 뜻이다. 따라서 월별로 session_count가 되어 필드값이 나오는 것을 확인할 수있다.


### 2. 가장 많이 사용된 채널은 무엇인가?
```sql
SELECT
	channel_id,
    COUNT(1) AS session_count,
    COUNT(DISTINCT user_id) AS user_count
FROM session
GROUP BY 1
ORDER BY 2 DESC;
```
![Desktop View](/assets/img/2022.07/15-3.PNG)
channel_id를 그룹화하여 count를 하게 되는데 두가지 경우가 있다. 첫번째는 그저 채널의 사용수를 구하는 것이고 두번째는 한사람이 채널을 두번 사용하는 중복을 없애고 채널을 사용한 사용자 수를 구하는 것이다.  
ORDER BY 2 를 하느냐, 3을 하느냐로 어떤요소의 내림차순으로 필드를 보여줄지 결정할 수 있다. 
사진은 ORDER BY 2 DESC 를 한 결과이기에 session_count가 내림차순으로 보여진다.

### 3. 가장 많은 세션을 만들어낸 사용자 ID는 무엇인가?
```sql
SELECT
	user_id,
    COUNT(1) user_id
FROM session
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1;
```
![Desktop View](/assets/img/2022.07/15-4.PNG)

이 문제를 해결하기 위해서는 우선 사용자 별로 얼마나 session을 이용했는지 count하고 이를 내림차순으로 정렬한다. 그 후 가장 많은 ~ 이므로 LIMIT 1 을 하여 결과를 도출한다.

### 4. 월별 채널별 유니크한 사용자 수 
> channel_id를 channel로 바꾸어 표현 **-> JOIN**
이 문제는 우선 둘의 공통필드은 channel_id를 이용해 JOIN을 해야한다.   JOIN하는 법은 다음과 같다.
```sql
SELECT s.id, s.user_id, s.created, s.channel_id, c.channel 
FROM session s	-- session을 s로 표현하겠다.
JOIN channel c ON c.id = s.channel_id;
```
![Desktop View](/assets/img/2022.07/15-5.PNG)

<br>

이러한 JOIN을 이용해 문제를 풀어보자.
```sql
SELECT 
	LEFT(created,7) AS mon,
    c.channel,
    COUNT(distinct user_id) AS user_count
FROM session s
JOIN channel c ON c.id = s.channel_id
GROUP BY 1,2
ORDER BY 1 DESC, 2;	
-- 1을 내림차순으로 하되 같은 1안에서는 2를 오름차순으로
```
![Desktop View](/assets/img/2022.07/15-6.PNG)
