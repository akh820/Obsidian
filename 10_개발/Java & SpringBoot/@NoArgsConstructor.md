#jpa 
```java
@NoArgsConstructor(access = AccessLevel.PUBLIC)    // public User() {}
@NoArgsConstructor(access = AccessLevel.PROTECTED) // protected User() {} ⭐ 권장
@NoArgsConstructor(access = AccessLevel.PRIVATE)   // private User() {} - JPA 안 됨!
@NoArgsConstructor  // 기본값 = PUBLIC
```

## NoArgsConstructor(PROTECTED) 사용 이유 3가지

### 1. JPA 요구사항 (필수)

- JPA가 리플렉션으로 빈 객체 만들고 → 값 주입
- NoArgsConstructor 없으면 JPA 작동 안 됨

### 2. 외부 빈 객체 생성 방지 (PROTECTED)

```java
// PUBLIC이면
User user = new User();  // ✅ 가능 - 빈 객체 생성 (위험!)
// email, password 모두 null → 데이터 오류

// PROTECTED면
User user = new User();  // ❌ 컴파일 에러 - 외부 생성 막음
```

### 3. Builder 패턴 강제

```java
// 이렇게만 생성 가능 (안전)
User user = User.builder()
    .email("test@test.com")   // 필수 값 명시
    .password("1234")
    .name("홍길동")
    .role(UserRole.USER)
    .build();  // 모든 값이 설정된 완전한 객체
```

**정리:**

- NoArgs = JPA 작동
- PROTECTED = 빈 객체 방지
- 결과 = Builder만 사용 (안전한 객체 생성)
