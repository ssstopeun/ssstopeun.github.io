---
title: Day18.JDBC & Spring
date: 2022-08-11 12:00:00 +0900
categories: [Backend, SpringBoot]
tags: [SpringBoot, Backend, SW, JDBC] 
author: author_id 
---

# [Day18] JDBC & Spring


## Data Connection Pool (DBCP)
---
> Data와 관련된 connection을 pool에 미리 만들어 두고 필요할 때마다 가져와 사용하는 방식이다.

1. pool에서 connection을 가져온다.
2. connection을 사용한다.
3. connection을 pool에 반환한다.

## HikariCP
---
> HikariCP는 2012년도경에 Brett Wooldridge가 개발한 매우 가볍고 빠른 JDBC Connection Pool이다.

HikariCP를 사용할때는 기존의 연결방식과 조금 다르게 DB와 연결한다.

```java
// 기존의 연결 방식
var connection = DriverManager.getConnection("jdbc:mysql://localhost/order_mgmt", "user_id", "user_password");

// HikariCP를 사용할때 연결 방식
var connection = dataSource.getConnection();
```

- **@TestMethodOrder(MethodOrderer.OrderAnnotation.class)**
  : 이 코드를 Test Class에 입력하면 Order(n)을 통해 Test할 순서를 정해줄 수 있다.

## JDBC Template
---
> 꼭 변경할 코드만 변경하여 간결하게 코드를 작성하게 해주는 것이 JDBC Templete이다.

```java
@Override
public List<Customer> findAll() {
    List<Customer> allCustomers = new ArrayList<>();
    try (
            var connection = dataSource.getConnection();
            var statement = connection.prepareStatement("select * from customers");
            var resultSet = statement.executeQuery();
    ) {
        while (resultSet.next()) {
            mapToCustomer(allCustomers, resultSet);
        }
    } catch (SQLException throwable) {
        logger.error("Got error while closing connection", throwable);
        throw new RuntimeException(throwable);
    }
    return allCustomers;
}
```

이렇게 작성된 코드를 Jdbc Template을 사용하면

```java
@Override
public List<Customer> findAll() {
    return jdbcTemplate.query("select * from customer", customerRowMapper);
}
```

이렇게 훨씬 간단하게 findAll을 작성할 수 있다. 하지만 이를 위해서는 customerRowMapper를 정의해 주어야 한다.

```java
private static final RowMapper<Customer> customerRowMapper = (resultSet, i) -> {
    var customerName = resultSet.getString("name");
    var email = resultSet.getString("email");
    var customerId = toUUID(resultSet.getBytes("customer_id"));
    var lastLoginAt = (resultSet.getTimestamp("Last_login_at") != null) ? resultSet.getTimestamp("last_login_at").toLocalDateTime() : null;
    var createdAt = resultSet.getTimestamp("created_at").toLocalDateTime();
    return new Customer(customerId, customerName, email, lastLoginAt, createdAt);        
};
```

이렇게 RowMapper를 추가해주면 findAll, findById등의 코드를 query문만 바꿔주며 더 쉽게 작성할 수 있다.
<br>

findById 코드도 한번 바꿔보자.

```java
@Override
public Optional<Customer> findById(UUID customerId) {
    try{
        return Optional.ofNullable(jdbcTemplate.queryForObject("select * from customers WHERE customer_id = UUID_TO_BIN(?)", 
        customerRowMapper, 
        customerId.toString().getBytes()));
    } catch(EmptyResultDataAccessException e){
        logger.error("Got empty result", e);
        return Optional.empty();
    }
}
```

이렇게 **jdbcTemplate.queryForObject**를 사용하게 되는데 이는 findById 뿐 아니라 한 건의 데이터를 사용해 구하는 findByEmail, findByName등의 기능을 모두 처리할 수 있다.
<br>

그렇다면 update, insert 처럼 조회가 아닌 갱신을 필요로 하는 기능은 어떻게 할까? update를 예시로 보겠다.

```java
@Override
public Customer update(Customer customer) {
    var update = jdbcTemplate.update("UPDATE customers SET name = ?, email = ?, last_login_at = > WHERE customer_id = UUID_TO_BIN(?)",
            customer.getName(),
            customer.getEmail(),
            customer.getLastLoginAt() != null ? Timestamp.valueOf(customer.getLastLoginAt()) : null,
            customer.getCustomerId().toString().getBytes());
    if (update != 1) {
        throw new RuntimeException("Nothing was updated");
    }
    return customer;
}
```

**jdbcTemplate.update**를 사용하게 되는데 query문을 작성하고 ? 에 들어갈 것을 차례로 나열해 주면 된다. 그 후 update가 없을때의 예외처리까지 해주면 훨씬 간단한 코드가 된다.

<br>

이와 같이 jdbcTemplate를 이용해 코드를 수정해 보았는데 jdbcTemplate의 용도와 같이 꼭 입력해야하는 query, 변수등의 정보만 입력하여 기능을 구현할 수 있어 훨씬 간단하게 작성할 수 있다.