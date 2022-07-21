---
title: Day11-2.Spring Framework (1)
date: 2022-07-18 13:28:00 +0900
categories: [Backend, SpringBoot]
tags: [SpringBoot, Backend, SW] 
author: author_id 
---

# Spring Framework 핵심개념 (1)

## 0. SpringBoot 사용법
---
1. Manual Setup : Maven / Gradle로 프로젝트 생성후 pom.xml / build.gradle을 직접 수정한다.
2. Spring Boot : <https://docs.spring.io/spring-boot/docs/2.5.0/reference/html/getting-started.html#getting-started> 
3. Spring Boot CLI
   - 설치 : <https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html#getting-started.installing.cli>
   - 명령어 실행 : <https://docs.spring.io/spring-boot/docs/current/reference/html/cli.html>
4. Spring initializ 이용 : Intellij의 Spring Initializr

개인적으로 Spring Initializ가 가장 편했습니다.

## 1. SpringBoot의 핵심개념
---

- Spring IoC 컨테이너 / Beans
- 리소스 핸들링 (Resource와 ResourceLoader)
- 벨리데이션과 데이터 바인딩, 타입 변환
- 스프링 expression 언어
- AOP
- Null-safety
- 데이터 버퍼와 코덱
- 로깅

이 중 가장 핵심적인 내용 위주로 살펴보자.

## 2. Domain Driven Design
---

앞으로 스프링 부분 강의를 들으면서 **주문관리 애플리케이션**을 만들 것인데 이때 나오는 몇가지 용어들을 정리하고자 한다.

### Domain
Domain은 **비즈니스 그자체**를 말하는데 예를들어 주문관리 애플리케이션에서는 **주문에 관한 관리 자체**가 Domain이 된다.  
여기서 자바기반으로 Domain Driven Design을 하게 되면 class로 작성하고 모두 객체로 만들어진다. 객체에는 종류가 있는데 **Entity와 Value Object**가 있다.

### Entity
> 앤터티는 다른 앤터티와 구별할 수 있는 **식별자**를 가지고 있고 시간에 흐름에 따라 **지속적으로 변경되는 객체**이다.

![Desktop](/assets/img/2022.07/18-4.JPG){:width:70%}

다음 모델의 "주문" 객체는 "주문 Id"와 같은 식별자를 가지고 있고 지속적으로 변경되는 Entity이다.
<br>

여기서 VO가 Value Object이다.

### Value Object
> 값 속성이 개별적으로 변화하지 않고 **값 그 자체로 고유한 불변 객체**이다.

위에 그림에서 처럼 주문의 주문자를 보자. 이는 주문자의 이름이라는 string 값 자체로 고유한 불변 객체인 것이다.  
불변객체는 **record**를 사용하면 쉽게 만들 수 있다.  
<br>

**<span style = "background-color: #fff5b1">식별자의 유무가 Entity와 Value Object의 차이가 아니다. Value Object도 식별자를 가질 수 있다. 지속적으로 변경하는지 불변객체인지가 핵심!!!</span>**

## 3. 의존성 관리
---

### 의존성
> 어떤 객체가 협력하기 위해 다른 객체를 필요로 할때 두 객체 사이의 의존성이 존재하게 된다.  
> 의존성은 실행 시점과 구현 시점에 서로 다른 의미를 가진다.

- 컴파일타임 의존성 : 코드를 작성하는 시점에서 발생하는 의존성 (클래스 사이의 의존성)
- 런타임 의존성 : 테스트할때 객체를 고르는 것으로 애플리케이션이 실행되는 시점의 의존성 (객체 사이의 의존성)
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

### Inversion of Control (제어의 역전) : IoC
> 제어의 흐름이 역전되는 것  
**객체가 자신이 사용할 객체를 스스로 생성, 선택하지 않는다.**

```java
// 1. Order 스스로 Voucher를 결정하는 경우
public class Order{
   ...
   
   public Order(UUID orderid, UUID customerId, List<OrderItem> orderItems){
      this.orderid = orderid;
        this.customerId = customerId;
        this.orderItems = orderItems;
        this.voucher = new FixedAmountVoucher(UUID.randomUUID(), 10L);
   }
}

// 2. 다른 class에서 어떤 Voucher를 사용할지 주입받는 IoC가 발생하는 경우
public class Order{
   ...
   
   public Order(UUID orderid, UUID customerId, List<OrderItem> orderItems, Voucher voucher){
      this.orderid = orderid;
        this.customerId = customerId;
        this.orderItems = orderItems;
        this.voucher = voucher;
   }
}
public class OrderTester {
   public static void main(String[] args){
      var fixedAmountVoucher = new FixedAmountvoucher(UUId.randomUUID(), 10L);
      Order(~ , fixedAmountVoucher);
   }
}
```

간단하게 코드로 표현해보았다.
1번 코드의 경우 어떤 Voucher를 사용할지 Order class안에서 직접 결정하고 이는 Order가 직접 제어를 하고 있다고 말할 수있다.  
하지만 2번의 경우 Order에서 결정하는 것이 아니라 OrderTester에서 어떤 Voucher를 사용할지 결정해 Order는 어떤 Voucher를 사용할지 주입을 받게 된다. 이것이 바로  Order가 OrderTester에 의해 제어가 되고 있는 IoC상황인 것이다.


**IoC에 대해서는 추가공부가 필요한 것 같다.**
