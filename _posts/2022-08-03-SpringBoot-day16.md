---
title: Day15.SpringTest
date: 2022-08-03 18:00:00 +0900
categories: [Backend, SpringBoot]
tags: [SpringBoot, Backend, SW, SpringTest] 
author: author_id 
---

# [Day16] SpringTest

## 1. 소프트웨어 테스팅
---
> 주요 이해관계자들에게 시험대상의 품질에 관한 정보를 제공하는 조사과정이다. 소프트웨어의 결함이 있는지 찾는 과정이라고 생각할 수 있다.

이 테스팅에는 여러 종류의 testing level이 존재하는데 일반적으로 backend Engineer의 경우 단위테스트 (unit test)를 거의 무조건 작성하게 되고 통합테스트도 일부 작성한고 한다.

### Unit Test : 단위테스트
> 보통 모든 class에 대해 한 class당 한 test를 작성하여 단위테스트를 진행한다.

- SUT(System under Test)  
테스트하는 대상으로 단위테스트의 단위 부분이다. 보통 class들이 SUT가 되고 이 안에는 method, property, constructor들이 있고 주로 method를 테스트하게 된다. class속 기능들을 테스트한다고 보면 된다.

![Desktop View](/assets/img/2022.08/03-1.PNG){: width="70%" }
Unit Test는 이러한 구조를 띄는데 예를 들어서 계산기 프로그램이 있다고 가정했을때 SUT는 이 계산기 프로그램이 되고 **Method** 는 덧셈, 뺄셈, 곱셈, 나눗셈 등의 기능이 된다.   
숫자, 계산식을 GIVEN으로 주고 전제조건, WHEN에 따라 Method들이 호출되고 THEN으로 출력값이 나와 이를 테스트할 수 있는 것이다.  
<br>

이렇게되면 새로운 기능이 추가가 되었을때도 앞선 기능들에 대한 확신이 되는 것이다.  
또한 코드들을 일일이 살펴보지않고 테스트명세만 봐도 이 프로그램에 대한 신뢰가 생긴다.

### Itegration Test : 통합테스트
> 통합테스트는 내 코드가 다른 의존관계와 연동이 잘되는지 테스트하는 것이다.

## JUnit
---
- JUnit의 기능
    1. 매 단위 테스트시마다 테스트 클래스의 인스턴스가 생성되어 독립적인 테스트가 가능하다.
    2. 애노테이션을 제공해서 테스트 라이프 사이클을 관리하게 해주고 테스트 코드를 간결하게 작성하도록 지원한다.
    3. 테스트 러너를 제공하여 Intellij / eclipse / maven 등에서 테스트 코드를 쉽게 실행하게 해준다.
    4. assert로 테스트 케이스의 수행 결과를 판별하게 해준다. -> assertEquals(예상값, 실제값)
    5. 결과는 성공(녹색), 실패(붉은색) 으로 표시해준다.

project.test.java에서 test를 작성할 수 있는데 test하고 싶은 class에서 **ctrl+Insert**로 test class를 바로 생성할 수 있다. 그렇다면 test를 작성하는 방식을 알아보자.
<br>

import static org.junit.jupiter.api.Assertions.* 나 import static org.hamcrest.Matchers.* 을 사용할 수 있는데 개인적으로 후자가 더 좋은 것 같아 간단히 코드를 소개하고자 한다.

```java
@Test
@DisplayName("여러 hamcrest matcher 테스트")
void hamcrestTest(){
    assertEquals(2, 1+1);
    assertThat(1+1, equalTo(2));            //actual이 2와 같다.
    assertThat(1+1, is(2));                 //actual이 2다.
    assertThat(1+1, anyOf(is(1), is(2)));   //acture이 1이거나 2다.

    assertNotEquals(1,1+1);
    assertThat(1+1,not(equalTo(1)));         //actual은 1과 다르다.
}
```
이런식으로 작성할 수 있는데 assertEquals / assertNotEquals가 jupiter api이고 그 밑에 assertThat이 동일한 의미의 hamcrest 이다. jupiter api보다 훨씬 코드의 의미를 직관적으로 파악할 수 있는 것 같다.
<br>

또한 hamcrest는 list와 같은 요소를 test할 때 효과적이다.
```java
void hamcrestListMatcherTest(){
    var prices = List.of(1,2,3);
    assertThat(prices,hasSize(3));                  // List size가 3이다.
    assertThat(prices, everyItem(greaterThan(0)));  //List요소가 모두 0보다 크다.
    assertThat(prices, containsInAnyOrder(2,3,1));  //순서상관없이 2,3,1이 있다.
    assertThat(prices,contains(1,2,3));             // 1,2,3 순서로 list가 있다.
    assertThat(prices,hasItem(2));                  //List요소 중 2가 있다.
}
```
보는 것과 같이 list의 size나 요소들을 test하기에 굉장히 편리하다.

## 테스트 더블  
> 의존 구성요소를 사용할 수 없을때 테스트 대상 코드와 상호작용하는 객체이다.  
SUT속 객체가 직접테스트가 안될때 가짜 객체를 만들어 테스트를 하는 것이다.  

- 테스트 더블은 Mock(mock, spy)와 Stub(stub, dummy, fake)로 나뉜다.
- 쉽게 말해 Mock은 행위검증, Stub은 상태검증이라고 한다.

## Mock Object : 모의 객체
---
> 단위테스트의 테스트더블을 설명했었는데 이 테스트더블의 대상코드인 SUT와 상호작용하기 위한 것이 바로 이 Mock Object이다. Stub이 가짜 객체라면 Mock은 호출에 대한 반응을 하는지 알아보는 객체이다.