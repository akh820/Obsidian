#jpa
## JPA 연관관계 애노테이션 (간략)
Many = N , One = 1

OneToMany는 사실상 없어도 된다.
ManyToOne은 기본값이 eager 이기 때문에 항상 명시적으로 lazy사용

FetchType은 **2가지만 있음:**
- LAZY (지연 로딩)
- EAGER (즉시 로딩)

기본값 정리:**

- `@ManyToOne` → 기본값 **EAGER** ❌
- `@OneToMany` → 기본값 **LAZY** ✅
- `@OneToOne` → 기본값 **EAGER** ❌
- `@ManyToMany` → 기본값 **LAZY** ✅

**4가지 종류:**
### 1. @ManyToOne (N:1) - 가장 많이 씀 ⭐

```java
// 여러 요청 → 한 명의 유저
@ManyToOne
private User user;
```

### 2. @OneToMany (1:N)

```java
// 한 명의 유저 → 여러 요청
@OneToMany(mappedBy = "user")
private List<EquipmentRequest> requests;
```

### 3. @OneToOne (1:1)

```java
// 유저 1명 → 프로필 1개
@OneToOne
private UserProfile profile;
```

### 4. @ManyToMany (N:M) - 거의 안 씀

```java
// 학생 여러명 ↔ 강의 여러개
@ManyToMany
private List<Course> courses;
```

## FetchType 정리 (간략)

**2가지만 있음:**

- LAZY (지연 로딩)
- EAGER (즉시 로딩)

**보통 LAZY만 사용? → 맞습니다! ⭐**

---

## 중요! @ManyToOne 기본값

```java
// ❌ 이렇게만 쓰면
@ManyToOne
private User user;
// 기본값 = EAGER! (즉시 로딩)

// ✅ 그래서 명시해야 함
@ManyToOne(fetch = FetchType.LAZY)  // ← 필수!
private User user;
```

**기본값 정리:**

- `@ManyToOne` → 기본값 **EAGER** ❌
- `@OneToMany` → 기본값 **LAZY** ✅
- `@OneToOne` → 기본값 **EAGER** ❌
- `@ManyToMany` → 기본값 **LAZY** ✅