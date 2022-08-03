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

- 테스트 더블  
의존 구성요소를 사용할 수 없을때 테스트 대상 코드와 상호작용하는 객체이다.  
SUT속 객체가 직접테스트가 안될때 가짜 객체를 만들어 테스트를 하는 것이다.

![Desktop View](/assets/img/2022.08/03-1.PNG){: width="70%" }
Unit Test는 이러한 구조를 띄는데 예를 들어서 계산기 프로그램이 있다고 가정했을때 SUT는 이 계산기 프로그램이 되고 **Method** 는 덧셈, 뺄셈, 곱셈, 나눗셈 등의 기능이 된다.   
숫자, 계산식을 GIVEN으로 주고 전제조건, WHEN에 따라 Method들이 호출되고 THEN으로 출력값이 나와 이를 테스트할 수 있는 것이다.  
<br>
이렇게되면 새로운 기능이 추가가 되었을때도 앞선 기능들에 대한 확신이 되는 것이다.  
또한 코드들을 일일이 살펴보지않고 테스트명세만 봐도 이 프로그램에 대한 신뢰가 생긴다.