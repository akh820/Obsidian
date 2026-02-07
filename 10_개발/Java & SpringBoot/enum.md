#jpa 
## @Enumerated 사용 규칙 (간략)

**네, enum 필드에는 항상 사용해야 합니다!**

```java
// ✅ 올바른 사용
@Enumerated(EnumType.STRING)
private UserRole role;

// ❌ 안 붙이면?
private UserRole role;  
// 기본값 = EnumType.ORDINAL (숫자로 저장, 위험!)
```

**왜 필요?**

- JPA가 enum을 DB에 어떻게 저장할지 몰라서
- 명시적으로 "문자열로 저장해줘" 알려줘야 함

**규칙:**

```java
// enum 필드 있으면 항상 이 조합
@Enumerated(EnumType.STRING)  // ← 필수!
@Column(nullable = false)
private SomeEnum someField;
```

**요약:**

- enum 사용 → @Enumerated 필수
- 항상 STRING 사용 ⭐
- JPA가 DB와 연결해주려면 어떻게 저장할지 알려줘야 함