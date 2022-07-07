---
title: /[Day3] Interface
date: 2022-07-06 05:29:00 +0900
categories: [Backend, Java for framework]
tags: [Java, Backend, SW] 
author: author_id 
---

# [DAY3] Interface 이야기
> 오늘은 Interface에 관해 배워보고자 한다.
<br>

## 0. Interface란?
> **객체를 어떻게 구성해야하는지 알려주는 설계도**

<br>
```java
interface interfaceName{
    public static final int = 5;    
    // int = 5;
    public abstract void Method1(); 
    // void Method1();
}
```
이런 식으로 사용할 수 있다.  
주석과 같은 의미로 사용할 수 있는데 인터페이스에서 메소드는 무조건 추상메소드, int는 무조건 상수이기 때문에 public static final 과 public abstract void가 생략가능하다.

## 1. Interface의 기능
> **Interface는 개발코드를 직접수정하지 않고 사용하고 있는 객체를 변경할 수 있게 한다.**

### (1) 구현을 강제한다.
#0에서 말했다시피 인터페이스는 추상메소드와 상수만 구현되어 객체를 어떻게 구성해야 하는지 알려준다.   
따라서 이 인터페이스를 상속받은 클래스는 모든 메소드를 오버라이드하며 설계도를 따라 객체를 구성해나가야 하는 것이다. 
<br>

### (2) 다형성을 제공한다.
인테페이스의 가장 큰 장점이다. 인터페이스와 로직이 명확하게 분리되어 실제 메소드의 구현은 상속받은 자식 클래스에서 담당하기 때문에 외부에 노출할 필요 없는 로직을 캡슐화할 수 있다.
<br>

### (3) 결합도를 낮추는 효과 (의존성을 역전)
<!--
이 말이 가장 어려웠다. Dependency Injection, Dependency Inversion등의 용어가 나오는데 머리가 아팠다.
<br>
우선 **Dependency Injection** 부터 알아보자.
<br>
---

#### Dependency Injection (DI) : 의존관계 주입

수업때 했던 예로 로그인이 있다. 우리는 kakao로 로그인할때와 naver로 로그인할때에 따라 아이디와 비밀번호가 달라진다. 뭘로 로그인하느냐에 따라 로그인방법이 달라지는 것이다.   
이는 ***"user가 로그인종류에 의존한다"*** 라고 말할 수 있다.  
코드로 표현하면 다음과 같다.
```java
class User{
    private login Login;

    public User(){
        login = new Login;
    }
}
```
---

위의 코드는 단순히 login에만 의존하게 된다. 하지만 naver, kakao등 다양한 로그인을 의존받을 수 있게 하기위해 ***"인터페이스 추상화"***가 필요한 것이다.  
이 과정을 거치게 되면 인터페이스를 통해 더 다양한 의존 관계를 맺을 수 있고, 실제 구현 클래스와는 관계가 느슨해져 결합도가 낮아지게 된다.

---

그래서 결론적으로 **DI**란 

-->




## 2. default Method 기능
## 3. Functional Interface
## 4. Lambda 표현식
