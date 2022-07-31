---
title: Day15.Spring Framework (5)
date: 2022-07-31 16:00:00 +0900
categories: [Backend, SpringBoot]
tags: [SpringBoot, Backend, SW] 
author: author_id 
---

# [Day15] Spring Framework 핵심개념 (5)

## Logging
> 시스템을 작동할 때 시스템의 작동 상태의 기록과 보존, 이용자의 습성 조사 및 시스템 동작의 분석 등을 하기 위해 작동중의 각종 정보를 기록해야하는데 이 기록을 만드는 것이 로깅이다.  
> **로그 시스템의 사용에 관계된 인련의 [사건]을 시간의 겨과에 따라 기록하는 것이다.**

### Java Loggin Framework
- java.util.logging
- Apache Commons logging
- Log4J
- Logback
- SLF4J (Simple Logging Facade for Java)

### SLF4J
> Logging Framework들을 추상화 해놓은 것이다. Facade Pattern을 이용한 Logging Framework이다.

- Facade 패턴 : 많은 서브시스템을 거대한 클래스로 만들어 감싸서 편리한 인터페이스를 제공하는 것이다.

### 로깅 프레임워크의 Binding 모듈
> SLF4J가 다양한 로깅 프레임워크를 지원하는데 이는 바인딩 모듈을 통해서 처리된다. 바인딩 모듈은 로깅 프레임워크를 연결하는 역할을 한다.

- Log Level
  - trace
  - debug
  - info
  - warn
  - error

## Logger
---
### logback 설정하기
1. logback-test.xml 파일을 먼저 찾습니다.
2. 없다면 logback.groovy 을 찾습니다.
3. 그래도 없다면 logback.xml을 찾습니다.
4. 모두 없다면 기본 설정 전략을 따릅니다. BasicConfiguration

### 로그 Appender 설정
- ConsoleAppender
- FileAppender
- RollingFileAppender