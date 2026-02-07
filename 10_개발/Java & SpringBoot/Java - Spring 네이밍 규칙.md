#Java #Spring
## Java/Spring 네이밍 규칙 (간략)

### Java 코드

**클래스명: PascalCase**

```java
UserEntity, BaseEntity, OrderService
```

**변수/메서드명: camelCase**

```java
createdAt, findById(), getUserName()
```

**상수: UPPER_SNAKE_CASE**

```java
MAX_SIZE, DEFAULT_VALUE
```

**패키지명: 소문자**

```java
backend.domain, backend.service
```

### 데이터베이스

**테이블/컬럼명: snake_case**

```sql
user_entity, created_at, user_name
```

### Spring 관례

**엔티티 클래스:**

- `User` (추천) 또는 `UserEntity`

**Repository:**

- `UserRepository`

**Service:**

- `UserService`

**Controller:**

- `UserController`

**핵심:**

- Java = camelCase
- DB = snake_case
- 클래스 = PascalCase