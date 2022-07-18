---
title: Day11-2.Start SpringBoot
date: 2022-07-18 13:28:00 +0900
categories: [Backend, SpringBoot]
tags: [SpringBoot, Backend, SW] 
author: author_id 
---

# SpringBoot

## SpringBoot 사용법
---
1. Manual Setup : Maven / Gradle로 프로젝트 생성후 pom.xml / build.gradle을 직접 수정한다.
2. Spring Boot : <https://docs.spring.io/spring-boot/docs/2.5.0/reference/html/getting-started.html#getting-started> 
3. Spring Boot CLI
   - 설치 : <https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html#getting-started.installing.cli>
   - 명령어 실행 : <https://docs.spring.io/spring-boot/docs/current/reference/html/cli.html>
4. Spring initializ 이용 : Intellij의 Spring Initializr

개인적으로 Spring Initializ가 가장 편했습니다.

## SpringBoot의 핵심개념
- Spring IoC 컨테이너 / Beans
- 리소스 핸들링 (Resource와 ResourceLoader)
- 벨리데이션과 데이터 바인딩, 타입 변환
- 스프링 expression 언어
- AOP
- Null-safety
- 데이터 버퍼와 코덱
- 로깅

이 중 가장 핵심적인 내용 위주로 살펴보자.

## Domain Driven Design
---
앞으로 스프링 부분 강의를 들으면서 **주문관리 애플리케이션**을 만들 것인데 이때 나오는 몇가지 용어들을 정리하고자 한다.

### Entity
> 앤터티는 다른 앤터티와 구별할 수 있는 식별자를 가지고 있고 시간에 흐름에 따라 지속적으로 변경되는 객체이다.

![Desktop](/assets/img/2022.07/18-4.JPG){:width:70%}
다음 사진을 보게 되면 주문이라는 것은 지속적으로 변경되는 객체, 즉 Entity이고 주문은 주문마다 독립성을 가진다. 또한 주문 예약번호, 주문자 ID등의 식별자가 있다.  
<br>
여기서 VO는 Value Object이다.

### Value Object
> 값 속성이 개별적으로 변화하지 않고 값 그 자체로 고유한 불변 객체이다.

위에 그림에서 처럼 주문의 주문자를 보자. 이는 주문자의 이름이라는 string 값 자체로 고유한 불변 객체인 것이다.