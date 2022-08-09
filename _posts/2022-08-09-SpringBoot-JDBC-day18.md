---
title: Day18.JDBC & Spring
date: 2022-08-09 12:00:00 +0900
categories: [Backend, SpringBoot]
tags: [SpringBoot, Backend, SW, JDBC] 
author: author_id 
---

# [Day18] JDBC & Spring


## Data Connection Pool (DBCP)
---
> Data와 관련된 connection을 pool에 미리 만들어 두고 필요할 때마다 가져와 사용하는 방식이다.

1. pool에서 connection을 가져온다.
2. connection을 사용한다.
3. connection을 pool에 반환한다.

## HikariCP
---
> HikariCP는 2012년도경에 Brett Wooldridge가 개발한 매우 가볍고 빠른 JDBC Connection Pool이다.

