---
title: Collection
date: 2022-07-07 04:26:00 +0900
categories: [Backend, Java for framework]
tags: [Java, Backend, SW] 
author: author_id 
---

# [DAY4] Collection

## Collection
---
- Collection은 **여러 데이터의 묶음**이다. (묶음 단위로 움직인다.)
- **추상체** 이다.
![Desktop View](./assets/img/2022.07/07-1.png){: width="70%" }

<br>
<br>

## Iterator
- 여러 데이터의 묶음(Collection)을 풀어서 하나씩 처리할 수 있는 수단을 제공한다.
- next()를 통해서 다음 데이터를 조회할 수 있다. <span style="color: #dc4343">(이전 데이터는 조회불가)<span>


```java
List<String> list = Arrays.asList("A", "BC", "DEF");
Iterator<String> iter = list.iterator();

while(iter.hasNext()) {
    System.out.println(iter.next());
}
```
이처럼 list를 interator()로 설정하면 iter.hasNext()로 다음 데이터가 있는지 확인하고 iter.next()로 값들을 불러올 수 있다.

## Stream
- **데이터의 연속** 이다.
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

---
---
- 스트림을 만들때는 Stream.generate / Stream.iterate 로 만들 수 있다.
- 스트림의 장점 : 연속된 데이터를 위에서 말한 고차함수들을 사용해 기능들을 간결하게 표현할 수 있다.

**익숙해지면 굉장피 편리하니 자주 사용하여 익숙해지자!**

## Optional
- NPE : Null Pointer Exception -> 가장 많이 발생하는 에러중 하나