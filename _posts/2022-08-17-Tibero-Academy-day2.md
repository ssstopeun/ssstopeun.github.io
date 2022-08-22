---
title: Tibero Lecture2. Tibero Install & Setting
date: 2022-08-17 15:00:00 +0900
categories: [Tibero DBMS, Education]
tags: [Tibero, Backend, SW, DBMS] 
author: author_id 
---

# Tibero Install & Setting
> Tibero 설치 과정을 순서대로 따라하며 실습을 진행해보았다.

- Tibero 설치과정은 다음과 같다.
  1. 설치 파일 준비 (Tibero 바이너리, 라이센스 파일 등)
  2. 환경설정 파일에 환경변수 설정
  3. Tibero 바이너리 압축해제, 라이센스 파일 복사
  4. 파라미터 파일 생성용 shell 실행
  5. Tibero 인스턴스 기동 (nomount 모드)
  6. Database 생성
  7. Tibero 인스턴스 기동 (normal 모드)
  8. System object 생성용 shell 실행

이 순서대로 실습한 과정을 살펴보자. 실습 과정은 실행해야하는 T1 Virtual Machine과 putty가 설치되어 있는 환경에서 진행되었다.

## 0. 환경실행 & linux 명령어
---
> T1 Virtual Machine을 실행시킨 후 putty에서 작업을 진행한다.

- 해당 실습은 linux를 사용하기에 linux명령어를 알아야하는데 오늘 실습때 사용하는 명령어를 몇개 소개하고 시작하겠다. 또한 linux에서 파일을 참조할때 그 파일이 위치한 폴더에서 진행하게 되면 파일 이름만으로 참조가 가능하고 그렇지 않는 경우에는 [파일위치/파일이름] 으로 참조해야 한다.

1. 폴더 이동하기
```bash
$ cd [이동할 폴더]
```

2. 파일 위치 변경하기
```bash
$ cp [옮길 파일이름] [옮길 폴더위치]
$ cp [옮길 파일위치/파일이름] [옮길 폴더위치]
```

3. 파일 압축풀기
```bash
$ tar -xvzf [압축풀 파일이름]
$ tar -xvzf [압축풀 파일위치/파일이름]
```

4. 폴더 속 파일 확인하기
```bash
$ ls -l
```

## 1. 설치 파일 준비
---
- 티베로 테크넷 <https://technet.tmaxsoft.com/ko/front/main/main.do> 홈페이지에 접속해 데모라이선스 신청을 클릭한다.
- 회원가입과 로그인 후 신청을 하면 본인 메일로 라이선스가 도착한다.
- 이 라이선스와 tibero7 바이너리 압축파일을 준비해 linux 환경에 저장해 놓으면 된다.

## 2. 환경설정 파일에 환경변수 설정
---

1. vi편집기를 이용하여 아래의 text를 추가해 환경변수를 설정해 준다.
```bash
$ vi ~/.bash_profile
```

2. 아래 코드를 입력해 바뀐 환경변수를 적용해준다.
```bash
$ source ~/.bash_profile**
```

3. 환경변수가 잘 적용되었는지 확인한다. 아래의 코드에서 확인할 사항은 $TB_HOME 과 $TB_SID이고 나머지는 이 두 변수의 값에 따라 결정되는 고정값들이다.
```bash
$ echo $TB_SID
tibero
$ echo $TB_HOME
/tibero/tibero7
```

```
######## TIBERO ENV ########
export TB_HOME=/tibero/tibero7
export TB_SID=tibero
export TB_PROF_DIR=$TB_HOME/bin/prof
export PATH=.:$TB_HOME/bin:$TB_HOME/client/bin:~/tbinary/monitor:$PATH
export LD_LIBRARY_PATH=$TB_HOME/lib:$TB_HOME/client/lib:$LD_LIBRARY_PATH
export SHLIB_PATH=$LD_LIBRARY_PATH:$SHLIB_PATH
export LIBPATH=$LD_LIBRARY_PATH:$LIBPATH

######## TIBERO alias ########
alias tbhome='cd $TB_HOME'
alias tbbin='cd $TB_HOME/bin'
alias tblog='cd $TB_HOME/instance/$TB_SID/log'
alias tbcfg='cd $TB_HOME/config'
alias tbcfgv='vi $TB_HOME/config/$TB_SID.tip'
alias tbcli='cd ${TB_HOME}/client/config'
alias tbcliv='vi ${TB_HOME}/client/config/tbdsn.tbr'
alias tbcliv='vi ${TB_HOME}/client/config/tbnet_alias.tbr' 
alias tbdata='cd $TB_HOME/tbdata'
alias tbi='cd ~/tbinary'
alias clean='tbdown clean'
alias dba='tbsql sys/tibero'
alias tm='cd ~/tbinary/monitor;monitor;cd -'
```
<br>

위의 코드에서 변동되는 요소는 두가지이다.
- TB_SID : Tibero System Identify 서비스 이름
- TB_HOME : Tibero 소프트웨어가 설치될 디렉토리
이 두 요소를 필요에 맞게 변경해 환경을 설정하면 된다.

## 3. Tibero 바이너리 압축해제, 라이센스 파일 복사

처음에 준비해뒀던 설치파일들이다. 이 파일들이 저장되있는 폴더로 간 후 이를 알맞은 폴더로 이동시켜 줄 것이다.

1. 우선 티베로바이너리파일을 /tibero 로 옮겨준다.
```bash
$ cp [티베로 바이너리파일 이름] /tibero
$ cd /tibero
```

2. 이 파일의 압축을 풀어준다. 그럼 Tibero가 설치되는 것이다. 압축을 해제하고 나면 /tibero 폴더에 Tibero7 폴더가 만들어지는데 이것이 바로 처음 환경설정때 $TB_HOME을 /tibero/tibero7이 되는 것이다. $TB_HOME과 Tibero가 설치된 위치를 맞춰주어야 한다.
```bash
$ tar -xvzf [티베로 바이너리파일 이름]
```

3. 압축을 풀고 나면 $TB_HOME에 license라는 폴더가 있을 것이다. 이 폴더에 처음 티맥스 홈페이지에서 전송받은 라이선스를 위치시킨다.
```bash
$ cp [라이선스이름] $TB_HOME/license
```

## 4. 파라미터 파일 생성용 shell 실행

```shell
$ cd $TB_HOME/config
$ ./gen_tip.sh
```

그 후 초기 환경파일인 파라미터 파일 생성을 해주는 shell을 실행한다. 이 shell을 실행시키면 $TB_SID.tip 파일과 tbdsn.tbr 파일이 생성되고 두 파일은 다음과 같다.

- $TB_SID.tip : Tibero 파라미터 파일
- tbdsn.tbr : Tibero Client 접속 설정 파일

이 두 파일또한 환경변수 설정처럼 사용자가 설정하여 사용할 수 있고 실습환경에 맞춰 변경해준다.

- vi $TB_HOME/config/$TB_SID.tip 을 입력해 수정
```
DB_NAME=tibero
LISTENER_PORT=8629
CONTROL_FILES="/tibero/tbdata/tibero/c1.ctl","/tibero/tbdata/tibero/c2.ctl"
DB_CREATE_FILE_DEST=/tibero/tbdata/tibero
LOG_ARCHIVE_DEST=/tibero/tbdata/tibero/arch
MAX_SESSION_COUNT=20
TOTAL_SHM_SIZE=600M
MEMORY_TARGET=1G
```
코드의 내용은 다음과 같다.
- DB_NAME : 데이터베이스 이름
- LISTENER_PORT : 리스너가 사용할 포트번호
- CONTROL_FILES : 컨트롤 파일이 존재하는 위치 (절대경로)
- MAX_SESSION_COUNT : 데이터베이스에 접속 가능한 최대 세션 수
- TOTAL_SHM_SIZE : 데이터베이스의 인스턴스 내에서 사용할 전체 공유 메모리의 크기
- MEMORY_TARGET : 데이터베이스가 사용할 수 있는 메모리의 총 크기

- cat $TB_HOME/client/config/tbdsn.tbr : tbdsn.tbr 파일은 따로 수정할 것이 없어서 cat 명령어로 확인만 해주었다. 하지만 DB_NAME이나 새로운 $TB_SID를 설정해 접속하고자 한다면 수정이 필요하다.


## 5. Tibero 인스턴스 기동 (nomount 모드)

현재 database가 생성되기 전이기 때문에 Tibero를 인스턴스까지 기동을 하여 database를 추가해 주어야 한다. 인스턴스 기동을 하게 되면 nomount 모드로 시작이 되고 sys user로만 접속이 가능하게 된다.

1. Tebero를 nomount로 부팅
```shell
$ tbboot nomount
listener port = 8629
change core dump dir to /tibero/tibero7/bin/prof

Tibero 7

Copyright (c) 2008, 2009, 2011 Tibero Corporation. All rights reserved.

Tibero instance started suspended at NOMOUNT mode.
```

2. System 유저로 접속 (nomout모드는 System유저로만 접속이 가능하다.)
```shell
$ tbsql sys/tibero
tbSQL 7

TmaxTibero Corporation Copyright (c) 2020-. All rights reserved.

Connected to Tibero.
SQL>
```

## 6. Database 생성

이렇게 System 유저로 접속 후 SQL 스크립트에서 Database를 생성한다.

```
SQL> CREATE DATABASE 
          USER sys IDENTIFIED BY tibero
          MAXDATAFILES 256
          CHARACTER SET MSWIN949        -- UTF8, EUCKR, ASCII ...
          NATIONAL CHARACTER SET UTF16
          LOGFILE 
              GROUP 0 'log01.log' SIZE 50M,
              GROUP 1 'log11.log' SIZE 50M,
              GROUP 2 'log21.log' SIZE 50M
          MAXLOGFILES 100
          MAXLOGMEMBERS 2
          NOARCHIVELOG
              DATAFILE 'system001.dtf' SIZE 100M
              AUTOEXTEND ON NEXT 64M MAXSIZE 3G
          DEFAULT TEMPORARY TABLESPACE TEMP
              TEMPFILE 'temp001.dtf' SIZE 100M
              AUTOEXTEND ON NEXT 64M MAXSIZE 3G
              EXTENT MANAGEMENT LOCAL AUTOALLOCATE
          UNDO TABLESPACE UNDO
              DATAFILE 'undo001.dtf' SIZE 200M
              AUTOEXTEND ON NEXT 64M MAXSIZE 3G
              EXTENT MANAGEMENT LOCAL UNIFORM SIZE 128k
          DEFAULT TABLESPACE USR
             DATAFILE 'usr001.dtf'    SIZE 50m
               AUTOEXTEND ON NEXT 64m MAXSIZE 3G
          SYSSUB
             DATAFILE 'syssub001.dtf' SIZE 50m
               AUTOEXTEND ON NEXT 64M MAXSIZE 3G ;

Database created.
SQL> Q
```

## 7. Tibero 인스턴스 기동 (normal 모드)

데이터베이스를 추가한 후 다시 tibero를 기동한다.

```
$ tbdown
Tibero instance terminated (NORMAL mode).

$ tbboot
listener port = 8629
change core dump dir to /home/tibero/tibero7/bin/prof

Tibero7

Copyright (c) 2008, 2009, 2011 Tibero Corporation. All rights reserved.

Tibero instance started up (NORMAL mode).
```

## 8. System object 생성용 shell 실행

그 후 이 과정이 중요한데 현재 Tibero 에는 데이터베이스의 파일만이 저장이 되어있기 때문이 이 Data의 Dictionary, system 패키지를 생성해야 우리가 데이터를 조회하고 기능을 수행할 수 있다.

```bash
$ cd $TB_HOME/scripts
$ sh system.sh
Createing ...
...
...
...
```
sys의 패스워드는 tibero, syscat의 비밀번호는 syscat으로 입력후 다른 항목들은 모두 Y를 입력하여 생성해주었다.

<br>

이 과정이 끝나면 환경설정은 모두 마쳤고 **tbsql sys/tibero** 를 통해 접속하여 sql문을 실행시킬 수 있다. 과정들이 하나라도 빠지거나 설정을 자칫 잘못했을 때 오류가 생기고 접속이 안되어서 꽤나 어려운 환경설정과정이었다.
  
  
  




