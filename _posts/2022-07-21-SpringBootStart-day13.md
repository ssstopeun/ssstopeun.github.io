---
title: Day12.Spring Framework (3)
date: 2022-07-25 15:00:00 +0900
categories: [Backend, SpringBoot]
tags: [SpringBoot, Backend, SW] 
author: author_id 
---

# [Day12] Spring Framework 핵심개념 (3)

## 컴포넌트 스캔
---
> 컴포넌트 스캔이란 스프링이 직접 클래스를 검색해서 Bean으로 등록해주는 기능이다.

설정 클래스, 즉 AppConfiguration에서 Bean으로 따로 등록하지 않아도 원하는 클래스를 빈으로 등록할 수 있다. 그 방법을 알아보자.

### 1. 스트레오타입
이는 번역하면 고정관념이라는 용어인데 UML 다이어그램을 확장시켜주는 도구로서 특정요소를 상황이나 도메인에 맞게 분류해주는 것이다.  
우리가 spring을 사용할때 클래스의 용도에 맞게 @Component, @Controller, @Service, @Configuration 등의 스트레오타입 애노테이션을 사용하여 컴포넌트 스캔을 하도록 할 수 있다. 