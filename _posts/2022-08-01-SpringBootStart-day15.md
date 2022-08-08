---
title: Day15.Spring Framework (5)
date: 2022-08-01 16:00:00 +0900
categories: [Backend, SpringBoot]
tags: [SpringBoot, Backend, SW] 
author: author_id 
---

# [Day15] Spring Framework 핵심개념 (5)

## Logging
> 시스템을 작동할 때 시스템의 작동 상태의 기록과 보존, 이용자의 습성 조사 및 시스템 동작의 분석 등을 하기 위해 작동중의 각종 정보를 기록해야하는데 이 기록을 만드는 것이 로깅이다.  
> **로그 시스템의 사용에 관계된 인련의 [사건]을 시간의 겨과에 따라 기록하는 것이다.**

- 버그가 발생했을때 되돌려 보는 용도로 사용하는 것이 대표적이다.
- 실습때 사용하던 println/print를 사용하지 않고 이 log를 사용한다.

### Java Loggin Framework
- java.util.logging
- Apache Commons logging
- **Log4J**
- **Logback** 
- SLF4J (Simple Logging Facade for Java)

<br>

이 Loggin Framework중에 Log4J, Logback을 주로 사용했었는데 두개를 병행해서 쓰고 싶어 나온 것이 **SLF4J** 이다.

### SLF4J
> Logging Framework들을 추상화 해놓은 것이다. Facade Pattern을 이용한 Logging Framework이다.

- Facade 패턴 : 많은 서브시스템을 거대한 클래스로 만들어 감싸서 편리한 인터페이스를 제공하는 것이다.

SLF4J는 새로운  Loggin Framework이 나올때마다 Log 코드를 수정해야 하는 불편함을 없애준다. SLF4J는 **Binding모듈**과 설정만 바꿔주면 된다는 장점이 있다.

### 로깅 프레임워크의 Binding 모듈
> SLF4J가 다양한 로깅 프레임워크를 지원하는데 이는 바인딩 모듈을 통해서 처리된다. 바인딩 모듈은 로깅 프레임워크를 연결하는 역할을 한다.

- Log Level
  1. trace
  2. debug
  3. info
  4. warn
  5. error

**보통 trace나 debug는 배포과정에서가 아닌 개발과정에서 주로 사용한다.**
**주로 info를 base로 자주 사용한다.**
<br>

그렇다면 이 log를 어떻게 사용하는지 알아보자.

```java
private static final Logger log = LoggerFactory.getLogger("org.prgrms.kdt.OrderTester");

private static final Logger log = LoggerFactory.getLogger(OrderTester.class);
```
static으로 사용하여 logger는 단 하나 라는 것을 의미해주고 class의 이름 전체를 가져와야 한다.
<br>

```java
//org.prgrms.kdt  // SET WARN

//org.prgrms.kdt.A  => WARN

//org.prgrms.kdt.voucher  => WARN => SET INFO => INFO
```

"."을 기준으로 set이 되기때문에 ~.kdt에서 WARN을 설정하면 하위 클래스들은 자동으로 WARN으로 설정이 된다. 따라서 ~.kdt.voucher는 INFO로 설정하고 싶다고 하면 ~.voucher를 Info로 다시 set해줘야 한다.

<br>

```java
logger.info("logger name => {}",logger.getName());

logger.info(MessageFormat.format("logger name => {0}", logger.getName()));
```

또한 log를 사용할때는 편리한 점이 MessageFormat을 사용하지 않아도 "{}"를 순서대로 인식해 값을 넣을 수 있다.

<br>

이 처럼 springboot는 설정없이 log를 사용할 수 있다. 그럼 직접 logback설정파일을 넣어 이 설정에 의해 log가 출력되도록 해보자.

## Logger
---

### logback 설정하기

1. logback-test.xml 파일을 먼저 찾습니다.
2. 없다면 logback.groovy 을 찾습니다.
3. 그래도 없다면 logback.xml을 찾습니다.
4. 모두 없다면 기본 설정 전략을 따릅니다. BasicConfiguration

<br>

우선 본 실습파일에는 1,2,3 이 해당이 안되기에 main.resources에 logback.xml을 추가한 후 <https://logback.qos.ch/manual/configuration.html> 여기서 **Basic Configuration file** 을 가져온다.

- Basic Configuration file

```html
<configuration>
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <!-- encoders are assigned the type
         ch.qos.logback.classic.encoder.PatternLayoutEncoder by default -->
    <encoder>
      <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
  </appender>

  <root level="debug">
    <appender-ref ref="STDOUT" />
  </root>
</configuration>
```

이를 입맛대로 변경해주면 되는데

```html
<!-- logback.xml -->
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <!-- encoders are assigned the type
             ch.qos.logback.classic.encoder.PatternLayoutEncoder by default -->
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <logger name = "org.prgrms.kdt" level = "debug" />

    <root level="warn">
        <appender-ref ref="STDOUT" />
    </root>
</configuration>
```

이때 **root level = "..."** 는 전체 class에 대한 log의 level을 설정해주게 된다. **root level="warn"** 이라 설정했으니 warn이상의 log는 모두 나오는 것이다.  
  
하지만 org.prgrms.kdt 하위 class는 debug이상으로 나오게 하고싶다면?  
**logger name = "org.prgrms.kdt" level = "debug"** 를 추가해 설정해 주어야한다. 이것이 바로 위에서 말했던 "."기준으로 log level이 설정된다고 했던 부분을 적용한 것이다.

<br>

logback.xml의 messageformat을 작성하는 부분도 살펴보자.

```html
<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
```

- {HH:mm:ss.SSS}  : log가 남겨진 시간
- [%thread]       : DEBUG, INFO 와 같은 thread 이름      
- %-5level        : -n 이면 왼쪽정렬에 n글자 / n 이면 오른쪽정렬에 n글자
- %logger{36}     : org.prgrms.kdt.OrderTester 을 출력할때의 글자수를 의미하는데 logger{6}이라면 o.p.k.OrderTester 이런 식으로 프로그램내에서 글자수에 맞춰 줄여서 출력해준다.
- %msg%n          : log message



## 로그 Appender 설정
---

1. ConsoleAppender
2. FileAppender
3. RollingFileAppender

앞서 사용했던 방식들이 ConsoleAppender 방식인데 이보다 FILEAppender를 사용해 file로써 log를 남기는 것이 더 좋다.

### FileAppender

```html
<timestamp key="bySecond" datePattern="yyyyMMdd'T'HHmmss" />

<appender name="FILE" class="ch.qos.logback.core.FileAppender">
    <file>log/kdt_${bySecond}.log</file>
    <append>false</append>
    <encoder>
        <pattern>${LOG_PATTERN}</pattern>
    </encoder>
</appender>
```

이런식으로 하면 log가 생성될때 마다 새로운 file로 생성이 된다.  
하지만 이렇게 할 경우 log파일이 무자비하게 생성된다는 문제점이 있어 하루에 하나씩 log file이 생성되게 하는 것이 일반적이다.  
  
이 때 사용하는 것이 RollingFileAppender이다.

### RollingFileAppender

```html
<appender name="ROLLING_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
        <fileNamePattern>logs/access-%d{yyyy-MM-dd}.log</fileNamePattern>
    </rollingPolicy>

    <encoder>
        <pattern>${LOG_PATTERN}</pattern>
    </encoder>
</appender>

<logger name = "org.prgrms.kdt" level = "debug">
    <appender-ref ref="ROLLING_FILE"/>
</logger>
```

이렇게 작성하여 서버를 구동하게 되면 하루에 하나의 file이 만들어지고 그날의 log들이 그 파일에 기록되게 된다.

## Conversion
---

conversion은 message format의 요소들의 색지정을 통해 log를 더 알아보기 쉽게 해주는 것이다.

```html
<conversionRule
        conversionWord="clr"
        converterClass="org.springframework.boot.logging.logback.ColorConverter" />

<property name="CONSOLE_LOG_PATTERN" value="%clr(%d{HH:mm:ss.SSS}){cyan} [%thread] %clr(%-5level) %logger{36} - %msg%n" />

<property name="FILE_LOG_PATTERN" value="%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n" />
```

이러한 방식으로 %clr를 통해 색을 지정해줄 수 있다.  
여기서 주의사항은 CONSOLE에 출력될때와 달리 FILE에 기록될때는 적용이 안되므로 위와 같이 CONSOLE_LOG_PATTERN과 FILE_LOG_PATTERN을 따로 설정해주는 것이 좋다.