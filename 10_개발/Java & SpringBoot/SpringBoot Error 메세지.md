
## 발생한 문제

### 에러 1: Hibernate Dialect 감지 실패
```
Unable to determine Dialect without JDBC metadata (please set 'jakarta.persistence.jdbc.url' for common cases or 'hibernate.dialect' when a custom Dialect implementation must be provided)
```

### 에러 2: MySQL 호환성 문제
```
HHH100045: Could not obtain connection metadata: java.sql.SQLSyntaxErrorException: Unknown column 'RESERVED' in 'WHERE'
```

## 원인 분석

1. **Spring Boot 4.0.1**은 **Hibernate 7.2.0.Final**을 사용합니다
2. Hibernate 7.2는 MySQL 메타데이터를 조회할 때 `RESERVED` 컬럼을 사용하는데, 이는 MySQL 8.0의 새로운 기능입니다
3. 만약 MySQL 버전이 8.0 미만이거나 특정 설정이 없다면 이 에러가 발생합니다
4. Hibernate가 자동으로 dialect를 감지하지 못했습니다

## 해결 방법

### 방법 1: application.yaml에 Hibernate Dialect 명시 (권장)

`backend/src/main/resources/application.yaml` 파일에 다음 설정을 추가:

```yaml
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
      ddl-auto: update
    properties:
      hibernate:
        show_sql: true
        format_sql: true
```

## 권장 설정

개발 환경에 적합한 전체 JPA 설정:

```yaml
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
      ddl-auto: update  # 개발: update, 운영: validate
    properties:
      hibernate:
        show_sql: true
        format_sql: true
        use_sql_comments: true
    open-in-view: false
```

## 참고 사항

- `ddl-auto: update`는 개발 환경에서만 사용하세요
- 운영 환경에서는 `ddl-auto: validate` 또는 완전히 제거하는 것이 좋습니다
- SQL 로깅은 디버깅에 유용하지만 운영 환경에서는 비활성화하세요
