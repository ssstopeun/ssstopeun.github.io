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
> Properties는 환경의 속성을 저장하는 것으로 이를 외부 속성으로 관리하고 application이 읽을 수 있게 한다. 

```java
//application.properties
version = v1.0.0
kdt.version = v1.0.0
```

이렇게 properties를 정의한 후 OrderProperties에서 이 version을 불러와 확인해 보자.   
test class에서 bean을 가져와 불러오는 것이 아니라 InitializingBean을 사용해 class에서 바로 확인할 수 있다.

```java
@Component
public class OrderProperties implements InitializingBean {
    @Value("${kdt.version}") // ${kdt.version:v0.0.0} 으로 kdt.version이 없을때 default값을 줄 수 있다.
    private String version;

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println(MessageFormat.format("[OrderProperties] version -> {0}", version));
    }
}
```


## YAML로 property 작성
---
> YAML의 정의는 "또 다른 마크업 언어 (Yet Another Markup Language)" 에서 유래되었다.

```java
//application.properties
kdt.version = v1.0.0

kdt.minimum-order-amount = 1

//application.yaml
kdt:
  version: "v1.0"
  minimum-order-amount: 1

```

properties에서 설정한 것을 yaml으로 바꾸면 이런 식으로 정의할 수 있다.

### @ConfigurationProperties
> 스프링부트에서 지원하는 기능으로 별도의 Bean을 만들 수 있게 도와준다. @ConfigurationProperties를 이용하면 특정 그룹의 속성을 모델링할 수 있고 Bean으로 등록해 사용할 수 있다.

- property들을 하나로 그룹화, 타입화를 시켜 다른 class들이 주입받아서 쓸 수 있도록 한다.

## 스프링 Profile
---
> 애플리케이션 설정의 일부를 분리하여 특정 환경에서 사용할 수 있게 한다. 

- 여러 Bean 정의들이 특정 Profile에서만 동작하게 할 수 있다.
- 특정 Property들을 특정 Profile로 정의해서 해당 Profile이 활동 중일때만 적용되게 할 수 있다.

## Resource
> 외부 resource (이미지, 텍스트, 암복호화를 위한 키파일) 들을 Resource, ResourceLoader 인터페이스를 제공해 하나의 API로 제공한다.

- 텍스트 파일 읽어오기
```java
var resource2 = applicationContext.getResource("file:sample.txt");
var strings = Files.readAllLines(resource2.getFile().toPath());
System.out.println(strings.stream().reduce("",(a, b)-> a + "\n" + b));
```

- url로 접근하여 읽어오기
```java
var resource3 = applicationContext.getResource("https://stackoverflow.com/");
var readableByteChannel = Channels.newChannel(resource3.getURL().openStream());
var bufferedReader = new BufferedReader(Channels.newReader(readableByteChannel, StandardCharsets.UTF_8));
var contents = bufferedReader.lines().collect(Collectors.joining("\n"));
System.out.println(contents);
```

이와 같이 텍스트든 url접근이든 applicationContext.getResource라는 일관된 방법으로 읽어와 처리할 수 있다.