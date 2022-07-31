---
title: Day12.Spring Framework (2)
date: 2022-07-20 19:30:00 +0900
categories: [Backend, SpringBoot]
tags: [SpringBoot, Backend, SW] 
author: author_id 
---

# [Day12] Spring Framework 핵심개념 (2)

## Inversion of Control (제어의 역전) : IoC
---

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
<br>

실습에서는 그림과 같이 조금 더 발전해 OrderContext, 즉 IoC 컨테이너를 사용해 개별 객체들을 생성/파괴를 관장하도록 하는데 OrderService가 OrderRepository를 사용해 Order를 관리하긴 하지만 Interface로 만든 OrderRepository를 구체화하고 생성하는 역할은 OrderContext에서 처리하는 것이다.  
IoC 컨테이너가 Runtime의 의존성을 맺게 해줌으로써 클래스간의 느슨한 결합도를 가능하게 해주는 것이다.    
**<span style = "background-color: #fff5b1">핵심은 class가 자신이 사용할 객체를 스스로 만들지 않고 IoC 컨테이너를 통해 만들게 하여 단단한 결합도 생성을 방지한다.</span>**
<br>

이런 IoC 컨테이너를 Spring에서는 ApplicationContext Interface를 통해 제공한다.
## ApplicationContext
---
ApplicationContext는 BeanFactory를 상속하는데 객체에 대한 생성/조합/의존관계설정 등을 제어하는 IoC의 기본기능을 BeanFactory가 담당하게 된다.

### Bean

Bean이란 IoC Container에 의해 관리되는 객체들을 말하는데 @Bean을 통해 Bean으로 만들어진다 라고 정의한다.

### Configuration Metadata

스프링의 ApplicationContext는 실제 만들어야할 빈 정보를 Configuration Metadata로 부터 받아온다. 이 메타데이터를 이용해서 IoC 컨테이너에 의해 관리되는 객체들을 생성하고 구성한다. 객체들의 도면같은 느낌이라고 보면된다.

### AnnotationConfigApplicationContext

Java 기반으로 설정을 할 때 이 구현체를 사용해 메타데이터를 설정하고 생성할 수 있다.

## Dependency Injection
---

IoC는 다양한 방법으로 만들 수 있다. 그 중 의존관계 주입패턴이 있는데 Order가 어떤 Voucher를 생성할지, OrderService가 어떤 orderRepository 객체를 생성할지 스스로 결정하는 것이 아니라 생성자를 통해 객체를 주입받는 패턴으로 이를 생성자 주입 패턴, Dependency Injection 이라고 한다.

