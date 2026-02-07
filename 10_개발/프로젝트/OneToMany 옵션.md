## OneToMany 자주 쓰는 옵션 (간략)

### 1. cascade (가장 중요) ⭐

```java
CascadeType.ALL        // 모든 작업 전파 (실무 많이 씀)
CascadeType.PERSIST    // 저장만 전파
CascadeType.REMOVE     // 삭제만 전파
CascadeType.MERGE      // 업데이트만 전파
```

**실무 패턴:**

```java
// 부모-자식 관계 (강한 결합)
@OneToMany(cascade = CascadeType.ALL)  // ⭐ 가장 많이 씀

// 저장만 전파 (삭제는 별도)
@OneToMany(cascade = CascadeType.PERSIST)

// 여러 개 조합
@OneToMany(cascade = {CascadeType.PERSIST, CascadeType.MERGE})
```

---

### 2. orphanRemoval (고아 객체)

```java
@OneToMany(
    cascade = CascadeType.ALL,
    orphanRemoval = true  // 리스트에서 제거하면 DB도 삭제
)
private List<RequestItem> items;

// 사용
request.getItems().remove(item);  // DB에서도 삭제됨!
```

---

### 3. fetch

```java
FetchType.LAZY   // 기본값, 권장 ⭐
FetchType.EAGER  // 거의 안 씀
```

---

### 실무 권장 조합

```java
@OneToMany(
    mappedBy = "parent",
    cascade = CascadeType.ALL,    // 자주 씀
    orphanRemoval = true,          // 선택
    fetch = FetchType.LAZY         // 기본값
)
private List<Child> children = new ArrayList<>();
```