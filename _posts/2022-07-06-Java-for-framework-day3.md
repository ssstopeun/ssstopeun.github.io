---
title: Day3.Interface
date: 2022-07-06 05:29:00 +0900
categories: [Backend, Java for framework]
tags: [Java, Backend, SW] 
author: author_id 
---

# [DAY3] Interface
---
> 오늘은 Interface에 관해 배워보고자 한다.
<br>

## 0. Interface란?
---
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
---
> **Interface는 개발코드를 직접수정하지 않고 사용하고 있는 객체를 변경할 수 있게 한다.**

### (1) 구현을 강제한다.
#0에서 말했다시피 인터페이스는 추상메소드와 상수만 구현되어 객체를 어떻게 구성해야 하는지 알려준다.   
따라서 이 인터페이스를 상속받은 클래스는 모든 메소드를 오버라이드하며 설계도를 따라 객체를 구성해나가야 하는 것이다. 
<br>

### (2) 다형성을 제공한다.
인테페이스의 가장 큰 장점이다. 인터페이스와 로직이 명확하게 분리되어 실제 메소드의 구현은 상속받은 자식 클래스에서 담당하기 때문에 외부에 노출할 필요 없는 로직을 캡슐화할 수 있다.
<br>

### (3) 결합도를 낮추는 효과 (의존성을 역전)
이 부분이 가장 어려웠던 것 같다. Dependency Injection, Dependency Inversion등의 용어가 이해가 되지않았다.  

<br>
우선 Dependency (의존관계) 부터 천천히 알아보자.

---

#### Dependency : 의존관계
> 의존 대상 B가 변경되었을 때 그 영향이 A에 미치는 관계

<br>
예를 든 것중에 "음식레시피" 예시가 가장 이해가 잘되었다.

> 쿠키요리사는 쿠키레시피에 의존한다. 만약 쿠키레시피가 변경되다면 요리사는 쿠키를 새로운 방법으로 만들게 된다. **"요리사는 레시피에 의존한다"** 라고 말할 수 있는 것이다.


```java
public class CokkieChef {
    private HamburgerRecipe cokkieRecipe;

    public CokkieRecipe() {
        this.cookieRecipe = new CokkieRecipe();
        //this.cookieRecipe = new ChockoCokkieRecipe();
    }
}
```
이렇게 코드를 작성한다면 두 클래스 간 결합성이 높다는 문제가 발생한다.  
CookieChef클래스와 CokkieRecipe 클래스가 강하게 결합되어 초코쿠키 레시피를 사용하고 싶다면 주석처럼 CookieChef의 생성자를 직접 바꿔주어야 한다.  <span style="color: blue">즉, 유연성이 떨어지는 것이다.</span>

<br>

이를 **Dependency Injection**가 해결할 수 있다.
#### Dependency Injection (DI) : 의존관계 주입
> 의존관계를 외부해서 결정(주입) 해주는 것을 말한다.   

우선 다양한 쿠키 레시피를 **Interface**를 사용해 추상화 하자.
```java
public interface CookieRecipe{

}
public class ChockoCookieRecipe implements CookieRecipe{

}
```
<br>
그 후 CokkieChef 클래스의 생성자에서 외부로부터 쿠키레시피를 주입 (Injection) 받도록 하는 것이다.

```java
public class CookieChef{
    private CookieRecipe cookieRecipe;
    
    public CookieRecipe(CookieRecipe cookieRecipe){
        this.cookieRecipe = cookieRecipe;
    }
}

public class Customer{
    private CookieChef cookieChef = new CookieChef(new CokkieRecipe());

    public void orderMenu(){
        cookieChef = new CokkieChef(new ChockoCokkieRecipe());
    }
}
```
이렇게 되면 customer가 레시피를 결정해 Chef에게 주입할 수 있다. 이것이 **Dependency Insection** 이다.  

<br>
이때 Dependency Inversion이 일어나게 되는데

#### Dependency Inversion : 의존관계 역전

![Desktop View](/assets/img/2022.07/09-1.JPG){: width="70%" }

왼쪽 그럼처럼 A가 B(구상체)와 직접적으로 의존관계를 맺는 것이 아니라 오른쪽 그림처럼 interface라는 추상체를 중간에 둬서 추상체를 통하여 의존하게 하는 것이다. 이 때 발생하는것이 **Dependency Inversion** 이다. 이는 의존성을 역전해서 사용하라는 객체지향 5원칙중 **DIP (Dependency Inversion Principle)** 에 해당한다.




## 2. default Method 기능
## 3. Functional Interface
## 4. Lambda 표현식
