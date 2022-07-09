---
title: Day1.Special
date: 2022-07-04 04:30:00 +0900
categories: [Backend, Java for framework]
tags: [Java, Backend, SW, SW Developer] 
author: author_id 
---

# [Day1.Special] 초보개발자가 알아야 할 것

## 1. Coding Convention

---

> 팀이나 회사와 같은 개발 그룹에서 정해서 사용한다.  
정하지 않을 경우 => 일반적인 자바 코딩 룰을 따른다.

### (1) **클래스명은 대문자**로 시작한다.
```java
class myClass {}    // (x)
class My_Class {}   // (x)
class MyClass {}    // (o)
```

### (2) **메소드나 변수명은 소문자**로 시작한다.
```java
int my_variable = 0;    //(x)
int myVariable = 0;     //(o)

void GoHome() {}    //(x)
void goHome() {}    //(o)
```

### (3) **Indent : 들여쓰기**
- Tab or Space <span style = "color: #dc4343">(섞어서 쓰기 금지!)</span>
- VSCode에서는 Tab = Space * 4 이지만 이는 시스템에 따라 다르기 때문에 한가지만 쓰는 것이 중요하다.

## 2. Reference
---


### (Q1) ***Java에서는 alloc / free 를 개발자가 일일히 신경쓰지 않아도 된다.***  <span style = "color: #dc4343">O</span>
  
### (Q2)  ***Java를 하면 포인터를 몰라도 될까?***  <span style = "color: #dc4343">X</span>  
> Java에는 포인터대신 **레퍼런스** 라는 개념이 있기 때문에 이 개념을 정확히 알아야 한다.

### Reference란?
  - java에서는 모든것이 레퍼런스 값이다.
  - 정확하게 말하면 primitive한 8개를 제외한 것이 레퍼런스이다.
  - primitive : boolean, byte, int short, long, float, double, char  
  - array 는 reference처럼 취급한다. (Int [] 도 reference취급)
  - Call by value / Call by reference
  
---

<br>
코드를 보며 reference에 대해 이해해보자.

```java
public class ByeWorld{
    public static void main(String[] args){
    ByeWorld b = new ByeWorld();

    int a = 100;
    b.doSomething(a);

    System.out.println(a);  //main a에는 변화가 없다. a = 100
    }

    private void doSomething(int a) {   //doSomething의 a와 main의 a는 다른 곳에 저장되어있다.
        a *= 2;     //doSomething의 a = 200
    }
}
```
- Call by value  
다음 코드에서는 doSomething을 호출해 a의 값을 2배로 늘렸다고 생각하겠지만 main의 a와 doSomething의 a의 메모리가 다르기 때문에 결국에는 main의 a에는 변화가 생기지 않는다.  

<br>
그렇다면 a가 같은 메모리를 참조해 a의 값이 2배로 늘게 하려면 어떻게 해야할까?

```java
class Int {
    int a = 100;    //Int 라는 object생성
}
public class ByeWorld{
    public static void main(String[] args){
    ByeWorld b = new ByeWorld();

    int a = new Int();      //a는 Int를 가르킨다.
    b.doSomething(a);

    System.out.println(a.a);  
    }

    private void doSomething(Int a) {   //Int를 가르킨다.
        a *= 2;     
    }
}
```
- Call by reference  
이렇게 Int라는 object를 생성하여 사용하게 되면 main의 a와 doSomething의 a의 메모리는 다르겠지만 두 메모리가 가르키는 곳은 Int로 같다. 따라서 같은 data를 참조하고 변화시키기 때문에 a의 값이 200으로 나오게 되는 것이다.
<br>

## 3. Contant Pool
---

> Java에서는 Constant Pool을 이용해 String을 특별취급한다.
> 따라서 String 객체는 한번 값이 할당되면 그 공간이 변하지 않는다. **(불변성)**

예시코드를 보자.

```java
String a = "";
for (int i = 0; i < 10; i++){
    a += 1;
}
System.out.println(a);
```
결과는 0123456789다.  
a = ""가 저장된 공간에서 "0", "01", "012" ... 이런식으로 값만 계속 바뀔 것이라고 생각한다.  
하지만 Java에서는 이를 Constant Pool공간에 "", "0", "01" ... 총 11개의 값이 모두 따로 저장된다.  
=> 메모리사용에 있어 비효율적이다.  

```java
String a = "Hello World";
String b = "Hello World";
System.out.println(a == b); //reference가 같은지 비교
```
그럼 이경우는 어떨까? a = "Hello World" 따로 b = "Hello World" 따로 저장되었을 것이라고 생각한다.
하지만 java에서는 우선 Constant Pool에 "Hello World"라는 글자를 만들고 a가 이를 가르킨다. b는 Constant Pool에 똑같은 것을 찾게 되면 그 메모리를 참조하여 사용한다.  
=> 메모리를 효율적으로 사용할 수 있다.

<br>

***첫번째 코드의 비효율성을 어떻게 해결할 수 있을까?***
> 조합을 다한 후 한번만 Constant Pool에 저장하자.  
> => StringBuffer를 사용
> StringBuffer는 한번값이 할당되더라도 다른 값이 할당되면  공간이 변한다. **(가변성)**

- StringBuffer
```java
StringBuffer sb = new StringBuffer();
for (int i = 0; i < 10; i++){
    sb.append(i);
}
System.out.println(sb);
```
결과는 위와 동일하다. 하지만 Constant Pool에는 "0123456789" 라는 값만 저장되어 메모리를 효율적으로 사용할 수 있다.

<br>

비슷한 기능으로 StringBuilder도 있는데 StringBuffer와의 차이를 살펴보자.
##
### StringBuffer VS StringBuilder
>값이 변경되었을때 **같은 주소공간을 참조** 하고 **값이 변경되는 가변성**을 띈다는 점에서는 같다.

- 차이점은 바로 <span style = "color: #dc4343">**동기화 (Synchronization)**</span>이다.
    - StringBuilder는 동기화를 지원하지 않고 StringBuffer는 동기화를 지원한다.
    - 동기화를 지원한다는 것은 멀티 스레드 환경에서도 안전하게 동작할 수 있다는 말이다.
    - StringBuffer는 동기화를 위해 **synchronized** 키워드를 사용한다.
    - **synchronized** : 여러개의 스레드가 한 개의 자원에 접근하려고 할 때, 현재 사용하고 있는 스레드를 제외한 나머지 스레드들이 데이터에 접근할 수 없도록 막는 역할을 한다.
    <br>

    ```
    멀티 스레드 환경에서 A, B 스레드가 StringBuffer의 append()메소드를 사용하고자 한다고 해보자.  
      
    A 스레드 : sb의 append() 동기화 블록에 접근 및 실행  
    B 스레드 : A 스레드 sb 의 append() 동기화 블록에 들어가지 못하고 block 상태가 됨.  
    A 스레드 : sb의 append() 동기화 블록에서 탈출  
    B 스레드 : block 에서 running 상태가 되며 sb 의 append() 동기화 블록에 접근 및 실행.  

    ```

    - 하지만 속도측면에서는 StringBuilder가 더 빠르다.
  <br>
- 정리하자면
  - 변하지 않는 문자열을 자주 사용할 경우  
    **=> String**  
  - 단일 스레드 환경이고 문자열이 계속 변할 경우  
    **=> StringBuilder**
  - 멀티 스레드 환경이고 문자열이 계속 변할 경우  
    **=> StringBuffer**

<br>

## 4. Object
---

> **모든 객체의 최상위 객체** 이다.
> 모든 객체에는 Object의 메소드를 호출할 수 있다.
> => Object의 메소드 종류, 기능을 명확히 알아야 한다.

- 대표적인 메소드
  - toString()
  - equals()
  - hashCode()

<br>

## 5. Git
- git은 기본!
- 명령어로 익힐 필요는 없지만 어떻게 사용하는지는 알아야 한다.
  - ex. branch는 어떻게 따고, branch가 merge되면 어떻게 되고, 충돌이 나면 어떻게 해야하고 등등
- git tool 사용
  - GitHub Desktop
  - Sourcetree
  - intellij Extension -> Git graph
- <span style = "color: #dc4343">.gitignore을 잘 활용하자!</span>
  - gitignore : 포함되지 않아도 되는 파일들을 관리할때 사용
  - build결과 (*.class, *.jar, build/), generate가능한 파일, local설정, 키/보안관련
- .gitignore에 어떤 파일을 넣어햐 하나요?
  - https://gitignore.io 를 통해 본인의 작업파일에서의 필요한 .gitignore파일 내용 생성이 가능하다.

## 정리
- 자바 개발자라면 기본적으로 지켜야할 것이 있다.
- 자바 개발을 위해서는 Reference의 개념을 명확히 알고 사용할 줄 알아야 한다.
- Call by value와 Call by reference의 차이를 알아야 한다.
- String, StringBuffer, StringBuilder의 차이를 알고 상황에 맞춰 사용하자.
- Object는 모든 객체의 최상위 객체로서 알고있으면 활용범위가 넓어지므로 Object의 메소드와 기능들에 대해 학습해야한다.
- git은 개발자의 기본이므로 사용법을 잘 익혀 활용할 줄 아는 것이 중요하다.
- git를 센스있게 사용하기 위해서는 .gitignore을 잘 사용해야 한다.