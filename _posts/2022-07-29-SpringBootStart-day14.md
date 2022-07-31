---
title: Day14.Spring Framework (4)
date: 2022-07-29 14:00:00 +0900
categories: [Backend, SpringBoot]
tags: [SpringBoot, Backend, SW] 
author: author_id 
---

# [Day14] Spring Framework 핵심개념 (4)

Application 속성관리를 spring에서 어떻게 하는지 중심적으로 학습하려고 한다.

## Environment profile
---

예를 들어 개발시에는 H2 DataBase를 사용하도록 빈이 등록된다. 사용하는 DataSource의 Connection 대상이 H2 DataBase이 된다. 하지만 그 후 운영중에는 mysql을 사용하도록 바뀌게 된다면 DataBase, 즉 환경이 바뀌는 것이다.

### Properties
> 환경의 속성을 저장하는데 이를 외부 속성으로 관리하고 application이 읽을 수 있게 한다. 

## YAML로 property 작성
---

### @ConfigurationProperties
> 스프링부트에서 지원하는 기능으로 별도의 Bean을 만들 수 있게 도와준다. @ConfigurationProperties를 이용하면 특정 그룹의 속성을 모델링할 수 있고 Bean으로 등록해 사용할 수 있다.

## 스프링 Profile
---
> 애플리케이션 설정의 일부를 분리하여 특정 환경에서 사용할 수 있게 한다. 

- 여러 Bean 정의들이 특정 Profile에서만 동작하게 할 수 있다.
- 특정 Property들을 특정 Profile로 정의해서 해당 Profile이 활동 중일때만 적용되게 할 수 있다.

## Resource
> 외부 resource (이미지, 텍스트, 암복호화를 위한 키파일) 들을 Resource, ResourceLoader 인터페이스를 제공해 하나의 API로 제공한다.