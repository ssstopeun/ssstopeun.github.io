---
title: Tibero Lecture4. Tibero Tools
date: 2022-08-24 15:00:00 +0900
categories: [Tibero DBMS, Education]
tags: [Tibero, Backend, SW, DBMS] 
author: author_id 
---

# Tibero Tools
> Tibero Tool이란 Tibero에서 제공하는 각종 유틸리티 도구로 이 도구들의 사용법에 대한 학습을 한다.

## 0. Tibero Utility
> Tibero Utility에는 tbSQL, tbStudio, tbExport/tbImport, tbLoader, T-Up, 기타 Utility (tbrmgr, tbpc, tbdv) 이 있고 이들을 하나씩 알아보겠다.

## 1. tbSQL
> tbSQL은 앞서 실습에서 계속 사용했던 기능으로 **$tbsql**로 실행시켜 일반적인 SQL 문장 및 tbPSM 프로그램을 입력, 편집, 저장, 실행할 수 있다.

<주요기능>
- 트랜잭션 설정 및 종료
- 스크립트를 통한 일괄 실행
- DBA에 의한 데이터베이스 관리
- 데이터베이스의 기동 및 종료
- 외부 유틸리티 및 프로그램의 실행
- tbSQL 환경 설정

## 2. tbStudio
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

## tbLoader