### 1. Builder는 모든 필드를 null로 초기화

```java
@Builder
public class Example {
    private String name = "기본이름";  // 무시됨 → null
    private List<String> items = new ArrayList<>();  // 무시됨 → null
}
```

### 2. List는 무조건 @Builder.Default?

**아니요, 필요할 때만:**

```java
// add() 사용 예정 → @Builder.Default 필요
@Builder.Default
private List<RequestItem> items = new ArrayList<>();

// 나중에 새 리스트로 교체 → 불필요
private List<RequestItem> items;  // null OK
```

### 3. @Builder.Default는 @Builder와 세트

- @Builder 없으면 사용 불가
- @Builder와 함께만 의미 있음

### 4. 선택적 기본값

```java
@Builder
public class Example {
    private String name;  // → null (기본 Builder 동작)
    
    @Builder.Default
    private List<String> items = new ArrayList<>();  // → []
    
    @Builder.Default
    private int count = 0;  // → 0
}
```

**핵심: @Builder.Default = "이 필드만 기본값 써줘"**