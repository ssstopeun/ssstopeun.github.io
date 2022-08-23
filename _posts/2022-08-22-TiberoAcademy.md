---
title: Tibero Lecture3. Tibero Database Link
date: 2022-08-16 15:00:00 +0900
categories: [Tibero DBMS, Education]
tags: [Tibero, Backend, SW, DBMS] 
author: author_id 
---

# Tibero Database Link

![Desktop View](/assets/img/2022.08/22-1.jpg){: width="70%" }

- B 에 특화된 GateWay를 사용하여야 한다.
- GateWay의 역할은 B와 A의 연결을 위해 변형을 하는 역할을 한다.
- GateWay 구성을 위해 Client Library가 필요하고 이는 설치, 설정문제에서 보다 복잡하다.

## Tibero to Tibero (TtoT)
- 위의 사진에서 A, B가 Tibero인 경우이다.
- 이 경우에는 A와 B가 같아 변형이 필요없기 때문에 GateWay가 필요없다.

## Tibero to Oracle (TtoO)
- GateWay가 사용할 Oracle Interface Driver가 필요하다.  

## Public DBLink
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
> 데이터베이스 링크를 생성한 사용자만 데이터베이스 링크를 사용할 수 있다.
> Private DBLink를 생성하기 위해서는 create database link권한이 있어야 한다.

생성과 제거는 Public DBLink와 같지만 사용하기 위해서는 권한이 필요하다는 차이가 있다.

<br>

그렇다면 Tibero to Tibero 와 Tibero to Oracle을 하는 과정을 실습과 함께 알아보자.
<br>


## Tibero to Tibero (TtoT)

![Desktop View](/assets/img/2022.08/22-2.png){: width="70%" }

다음그림은 실습진행의 architecture이다. Tibero to Tibero이니 왼쪽의 PC에 있는 Virtualbox안에 T1 서버도 Tibero환경, 오른쪽의 강사가 셋팅한 서버또한 Tibero환경이다. 따라서 두 환경사이에서는 GateWay없이 쿼리와 결과를 전달받을 수 있다.