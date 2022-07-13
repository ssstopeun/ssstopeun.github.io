---
title: Day4.Collection
date: 2022-07-07 16:26:00 +0900
categories: [Backend, Java for framework]
tags: [Java, Backend, SW] 
author: author_id 
---

# [DAY4] Collection

## Collection
---
> Collection은 **여러 데이터의 묶음**이다. (묶음 단위로 움직인다.)  
 **추상체** 이다.  

![Desktop View](/assets/img/2022.07/07-1.PNG){: width="70%" }

<br>
<br>

## Iterator
---

> 여러 데이터의 묶음(Collection)을 풀어서 하나씩 처리할 수 있는 수단을 제공한다.
- next()를 통해서 다음 데이터를 조회할 수 있다. <span style="color: #dc4343">(이전 데이터는 조회불가)</span>


```java
List<String> list = Arrays.asList("A", "BC", "DEF");
Iterator<String> iter = list.iterator();

while(iter.hasNext()) {
    System.out.println(iter.next());
}
```
이처럼 list를 interator()로 설정하면 iter.hasNext()로 다음 데이터가 있는지 확인하고 iter.next()로 값들을 불러올 수 있다.

## Stream
---

> **데이터의 연속** 이다.
- Java 8 이상에서 부터 사용가능하다.
- 이미 우리가 쓰고 있는 Stream에는 System.in / System.out 가 있다.
- Java 8 : Collection.stream() 을 제공한다.
- filter / map / forEach 등과 같은 고차함수 (함수형 인터페이스를 사용하여 함수를 인자로 받는 함수)를 제공한다.

```java
public class Main {
    public static void main(String[] args) {
        Arrays.asList("A", "AB", "ABC", "ABCD", "ABCDE")
            .stream()
            .map(s -> s.length()) //.map(String::length)
            .filter(i -> i % 2 == 1)
            .forEach(i -> System.out.println(i));   
            //.forEach(System.out::println)
    }
}
```
다음과 같이 Stream을 사용할 수 있다. 이 밖에도 count, reduce 등 다양한 고차함수로 기능들을 제공한다.  
(주석은 method reference로 바꾼 코드로 같은 의미이다.)

<br>

- 스트림을 만들때는 Stream.generate / Stream.iterate 로 만들 수 있다.  

이를 사용해 random한 숫자를 10개 생성하는 코드예시이다.
```java
//Stream.generate 사용예시
Random r = new Random();
Stream.generate(r::nextInt)         //Stream.generate(() -> r.nextInt())
    .limit(10)
    .forEach(System.out::println)

//Stream.iterate 사용예시
Stream.iterate(seed:0, (i) -> i + 1)
    .limit(10)
    .forEach(System.out::println)
```

다른 예시도 알아보자.
주사위를 100번 던져 6이 나오는 확률를 구해보자.
```java
Random r = new Random();
var count : long = Stream.generate(() -> r.nextInt(bound: 6) + 1) 
    .limit(100)
    .filter(n -> n === 6)
    .count();

System.out.println(count);
```


- 스트림의 장점 : 연속된 데이터를 위에서 말한 고차함수들을 사용해 기능들을 간결하게 표현할 수 있다.  

**<span style = "background-color: #fff5b1">익숙해지면 굉장히 편리하니 자주 사용하여 익숙해지자!</span>**

## Optional
---

- NPE : Null Pointer Exception  
-> 가장 많이 발생하는 에러중 하나  
-> 자바에서 거의 모든 것이 레퍼런스기 때문에 **거의 모든것이 null이 될 수 있다.**  
-> 항상 null을 확인해야 한다!

- <span style="color: #dc4343">그래서 null을 쓰지말자고 서로 약속한다. (계약한다.)</span>

어떻게하면 null을 쓰지않을 수 있을까? 그 방법을 알아보자.

---

- EMPTY 객체 사용  

```java
//EMPTY 정의
public static final User EMPTY = new User(age:0, name:"");

//초기값 설정
User user = User.EMPTY;

//사용예시
if(user == User.EMPTY){ 

}
```
이렇게 null을 사용하지 않고 pusblic static final로 EMPTY 초기값을 설정해준다.

<br>

- Opitonal 사용  
    - <span style = "color: #0000CD">Optinal이란?</span> 
    **null를 포함한 객체를 이동시켜주는 바구니** 로 하나의 type이다.
    - 객체를 담아 이동시켜주는데 객체가 null일때 null을 보여주는 것이 아니라 아무것도 없는 바구니를 보여주는 것이다. 객체가 null이 아니면 바구니 속 data를 보여준다.
    - 또한 간단한 기능들도 제공한다.  

```java
Optional<User> optionalUser = Optinal.empty();
OptionalUser = Optional.of(new User(age:1, name:"2"));

optionalUser.isEmpty();   //null이면 true
optionalUser.isPresent(); //값이 있으면 true

if (optionalUser.isPresent()) {
    //do1 
} else{
    //do2
}
// 위와 같은 if문을 Optional에서 제공하는 기능으로 사용할 수 있다.

optionalUser.ifPresentOrElse(user -> {  //user라는 객체가 존재
    //do 1
}, () ->{   //() : null값
    //do 2
})

```

## 정리
---

- Collection : 여러 데이터의 묶음  
- iterator : 데이터를 개별로 처리
- stream : 데이터의 연속으로 데이터들을 다양한 고차함수들로 간결하게 표현 가능
- Optional : 자바에서 많이 발생하는 에러인 NPE를 방지하기위해 개발자들간의 ***"null을 사용하지 말자"*** 는 암묵적인 rule이 생겼고 이를 위해 사용하는 type  
-> 프로그램을 더 안전하게 만들 수 있게 되었다.
