---
title: Tibero Lecture4. Tibero Tools
date: 2022-08-24 15:57:13 +0900
categories: [Tibero DBMS, Education]
tags: [Tibero, Backend, SW, DBMS] 
author: author_id 
---

# Tibero Tools
> Tibero Tool이란 Tibero에서 제공하는 각종 유틸리티 도구로 이 도구들의 사용법에 대한 학습을 한다.

## 0. Tibero Utility
---
> Tibero Utility에는 tbSQL, tbStudio, tbExport/tbImport, tbLoader, T-Up, 기타 Utility (tbrmgr, tbpc, tbdv) 이 있고 이들을 하나씩 알아보겠다.

## 1. tbSQL
---
> tbSQL은 앞서 실습에서 계속 사용했던 기능으로 **\$tbsql**로 실행시켜 일반적인 SQL 문장 및 tbPSM 프로그램을 입력, 편집, 저장, 실행할 수 있다.

\<주요기능>
- 트랜잭션 설정 및 종료
- 스크립트를 통한 일괄 실행
- DBA에 의한 데이터베이스 관리
- 데이터베이스의 기동 및 종료
- 외부 유틸리티 및 프로그램의 실행
- tbSQL 환경 설정

## 2. tbStudio
---
> 처음 배운 Tibero tool로 마치 mySQL workbench 같은 느낌이었다. 이는 Tibero를 이용하는 개발을 돕는 GUI Tool로써 개발에 필요한 기능과 환경을 제공한다.  
그 중 주요기능인 Export/Import 기능을 실습해 보았다.

기본적으로 editor에서는 기본 SQL문을 작성해 결과를 확인할 수 있다. 그 밖에 DB 구조와 데이터를 binary 파일로 Export 및 Import 할 수 있다.
<br>

![Desktop View](/assets/img/2022.08/24-1.PNG){: width="70%" }

우선 초기 data를 보면 이렇게 DEPT Table과 그 속의 4개의 columns이 존재하는 것을 볼 수 있다. 그렇다면 이 자료를 Export를 통해 binary파일로 저장해놓고 Dept Table을 삭제한 후 binary파일을 import하여 다시 가져와보겠다.

### Export
![Desktop View](/assets/img/2022.08/24-2.PNG){: width="70%" }

이렇게 Export File의 경로를 설정해준 후 Full Databse를 클릭해 모든 Data를 파일로 저장해 주었다. 
<br>

![Desktop View](/assets/img/2022.08/24-3.PNG){: width="70%" }

그 후 DEPT table을 삭제하고 select문을 통해 DEPT Table이 존재하지 않는 것을 확인했다. 
<br>

이 상태에서 Import를 통해 binary파일 속 DEPT Table을 가져와 보자.

### Import

![Desktop View](/assets/img/2022.08/24-4.PNG){: width="70%" }

이렇게 가져올 수 있는데 파일 속 내용중 User와 Table을 입력하여 Import가 가능하고 실습에서는 Tibero user에 아까 drop한 DEPT Talbe을 Import해주었다.
<br>

![Desktop View](/assets/img/2022.08/24-5.PNG){: width="70%" }

Import를 하고나면 다시 DEPT Table이 조회되는 것을 알 수 있다.

<br>
이 과정은 putty의 shell환경에서도 tbExport/tbImport로 할 수 있다.

## tbExport/tbImport
---
> shell 환경에서 이루어지는 export/import로 로컬 tibero에서 full모드로 export하고 dept 테이블을 일부러 손상시킨 후 export해놓은 파일을 import해 데이터를 복원하는 과정을 진행할 것이다.

![Desktop View](/assets/img/2022.08/24-5.PNG){: width="70%" }

다음과 같이 tibero에 tbexport, tbimport기능이 있는것을 확인할 수 있다.

### 1. tbexport하기
> 우선 tbexport를 이용하여 full모드로 export를 진행하여 data의 내용을 파일로 저장한다.

```bash
[tibero@T1:/tibero]$ mkdir expimp
[tibero@T1:/tibero]$ cd expimp
```

tbexport를 할때 생성될 파일을 저장할 디렉토리부터 생성해준다.

```bash
[tibero@T1:/tibero/expimp]$ tbexport IP=localhost PORT=8629 SID=tibero USERNAME=sys PASSWORD=tibero FULL=Y FILE=/tibero/expimp/default.dat SCRIPT=Y
```

tbexport의 타겟이되는 서버의 IP, PORT번호, SID, USERNAME, PASSWORD를 입력해주고 본 실습에서는 FULL로 export할 것이라 FULL=Y, 그리고 이 파일이 저장될 경로와 파일이름을 지정해준다. 파일이름은 default.dat으로 tbStudio에서 진행한 Export 파일의 이름과 동일하다.
<br>

그럼 /tibero/expimp 폴더에 다음과 같이 생성이 된다.

![Desktop View](/assets/img/2022.08/26-4.PNG){: width="70%" }


### 2. tbImport하기
> 위에서 생성한 default.dat파일로 손상된 데이터를 tbImport를 통해 복원해보겠다.

```
[tibero@T1:/tibero/expimp]$ tbimport IP=localhost PORT=8629 SID=tibero USERNAME=sys PASSWORD=tibero TABLE=TIBERO.DEPT IGNORE=Y;
```

파일만 있으면 tbImport는 간단히 해결할 수 있다.



## tbLoader
---
> tbLoader는 있는 데이터를 export하고 import하는 것과 다르게 table의 구조와 table속 data의 내용파일을 저장하고 이 둘을 local에서 tbLoader를 통해 table생성을 하는 것이다.  tbLoader는 Tibero 유틸리티안내서의 분리된 레코드 부분을 참고하여 실습하였다. (안내서를 잘 참고하는 것이 중요한거 같다 :> )

### 1. 테이블 생성하기
- loader폴더를 만든후 tbsql을 통해 SQL에 접속해 database를 생성한다. 그럼 데이터는 없는 빈 table이 생성된다.

### 2. control.ctl 파일 생성하기
> control.ctl이란 테이블에 들어갈 데이터가 있는 파일의 경로, 이름과 이를 어떤 table에 넣을지, 등이 들어있다. 파일의 내용을 보며 살펴보자.

```
LOAD DATA
INFILE './data.dat'                 -> ./data.dat 의 데이터를 load할거에요
APPEND                              
INTO TABLE club             -> club table에 넣을거에요(club table 미리 생성 필요)
FIELDS TERMINATED BY ','
       OPTIONALLY ENCLOSED BY '"'   -> ','나 '"' 로 데이터가 구분되요.
LINES TERMINATED BY '|\n'           -> line의 구분은 | 와 줄바꿈이에요.
IGNORE 1 LINES                      -> 첫째줄은 무시해요.
(
 id integer external, 
 name, 
 masterid integer external
)
```
<br>

이런식인데 tibero test를 볼땐 무시할 첫째줄이 없었고 line의 구분이 '|' 없이 줄바꿈만 되어 있어서 이렇게 작성했다. 또한 당연히 파일이름이나 table이름은 조건에 맞춰 바꿔줘야한다 :) 
```
LOAD DATA
INFILE [데이터경로/이름]
APPEND
INTO TABLE [table 이름]
FIELDS TERMINATED BY ','
       OPTIONALLY ENCLOSED BY '"'
LINES TERMINATED BY '\n'
```

### 3. data.dat 파일 작성하기
> data.dat파일은 위에서 생성한 table에 들어갈 data들이다. 이 data들을 control.ctl파일을 통해 설정해 tbLoader를 통해 load하는 것이다.

```
id     name      masterid|
111115,"DANCE MANIA",2456|
111116,"MUHANZILZU",2378|
111117,"INT'L",5555
```

위에 설명한 control.ctl 처럼 첫째줄은 무시하고 줄구분은 '|' 와 줄바꿈으로 하는 것을 확인할 수 있다.

### 4. tbLoader 로 load하기
```bash
[tibero@T1:/tibero/loader]$ tbloader userid=tibero/tmax@tibero control=./control.ctl
```
마지막으로 데이터를 넣을 스키마의 아이디, 비번, alias를 입력하여 설정파일로 load해주면 끝!

## T-UP
---
- T-UP은 T-Studio 와 비슷한 tool이다.

/<T-UP 과정>
- T-UP 은 티맥스 테크넷에서 다운로드 받을 수 있다.
- 실습시, 강사가 제공한 다음 파일 실행하여 진행함.
  C:\tibero\SW1\T-UP\T-Up_20220810_win\T-Up.x86_64.bat

- 오라클 접속을 위해 오라클 jdbc 드라이버가 필요하다.(오라클 홈페이지에서 다운로드)
- 강사가 공유폴더에 올려놓은 ojdbc6.jar 을 다운받아서, T-Up 의 lib 디렉토리에 넣어서 사용함
- 마이그레이션은 요약하면 2가지를 옮기는 작업임(DDL, DATA)
- SOURCE DB, TARGET DB 양쪽에 접속이 필요함(인터페이스 드라이버 파일, 접속정보)
- 무엇을 마이그레이션 할것인지 선택해야 함.(실습시, 오라클의 EDU 스키마를 선택함) 