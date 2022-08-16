---
title: Tibero Lecture1. Tibero Architecture
date: 2022-08-16 15:00:00 +0900
categories: [Tibero DBMS, Education]
tags: [Tibero, Backend, SW, DBMS] 
author: author_id 
---

# Tibero Architecture

## Tibero Instance
---
> Tibero Database 서버는 Tibero Instance와 Database로 구성된다. Database는 데이터 파일들의 집합이고 Instance는 이 데이터베이스를 관리하는 것이다.

## Tibero Process
> 대규모 사용자 접속을 수용하는 다중 프로세스 및 다중 스레드 기반의 아키텍쳐 구조이다.  
> LSNR (Listener), WPROC (Worker Process), Background process 로 구성된다.

### Tibero process 동작 과정

1. Client가 접속을 요청한다.
2. Listener가 현재 빈 WTHR이 있는 프로세스를 찾아 접속요청을 CTHR에게 넘겨준다.
3. 요청받은 CTHR은 자기 자신의 WTHR 상태를 체크해 일하지 않는 WTHR에게 할당한다.
4. WTHR은 Client와 인증 절차를 걸쳐 세션을 시작한다.
<br>


- 연결된 후의 Client의 동작여부는 중요하지 않다.
- 인증절차에는 IP포트번호, 데이터베이스이름, user명, password등의 정보가 필요하다.


### Listener
> Client의 접속요청을 가장 처음에 받아 유효한 Worker Process에 할당하는 중계역할을 하고 이는 별도의 실행 파일인 tblistener를 사용한다. 

### Worker Process ( Foreground Process )
> Client와 실제 통신을 하여 Client의 요구사항을 처리하는 프로세스이다. 

- CTHR (Control Thread)  
    - 각 Working Process마다 하나씩 생성되고 서버가 시작될때 지정된 개수의 Worker Thread를 생성한다.
    - 시그널처리를 담당한다.
    - I/O Multiplexing을 지원하고 Worker Thread대신 메시지 송/수신 역할을 수행하기도 한다.

- WTHR (Worker Thread)
    - 각 Worker Process마다 여러개가 생성된다.
    - Client가 보내는 메시지를 받아 처리하고 그 결과를 return한다.
    - SQL Parsin, 최적화, 수행 등의 DBMS가 해야 하는 대부분의 일을 직접적으로 처리하는 곳이다.

### Background Process
> 사용자의 요청을 직접 받아들이는 것이 아니라 Worker Thread나 다른 배경 프로세스가 요청할 때, 혹은 정해진 주기에 따라 동작하며 주로 시간이 오래걸리는 디스크의 작업을 담당한다.


