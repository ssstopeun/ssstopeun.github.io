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

## 의존성 관리
---

### 의존성
> 어떤 객체가 협력하기 위해 다른 객체를 필요로 할때 두 객체 사이의 의존성이 존재하게 된다.  
> 의존성은 실행 시점과 구현 시접에 서로 다른 의미를 가진다.

- 컴파일타임 의존성 : 코드를 작성하는 시점에서 발생하는 의존성 (클래스 사이의 의존성)
- 런타임 의존성 : 애플리케이션이 실행되는 시점의 의존성 (객체 사이의 의존성)
<br>

![Desktop](/assets/img/2022.07/18-5.JPG){:width:70%}
여기서 Order는 FixedAmountVourcher에 의존하고 있기 때문에 FixedAmountVourcher에 수정사항이 생기면 Order에도 영향을 미친다. 이는 둘의 결합도가 강한 것으로 결합도를 느슨하게 해줄 필요가 있다.

<br>

### 결합도
> 하나의 객체가 변경이 일어날 때에 관계를 맺고 있는 다른 객체에서 변화를 요구하는 정도

- 두 요소가 느슨한 결합도 / 약한 결합도를 가질때 두 요소 사이에 존재하는 의존성이 바람직하다고 한다.
- 반대로 두 요소가 강한 결합도를 가지면 두 요소 사이에 존재하는 의존성은 바람직하지 못한 것이다. 
<br>

![Desktop](/assets/img/2022.07/18-6.JPG){:width:70%}
이렇게 하면 Order는 Voucher가 Percent이든 Fixed이든 관계없이 돌아갈 수 있다.  
<br>

이렇게 강한 결합도를 약한 결합도를 바꾸는게 바로 JavaFramework강의의 Interface부분에서 배웠던 부분이다.    
참고) [Day3.Interface](https://ssstopeun.github.io/posts/Java-for-framework-day3/)