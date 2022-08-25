---
title: Tibero Lecture3. Tibero Database Link
date: 2022-08-22 14:27:48 +0900
categories: [Tibero DBMS, Education]
tags: [Tibero, Backend, SW, DBMS] 
author: author_id 
---

# Tibero Database Link

![Desktop View](/assets/img/2022.08/22-1.JPG){: width="70%" }

- B 에 특화된 GateWay를 사용하여야 한다.
- GateWay의 역할은 B와 A의 연결을 위해 변형을 하는 역할을 한다.
- GateWay 구성을 위해 Client Library가 필요하고 이는 설치, 설정문제에서 보다 복잡하다.

## Tibero to Tibero (TtoT)
---
- 위의 사진에서 A, B가 Tibero인 경우이다.
- 이 경우에는 A와 B가 같아 변형이 필요없기 때문에 GateWay가 필요없다.

## Tibero to Oracle (TtoO)
---
- GateWay가 사용할 Oracle Interface Driver가 필요하다.  


## Public DBLink
---
> 데이터베이스 링크를 생성한 사용자와 다른 사용자들도 데이터베이스 링크를 이용할 수 있.
> Public DBLink를 생성하기 위해서는 create public database link 권한이 있어야 한다.

- Public DBLink 생성

```
create public database link public_tibero using [연결할 데이터베이스를 가르키는 이름];
```

위의 괄호에 들어갈것은 tbdsn.tbr파일에 연결정보가 저장되어 있어야한다.
<br>

- Public DBLink 제거
```
drop public database link public_tibero;
```

## Private DBLink
---
> 데이터베이스 링크를 생성한 사용자만 데이터베이스 링크를 사용할 수 있다.
> Private DBLink를 생성하기 위해서는 create database link권한이 있어야 한다.

생성과 제거는 Public DBLink와 같지만 사용하기 위해서는 권한이 필요하다는 차이가 있다.

<br>

그렇다면 Tibero to Tibero 와 Tibero to Oracle을 하는 과정을 실습과 함께 알아보자.
<br>


## Tibero to Tibero (TtoT)
---

![Desktop View](/assets/img/2022.08/22-2.PNG){: width="70%" }

다음그림은 실습진행의 architecture이다. Tibero to Tibero이니 왼쪽의 PC에 있는 Virtualbox안에 T1 서버도 Tibero환경, 오른쪽의 강사가 셋팅한 서버또한 Tibero환경이다. 따라서 두 환경사이에서는 GateWay없이 쿼리와 결과를 전달받을 수 있다.
<br>

### 1. Tibero 클라이언트 설정
> 사진 속 왼쪽환경에서 오른쪽 환경에 대한 설정을 해주는 것으로 PC Virtualbox안에 T1서버의 tbdsn.tbr파일에 강사님이 셋팅한 서버의 정보를 입력해준다.
<br>

다음은 실습때 진행한 강사님이 셋팅한 서버로 연결할 서버에 맞게 tbdsn.tbr파일에 추가해주면 된다.
```
Tibero2=(
        (INSTANCE=(HOST=10.188.191.33)
                  (PORT=8629)
                  (DB_NAME=tibero)
        )
)
```

### 2. tbsql을 통해 DB Link Object 생성
> 서버를 설정했으니 이 서버에 접근해 DB Link Object를 생성한다. 

```
SQL> create database link <DB Link명> connect tso <접속 사용자 ID>
identified by <접속 패스워드> 2 using <접속에 사용할 alias>
```
<br>

적용한 것은 다음과 같다.
```
SQL> create database link TLINK connecto to edu identified 'edu' 2 using 'tibero'
```

그 후 Link가 잘 되었는지 확인해보았다.

![Desktop View](/assets/img/2022.08/25-1.PNG)

<br>

이렇게 설정이 잘됐으면 연결된 서버의 data를 사용할때 **@Link이름**을 붙이면 된다. 그럼 data를 가져와 로컬서버에 저장해보자.

### 3. Data가져오기
- 테이블과 데이터를 모두 가져오기
```SQL
SQL> CREATE TABLE DEPT AS FROM DEPT@TLINK;
```
<br>

- 테이블을 만들어놓고 데이터만 가져오기
```SQL
SQL> CREATE TABLE DEPT AS SELECT * FROM DEPT@TLINK WHERE ROWNUM < 1;
SQL> INSERT INTO DEPT SELECT * FROM DEPT@TLINK;
```
<br>

![Desktop View](/assets/img/2022.08/25-1.PNG)

이렇게 잘 들어온것을 확인할 수 있다 :)

## Tibero to Oracle (TtoO)
---
> 제일 처음 사진의 A가 Tibero, B가 Oracle인 환경으로 Tibero to Tibero 와 다르게 링크의 대상이 Oracle이기 때문에 Oracle을 위한 Gateway가 필요하다. Tibero는 필요한 질의를 Oracle Gateway로 전달하고 Gateway는 Oracle에 접속하여 질의를 수행한 후 Tibero에게 전송해준다.

![Desktop View](/assets/img/2022.08/23-1.PNG){: width="70%" }

다음 그림을 보자면 우리가 Client API를 통해 Tibero에 질의를 작성하고 이 질의를 TBGW라는 Gateway를 통해 OracleClient에게 전달하고 이 질의를 수행한 후 다시 Tibero에게 주는 것이다.
<br>

그렇다면 이를 수행할 수 있는 환경을 조성해보자.

### 1. 오라클 클라이언트 파일 설치
- Tibero to Tibero와 다른 것은 직접 Gateway의 설정을 해주는 것이다. 경로 설정을 통해 Tibero에서 Gateway를 통과했을때 Oracle Client로 향해 기능을 수행할 수 있도록 설정해 주는 것이다.

```bash
[tibero@T1:/tibero]$ cp instantclient-basic-linux.x64-19.16.0.0.0dbru.zip /tibero
[tibero@T1:/tibero]$ unzip instantclient-basic-linux.x64-19.16.0.0.0dbru.zip
[tibero@T1:/tibero]$ ls -l
drwxr-xr-x  2 tibero dba       233 Aug 23 10:46 instantclient_19_16
```
- 이를 통해 zip파일의 압축을 풀어주면 instanclient_19_16 폴더가 위치하게 된다. 이것이 오라클 클라이언트 파일이다.
<br>

### 2. Gateway 환경작업
- 파일을 설치했다면 이 파일에 맞춰진 gateway 환경설정을 해주어야 한다.

```bash
[tibero@T1:/tibero]$ vi ~/.bash_profile
```

- 우선 /.bash_profile을 통해 gateway와 oracle의 home 경로를 설정해준다.
<br>

```
######## TIBERO TO ORACLE DBLINK #######
export TBGW_HOME=/tibero/tbgateway
export ORACLE_HOME=/tibero/instantclient_19_16
export ORACLE_SID=orcl
export LIBPATH=$ORACLE_HOME:$LIBPATH
export LD_LIBRARY_PATH=$LIBPATH:$LD_LIBRARY_PATH
export PATH=$ORACLE_HOME:$PATH
```

- TBGW_HOME과 ORACLE_HOME만 본인의 환경에 맞게 설정해주면되는데 gateway의 home과 연결할 Oracle의 home 경로를 다음과 같이 설정해준다.
- .bash_profile을 수정할때는 source  ~/.bash_profile 을 통해 반영을 꼭 해주자!
<br>

### 3. tnsnames.ora 생성 및 설정
```bash
[tibero@T1:/tibero]$ mkdir -p $ORACLE_HOME/network/admin
[tibero@T1:/tibero]$ cd       $ORACLE_HOME/network/admin
[tibero@T1:/tibero/instantclient_11_2/network/admin]$ vi tnsnames.ora
```

```
ORCL =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 10.188.191.10)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = orcl)
    )
  )
```
- 다음의 경로로 tnsnames.ora 파일을 생성하고 위의 txt를 추가하였다. 이는 연결된 Oracle에 대한 접속정보를 설정하는 것이다.
<br>

### 4. Gateway 바이너리 복사
```bash
[tibero@T1:/tibero]$ cp /tibero/tibero7/client/bin/gw4orcl   /tibero/tbgateway
```
- gw4orcl 이 Tibero에서 제공하는 gateway기능이라고 보면 되는데 이를 /tibero/tbgateway에 위치시키고 tibero to Oracle이 가능하도록 해준다.
<br>

### 5. Gateway 환경설정
- 위의 복사한 Gateway바이너리를 tbgw.cfg 파일을 통해 tibero to Oracle이 가능하도록 설정해주는 과정이다.

```bash
[tibero@T1:/tibero]$ mkdir /tibero/tbgateway
[tibero@T1:/tibero]$ mkdir /tibero/tbgateway/oracle
[tibero@T1:/tibero]$ mkdir /tibero/tbgateway/oracle/config
[tibero@T1:/tibero]$ mkdir /tibero/tbgateway/oracle/log
[tibero@T1:/tibero]$ vi /tibero/tbgateway/oracle/config/tbgw.cfg
```
- 이 과정을 통해 tbgateway에 oracle 폴더와 그 속에 config, log폴더를 만들고 /tbgateway/oracle/config 폴더에 tbgw.cfg 파일을 생성해 다음을 작성한다.
<br>

```
LISTENER_PORT=9998
LOG_DIR=/tibero/tbgateway/oracle/log
LOG_LVL=2
MAX_LOG_SIZE=50240000
```
- 이 파일은 Gateway설정파일로 원래 Tibero에서 진행되었을때는 gateway가 필요없어서 설정해줄 필요가 없었지만 Tibero to Oracle을 할때는 사용자가 Gateway와 관련된 설정값을 변경해주어야 하기 때문에 필요한 설정이다.
<br>

### 6. Network Alias 설정
- Tibero 클라이언트의 Network Alias 설정파일인 tbdsn.tbr 파일에 Gateway의 정보를 설정해주는 과정이다.

```bash
[tibero@T1:/tibero]$ vi $TB_HOME/client/config/tbdsn.tbr
```

```
MOF=(
    (GATEWAY=(LISTENER=(HOST=localhost)(PORT=9998))
             (TARGET=ORCL)
             (TX_MODE=GLOBAL)
    )
)
```

- 이 과정으로 연결할 Oracle network의 정보를 입력하였다. 여길보면 MOF의 Port 번호가 tbgw.cfg에서 설정한 port번호가 같음을 알 수있다.

- **여기서 MOF= 이 아니라 MOF(공백)= 이라고 처음에 작성해 오류가 났었다. 이름뒤에 띄어쓰기 없이 = 을 써야한다.**

### 7. DB Link 생성 및 확인
```bash
[tibero@T1:/tibero]$ cd $TBGW_HOME
```


## 티베로 유틸리티 특징
---
- 접속 정보가 필요하다. (5가지) 
    - ip
    - port
    - db_name
    - username
    - password
- 접속을 위한 인터페이스 드라이버 라이브러리 파일이 필요하다.
    - tbsql : cli library 파일
    - tbexport, tbimport, tbloader, T-up : jdbc library 파일

- tbsql을 제외한 유틸리티들은 동작을 위해 jdk가 설치되어 있어야 한다.
