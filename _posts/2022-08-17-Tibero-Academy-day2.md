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

이 순서대로 실습한 과정을 살펴보자. 실습 과정은 실행해야하는 T1 Virtual Machine과 putty가 설치되어 있는 환경을 바탕으로 진행되었다.

## 0. 환경실행 & linux 명령어
---
> T1 Virtual Machine을 실행시킨 후 putty에서 작업을 진행한다.

- 해당 실습을 linux를 사용하기에 linux명령어를 아는 것이 중요하다.

## 1. 설치 파일 준비
---
- 티베로 테크넷 <https://technet.tmaxsoft.com/ko/front/main/main.do> 홈페이지에 접속해 데모라이선스 신청을 클릭한다.
- 회원가입을 해야하고 그 후 신청을 하면 본인 메일로 라이선스가 도착한다.
- 이 라이선스와 tibero7 바이너리 압축파일을 준비해 linux 환경에 저장해 놓으면 된다.
- linux가상환경과 연결된 파일에 저장을 하는 방식으로 진행하였다.

## 2. 환경설정 파일에 환경변수 설정
---

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

- 다음의 코드를 환경변수로 추가하면 된다.
- **vi ~/.bash_profile** 를 입력하여 vi편집기를 통해 위의 코드를 추가해주면 된다.
- 그 후 **source ~/.bash_profile** 를 사용하여 환경변수를 적용합니다. 이는 바꾼 /.bash_profile을 적용하는 과정입니다.


