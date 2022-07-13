---
title: Day3.Interface
date: 2022-07-06 17:29:00 +0900
categories: [Backend, Java for framework]
tags: [Java, Backend, SW] 
author: author_id 
---

# [DAY3] Interface

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

---

> Interface에서 추상메소드가 아닌 메소드 구현을 할 수 있는 것이다.  
> (java 8 부터 생성된 기능이다.)

```java
interface MyInterface {
    void method1(); // 추상메소드
    
    default void sayHello {
        System.out.println("Hello World");
    }
}

public class Main implements MyInterface {
    public static void main(String[] args){
        new Main().sayHello();
    }

    @Override
    public void method1(){
        throw new RuntimeException();
    }
}
```
method1 처럼 추상메소드로 생성되어 Main에서 Override해주어야 하는 것이 아니라 defauld Method로 생성을 하면 기본적인 구현을 할 수 있고 Override하지 않아도 main에서 호출할 수 있다.

<br>

그럼 어떨 때 default Method를 사용해야할까?

---

```java
interface MyInterface {
    void method1();
    void method2();
}

public class Main {
    public static void main(String[] args) {
        new Service().method1();
    }
}

class Service implements MyInterface {

    @ovveride
    public void method1() {
        System.out.println("Hello World");
    }

    @ovveride
    public void method2() {
        //(nothing)
    }

}
```
이 코드를 보게 되면 MyInterface에는 세가지의 추상메소드가 정의되어 있지만 정작 main에서는 method1만 사용하는걸 볼 수 있다.  
하지만 interface의 메소드를 모두 Override해야 하므로 method1을 사용하기 위해 불필요한 method2또한 Override를 해줘야 한다.  
<span style = "color:blue">(위 코드의 경우는 interface의 추상메소드가 두개 뿐이지만 메소드가 많을수록 불필요하게 Override해야할 메소드가 많아지게 되는 것이다.)</span>
<br>
<br>

이를 해결하기 위한 것이 **Adaptor**이다.

### Adaptor
```java
public class MyInterfaceAdaptor implements MyInterface{
    @Override
    public void method1() {
    
    }

    @Override
    public void method2() {

    }
}

class Service extends MyInterfaceAdapter {
    @ovveride
    public void method1() {
        System.out.println("Hello World");
    }
}
```
MyInterface를 Adaptor를 통해 inplements 하여 모든 메소드를 비어있는 상태로 구현한다.  
그 후 Service에서는 implements가 아닌 **extends**, 상속을 하여 method1만 Override하여 사용하는 것이다.  
그러면 이전처럼 불필요한 method2를 구현하지 않아도 된다.  
<br>
하지만 이 또한 문제가 생길 수 있다.  
만약 Service가 다른 object를 상속받았다고 하자. Java에서 extends는 하나만 가능한데 그럼 다시 method1을 쓰기위해 MyInterfaceAdator를 implements받아야 한다. 그렇게 되면 또 똑같이 불필요한 method2도 Override해야 하는 문제가 발생하는 것이다. 
<br>

이를 위한 것이 바로 **default Method**이다.

### default Method
```java
interface MyInterface {
    default void method1(){};
    default void method2(){};
}
class Service extends Object implements MyInterface {
    @ovveride
    public void method1() {
        System.out.println("Hello World");
    }
}
```
interface의 메소드를 default Method로 작성해 주면 이를 implements 받은 class에서 원하는 메소드만 Override해 사용할 수 있다.

- default Method 기능을 정리하자면
  1. Adaptor의 역할을 할 수 있다.  
  2. Override할 때 구현되는 기능이 항상 같다면, default Method로 한번만 구현하면 **Override없이 그저 implements하는 것만으로도 그 기능을 사용할 수있다.**  

## 2-1. Static Method
---
default Method처럼 Static Method도 있다.
```java
public interface Ability {
    static void sayHello() {
        System.out.println("Hello World");
    }
}

public class Main {
    public static void main(String[] args) {
        Ability.sayHello();
    }
}
```
특정 기능을 수행할 수 있는 Static Method를 interface에 담고 이를 호출하는 것으로 사용할 수 있다.  
<span style = "color: blue"> 함수의 역할과 같으므로 함수제공자라고 생각하면 된다.</span>


## 3. Functional Interface
## 4. Lambda 표현식
