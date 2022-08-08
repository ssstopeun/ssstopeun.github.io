---
title: Day17.JDBC
date: 2022-08-08 13:00:00 +0900
categories: [Backend, SpringBoot]
tags: [SpringBoot, Backend, SW, JDBC] 
author: author_id 
---
# [Day17] JDBC
> **JDBC는 Java Data Base Connecter의 약자**로 Java Application과 DataBase를 연결할때의 표준을 알려준다.

- 백엔드 개발자는 JDBC API를 이용하여 Query에 요청을 하는 등의 작업을 수행한다.
- JDBC Type 4가지 중 MySQL에서 제공하는 JDBC Type 4를 공부할 예정이다.

- JDBC Operation FLOW
    1. 우선 DB로 부터 Connection을 받아온다.
    2. Connection으로 부터 Statement를 만든다.
    3. Statement를 이용해 실행시킨다.
    4. 문제가 있다면 문제를 해결하고 Statement를 통해 쿼리를 실행하여 ResultSet을 받아오거나 update를 실행한다.
    5. Statement, Connection을 순서대로 종료해준다.

## JDBC 준비하기
---
