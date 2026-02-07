- [[@Builder.Default]]
## Builder 패턴

**일반 생성자의 문제:**

```java
// 순서 헷갈림, 가독성 나쁨
User user = new User("test@test.com", "1234", "홍길동", UserRole.USER);
User user = new User("홍길동", "test@test.com", "1234", UserRole.USER);  // ❌ 순서 틀림
```

**Builder 패턴:**

```java
User user = User.builder()
    .email("test@test.com")
    .password("1234")
    .name("홍길동")
    .role(UserRole.USER)
    .build();
```

**장점:**

1. **가독성** - 어떤 값이 어떤 필드인지 명확
2. **순서 자유** - 순서 바뀌어도 OK
3. **선택적 파라미터** - 필요한 것만 설정
4. **불변 객체** - setter 없이 생성 가능

**왜 강제?**

- 실수 방지
- 코드 깔끔
- 검증 로직 추가 가능

**업계 표준?**

- Java/Spring에서 **매우 흔하게 사용** ⭐
- 하지만 "무조건"은 아님

**언제 사용?**

- Entity: 자주 씀 (특히 필드 많을 때)
- DTO: 거의 필수
- 불변 객체: 필수

**디자인 패턴 (GoF):**

- Java만의 것이 아님
- 다른 언어에도 있음 (Kotlin, C#, Python 등)
- GoF의 23가지 디자인 패턴 중 하나

**실무:**

```java
// 필드 2~3개: Builder 안 써도 됨
// 필드 4개 이상: Builder 권장
// 필드 많고 복잡: Builder 거의 필수
```

**아키텍처 패턴 vs 디자인 패턴:**

- 아키텍처 패턴 ❌ (MVC, 레이어드 등)
- **디자인 패턴** ✅ (객체 생성 패턴)

