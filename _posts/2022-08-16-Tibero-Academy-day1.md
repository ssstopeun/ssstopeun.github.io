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

- 독립된 프로세스로서 사용자의 요청과 비동기적으로 동작한다.
- Backgrount Process의 종류
  - 감시 프로세스 (MONP : monitor process)
    : Tibero 가동 시 제일 먼저 생성되고 제일 마지막에 종료되어 프로세스들을 생성하고 주기적으로 그 프로세스들의 상태를 점검해주는 프로세스이다. **교착상태** 도 검사해주는 특징이 있다.
  - 매니저 프로세스 (MGWP : manager worker process)
    : 시스템을 관리해주는 역할을 하고 접속요청을 받았을때 예약된 워크 스레드에 접속을 할당해준다. 하지만 이는 sys계정만 접속을 허용하여 리스너를 거치지 않고 스페셜 포트를 통해 직접 접속을 도와준다.
  - 에이전트 프로세스 (AGNT : agent process)
    : 시스템 유지를 위해 주기적으로 처리해야하는 Tibero 내부의 작업을 담당한다. 내부작업의 판단을 맡고 있고 판단 후 워커프로세스에게 의뢰하는 구조이다. 다중 스레드 기반으로 동작하며 서로다른 용도의 업무를 스레드별로 나누어 수행한다.
  - 데이터베이스 쓰기 프로세스 (DBWR : database writer)
   : 데이터베이스에서 변경된 내용을 디스크에 기록하는 일과 연관된 스레드들이 모여있다. 주기적으로 사용자가 변경한 블록을 디스크에 기록한다. 리두 로그를 디스크에 기록하고 두 스레드를 통해 데이터베이스의 체크포인트과정을 관할하는 체크포인트 스레드이다.
  - 복구 프로세스 (RCWP : recover worker process)
    : 복구 전용 프로세스로 Crash / Instance Recovery 를 수행한다.
    
### Tibero Shared Memory (TSM)
- 인스턴스에 대한 데이터와 제어정보를 가지는 공유메모리 영역이다.
- 사용자와 동시에 데이터를 공유한다.
- Database buffer, Redo Log Buffer, SQL Cache, Data Dictionary Cache로 구성된다.
- Background Process는 인스턴스가 시작될 때 TSM영역을 할당하고 인스턴스가 종료되면 할당이 해제된다.
- TSM의 전체 크기는 인스턴스가 시작될 때 생성되어 고정된다.






