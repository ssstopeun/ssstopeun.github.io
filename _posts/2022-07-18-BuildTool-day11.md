---
title: Day11-1.Build Tool
date: 2022-07-18 13:28:00 +0900
categories: [Backend, SpringBoot]
tags: [SpringBoot, Backend, SW, Gradle, Maven] 
author: author_id 
---

# [Day11-1] Build Tool

## Build
---

- 필요한 라이브러리를 다운 받고 classpath에 추가한다.
- 소스 코드를 컴파일 한다.
- 테스트를 실행한다.
- 컴파일된 코드를 packaging한다. -> jar / war / zip etc
- packaging된 파일을 주로 artifacts 라고 부르고 server나 repository에 배포한다.

<br>

Build Tool에는 대표적으로 **Maven, Gradle**이 있다.

## Maven
---
> 빌드 도구로써 주로 **자바기반의 프로젝트**에서 많이 사용한다.
> XML 기반으로 설정모델을 제공하고 pom.xml 파일로 작성할 수 있다. 

*POM은 project object model의 약어*

- Maven을 사용하는 이유  
  1. archetypes라는 프로젝트 템플릿을 제공하여 반복되는 설정을 도와준다.
  2. 프로젝트에 사용하는 외부 라이브러리인 dependency를 관리해준다.
  3. 플러인과 외부 라이브러리를 분리하여 관리한다.
  4. dependency를 다운받는 Repository가 로컬이 될 수도 있고 Maven Central와 같은 공개된 Repository가 될 수도 있다.

<br>

### Maven 실습

Intellij에서 프로젝트를 생성하면
![Desktop Vies](/assets/img/2022.07/18-2.JPG){: width="30%" }  
이렇게 Advenced Settings을 할 수있는데 이 부분을 **Maven coordinates**라고 한다.
<br>

![Desktop View](/assets/img/2022.07/18-1.JPG){: width="70%" }  
설정을 하고 나면 pom.xml에 나타나게 되는데 Maven coordinates는 프로젝트를 식별하는데 사용된다.
- **groupId** : 주로 회사, 단체명을 작성
- **artifactId** : 주로 프로젝트명을 작성
- **version** : 프로젝트의 버전 작성

그리고 코드를 보면 < modules > 라는 부분이 있는데 이는 Maven의 Multiple Module 기능이다.
> Multiple Module  
> : 하나의 프로젝트에 여러 프로젝트를 관리할 수 있다.

<br>

### Transitive Dependencies
> Maven의 특이점 중 하나로 **의존성의 의존성**을 뜻한다.

예를 들어 a가 b를 참조하고 b가 c를 참조할 때 a는 c를 transitive 의존성으로 간주하는 것이다. 이렇게 되면 의존성 트리가 구성되고 동일한 groupId, artifactId에 대해서 가장 최신의 version을 가져와 사용한다. 좋은 기능같아 보이지만 <span style = "color:red">버전, 라이브러리 간의 충돌 등의 문제가 야기될 수 있다는 단점이 있다.</span> 

## Gradle
---
> Groovy / 코틀린 기반으로 스크립트를 작성하게 도와 주는 Build Tool

<br>

Gradle도 intellij서 사용해보면 다음과 같이 build.gradle.kts를 확인할 수 있다.
![Desktop](/assets/img/2022.07/18-3.JPG){: width:70%}

<br>

### Project & Task  
  - Gradle은 하나 이상의 프로젝트를 지원한다. 이는 마치 Maven의 Multiple Module과 비슷하다. 그리고 하나의 프로젝트는 하나 이상의 Task로 구성된다. Task는 클래스를 컴파일하거나 Jar를 생성하거나 하는 등의 build를 위한 작업이다.  
  일반적으로 Task는 Plugin에 의해서 제공된다.

### Plugin  
  > Gradle의 실제 Task와 주요한 기능들을 추가하게 해주는 것이다.
- 하나의 플로젝트의 여러 플러그인을 추가할 수 있다.
- 플러그인을 추가하면 새로운 Task들이 추가되고 도메인 객체나 특정 컨벤션들이 추가된다.

## Maven VS Gradle
---

- Build Tool을 처음 접할 때는 Gradle을 공부해서 사용하는 것을 추천한다.  
- 회사에서 Maven을 사용한다면 Maven을 학습하자.
- Maven은 XML 설정이 장황하지만 별다른 학습없이 직관적으로 사용할 수 있기 때문에 간단한 프로젝트를 만들때에도 Maven을 권장한다.