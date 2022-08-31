---
title: REST API Project1. Project Introduce
date: 2022-08-25 11:31:24 +0900
categories: [Backend, REST API Project]
tags: [react, Backend, SW, DBMS] 
author: author_id 
---

# REST API Project Chapter1. Introduce

## Background
---
> 고객들이 Coffe Bean package를 온라인 웹 사이트로 주문할 수 있도록 하는 API 개발

- 매일 전날 ㅇ후 2시부터 오늘 오후 2시까지의 주문을 모아서 처리한다.
- 커피의 종류
    - Columbia
    - Brazil Serra
    - Columbia Quindio
    - Ethiopia Sidamo
- 회원을 별도로 관리하지는 않는다.
- email로 고객을 구분하고 주문을 받을 때 email을 같이 받아 주문을 받는다.
- 하나의 eamil로 하루에 여러번 주문을 해도 하나로 합쳐서 다음날 배송을 보낸다.
- 고객에게 "당일 오후 2시 이후의 주문은 다음날 배송을 시작합니다." 라고 알려준다.

## 시스템 구성도
---
![Desktop View](/assets/img/2022.08/31-1.PNG){: width="70%" }

Server는 데이터베이스와 연결되게 된다.

## 프로젝트 생성
![Desktop View](/assets/img/2022.08/31-2.PNG){: width="70%" }
- Dependencies
    - Web - Spring Web
    - SQL - JDBC API
    - SQL - MySQL Driver