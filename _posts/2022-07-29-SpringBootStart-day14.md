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
> 환경의 속성을 저장하는데 이를 외부 속성으로 관리하고 application이 읽을 수 있게 한다. 다양하게 저장될 수 있는데 