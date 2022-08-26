---
title: Tibero Lecture5. Backup
date: 2022-08-25 11:31:24 +0900
categories: [Tibero DBMS, Education]
tags: [Tibero, Backend, SW, DBMS] 
author: author_id 
---

# Backup
> 프로그램을 사용하다 보면 다양한 원인에 의해 작업이 비정상으로 종료되는 경우가 있는데 이를 위해서는 백업으로 데이터베이스를 보호하는 것이 중요하다.

- 여러가지 유형의 장애
    - 명령문의 실패
    - 사용자 프로세스의 실패
    - 사용자로 인한 장애
    - Instance fail
    - Media fail
<br>

- 백업 (하루에 한번 Export 백업 권장)
![Desktop View](/assets/img/2022.08/26-1.PNG)
<br>

- 백업의 종류
    - 논리적인 백업 : Export 툴로 하는 백업으로 데이터베이스의 논리적인 단위를 백업한다.
    - 물리적인 백업 : COPY명령어를 통해 데이터베이스가 저장되어 있는 datafile, controlfile, archive logfile 등을 백업하는 것이다.

- 백업의 대상 1. Controlfile
    - 데이터베이스를 mount할때 반드시 필요한 파일로 두개이상의 controlfile을 구성하고 서로 다른 디스크에 위치시키는 것을 권장한다.
    - **V$CONTROLFILE**
    - 다중화방법 : 데이터베이스를 mount할때 필요한 것이므로 우선 tbdown을 시킨후 controlfile을 copy 시킨 후 $TB_SIT.tip 파일에 CONTOL_FILES 파라미터를 추가한 후 tbboot로 재기동한다.
    - OffLine Backup : O/S의 copy명령을 통해 별도의 위치에 COPY한다.
    - OnLine Backup : 다음의 구문으로 copy한다.
        ```sql
        SQL> ALTER DATABASE BACKUP CONTROLFILE TO TRACE AS [백업할 파일 경로 및 이름] REUSE NORESETLOGS;
        ```
<br>

- 백업의 대상 2. Redo logfile
    - 데이터베이스의 모든 변경사항이 저장되는 파일로 최소한 2개 이상의 redo log group으로 구성되어 각 log group은 하나 이상의 redo logfile로 구성된다.
    - **V$LOG** / **V$LOGFILE**
    



오늘은 다양한 방법으로 백업하고 고의로 데이터를 손상시켜 다시 복구하는 작업을 해보고자 한다. 이걸 배우면서 우영우드라마에 서버해킹당해서 고객들 개인정보 빠져나간 편이 생각났다...ㅋ :)

## 0. 아카이브 로그 모드 변경
> 우선 실습을 위해