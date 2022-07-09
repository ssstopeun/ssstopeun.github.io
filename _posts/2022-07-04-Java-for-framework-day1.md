---
title: Day1.Java
date: 2022-07-04 02:00:00 +0900
categories: [Backend, Java for framework]
tags: [Java, Backend, SW, Gradle] 
author: author_id 
---

# [Day1] JAVA

## JAVA 개발환경
---

>JDK (Java Development Kit)  
JVM (Java Virtual Machine) 필요 => 실행환경 : JRE (Java Runtime Environment)  
JRE + 개발 tool => 개발환경 : JDK  

- Oracle JDK : <https://www.oracle.com/java/technologies/downloads/>
특정 라이센스를 이용하기 위해서는 Oracle라이센스를 구매해야 한다.

- Open JDK : <https://openjdk.org/>
무료 오픈 소스이다.

- 다운을 받은 후 path설정 등의 과정을 거쳐야 사용할 수 있고 cmd창에 **java --version**을 통해 잘 설정이 되었는지 확인할 수 있다.

- VSCode EXTENSION에서 **java extension pack** 설치하기

## Build Tool
---

> 자동으로 Build, 실행해주는 tool  
ex. Ant, Maven, **Gradle**

- Gradle : <https://gradle.org>
- Gradle 사용  
    1. cmd창에서 build할 폴더로 이동
    2. **gradle init** 으로 프로젝트 생성
    3. **gradle tasks** 로 데스크 목록 확인
    4. **gradle run** 으로 실행 

## IDE
---

> 통합개발환경  
 ex. Eclipse, **Intellij**
 
 - Intellij : <http://www.jetbrains.com/ko-kr/idea>
 - psvm / sout 등의 키워드로 빠른 코딩이 가능하다. -> 생산성 향상  

 - 유용한 단축키
    - jetbrain에서 제공하는 단축키 정리 pdf   
    <https://resources.jetbrains.com/storage/products/intellij-idea/docs/IntelliJIDEA_ReferenceCard.pdf>

    - 대표적 단축키 

    |단축키|설명|  
    |:--------------------|:--------------------|  
    |**Alt+Enter**| 빠른 수정 |  
    |**Ctrl+1**| 폴더창으로 커서 이동 (<-> Esc) |
    |**Ctrl+N**| 새파일 생성 |
    |**Shift+Shift**| 폴더창에서 파일탐색 |
    |**Alt+Up/Down***| 단계별 블록지정 |
    |**Ctrl+/**| 주석지정 | 
    |**ctrl+Alt+L**| 코드 reformating |
    |**Shift+Ctrl+Alt+T**| refactoring <span style="color: #0000CD">(Intellij의 특징적인 기능)</span> |
    |**<span style ="color: #dc4343">Shift+Ctrl+A : 명령어 검색 </span>**| 명령어검색기능 (다른 기능기억못해도 이것만 기억하면 검색하여 사용가능!) |
    
<br>


## 정리
- JAVA 개발환경을 위해 jdk를 다운받아 나의 개발환경을 꾸미자.
- 자동으로 빌드와 실행을 해주는 Build tool 중 하나인 Gradle을 사용해보자.
- 자바개발자들이 주로 사용하는 IDE인 intellij와 이 환경에서 사용하는 단축키를 알아보자.
- 단축키는 Shift+Ctrl+A로 검색이 가능하고 Intellij Cheat Sheet에 모든 단축키가 나와있다.