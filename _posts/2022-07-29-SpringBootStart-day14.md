---
title: Day14.Spring Framework (4)
date: 2022-07-29 14:00:00 +0900
categories: [Backend, SpringBoot]
tags: [SpringBoot, Backend, SW] 
author: author_id 
---

# [Day14] Spring Framework 핵심개념 (4)

## 컴포넌트 스캔
---
> 컴포넌트 스캔이란 스프링이 직접 클래스를 검색해서 Bean으로 등록해주는 기능이다.

설정 클래스, 즉 AppConfiguration에서 Bean으로 따로 등록하지 않아도 원하는 클래스를 빈으로 등록할 수 있다. 그 방법을 알아보자.

### 1. 스트레오타입
이는 번역하면 고정관념이라는 용어인데 UML 다이어그램을 확장시켜주는 도구로서 특정요소를 상황이나 도메인에 맞게 분류해주는 것이다.  
우리가 spring을 사용할때 클래스의 용도에 맞게 @Component, @Controller, @Service, @Configuration 등의 스트레오타입 애노테이션을 사용하여 컴포넌트 스캔을 하도록 할 수 있다. 

## Autowired
---
> @Autowired는 Bean으로 등록이 된 상태에서 자동으로 의존성을 주입해주는 것을 말한다. 필요한 의존 객체의 타입에 해당하는 Bean을 찾아 주입하는 것이다.

Autowired는 다음 3가지 경우에 사용할 수 있다.

1. 생성자
2. setter
3. 필드

```java
 @Autowired
  private VoucherRepository voucherRepository;

//  public VoucherService(VoucherRepository voucherRepository) {
//    this.voucherRepository = voucherRepository;
//  }
```

보는 것과 같이 생성자를 지우고 필드에 Autowired 어노테이션을 추가하는 것으로 의존관계를 주입할 수 있다.

<br>

```java
 @Autowired
  public VoucherService(VoucherRepository voucherRepository) {
    this.voucherRepository = voucherRepository;
  }

  public VoucherService(VoucherRepository voucherRepository, String dummy) {
    this.voucherRepository = voucherRepository;
  }
```
다음 코드를 보면 생성자가 두개가 되는데 기본적으로 생성될 생성자에게 @Autowired를 붙인다. 생성자가 하나만 있는 경우에는 @Autowired 처리를 하지 않아도 자동으로 처리가 된다.
<br>

보통 생성자에 Autowired를 사용하는 것을 권장하는데 이유는 다음과 같다.
1. 초기화시에 필요한 모든 의존관계가 형성되기 때문에 안전하다.
2. 잘못된 패턴을 찾을 수 있게 도와준다.
3. 테스트를 쉽게 해준다.
4. 불변성을 확보한다.

<br>

그렇다면 자동으로 의존관계를 주입해줄때 Service에서 Repository를 생성하는데 이때 Repository가 두개 이상이라면 어떤 Repository를 생성해야하는지 모르게 되는 문제가 발생한다. 이때 어떻게 해야하는지 보자.
<br>

실습중인 코드를 보면 VoucherSerive에서 VoucherRepository를 생성하는데 MemoryVoucherRepository와 jdbcVoucherRepository가 있다고 가정해보자. 그렇다면 이 VoucherRepository들을 구별시켜줘야하는데 방법은 다음과 같다.

```java
@Repository
@Qualifier("memory")
public class MemoryVoucherRepository implements VoucherRepository {
    ...
}

@Repository
@Qualifier("jdbc")
public class jdbcVoucherRepository implements VoucherRepository {
    ...
}
```
@Qualifier을 통해 Repository의 별칭을 정해주는 것이다. 그 후 Service의 생성자에서 이 별칭을 가지고 어떤 Repository를 기본적으로 불러올지 결정한다.

```java
public VoucherService(@Qualifier("memory") VoucherRepository voucherRepository) {
    this.voucherRepository = voucherRepository;
}
```

또한 getBean을 할때에도 
```java 
// getBean을 통해 VoucherRepository를 불렀을때 VoucherRepositoy가 두개라 오류가 발생한다.
var voucherRepository = applicationContext.getBean(VoucherRepository.class);

// 이렇게 생성해주어야 한다.
var voucherRepository = BeanFactoryAnnotationUtils.qualifiedBeanOfType(applicationContext.getBeanFactory(), VoucherRepository.class, "memory");
```
아래와 같이 생성해주어야 하는데 특정 qualified된 Bean의 타입을 가지고 오는것으로 원하는 qualifier를 설정하여 불러올 수 있는 것이다.
<br>

**<span style = "background-color: #fff5b1">하지만 이런식으로 별칭을 주게 되면 사용자가 이 별칭들을 입력해줘야 하기 때문에 보통은 기본값으로 주어질 Repository에 @Primary를 주어 *이 Repository를 기본 Repository로 설정하겠다* 라는 설정을 합니다.</span>**

## Bean Scope
---
> Bean이 어떤 범위로 생성되는지를 말한다.

- singleton   
: 기본적인 scpoe
- prototype
- request  
: web 관련
- session  
: web 관련
- application
- websocket
