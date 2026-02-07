
```yml
spring:
  application:
    name: backend
  datasource:
    url: jdbc:mysql://localhost:3306/resource_tracker
    username: dev
    password: 1234
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    database-platform: org.hibernate.dialect.MySQLDialect
    hibernate:
      ddl-auto: update # [[ddl-auto]]
    properties:
      hibernate:
        show_sql: true
        format_sql: true
```
