### EAGER (즉시 로딩)

```java
@ManyToOne(fetch = FetchType.EAGER)
@JoinColumn(name = "user_id")
private User user;

// 사용 코드
EquipmentRequest request = repository.findById(1L);
// ↑ 이 순간! request + user 둘 다 조회됨

// user 사용 안 해도 이미 조회됨
System.out.println(request.getId());  // user는 사용 안 했지만 이미 DB에서 가져옴
```

**실행 SQL:**

```sql
-- findById 호출 시점에 바로 JOIN
SELECT r.*, u.* 
FROM equipment_requests r 
LEFT JOIN users u ON r.user_id = u.id 
WHERE r.id = 1;
```

---

### LAZY (지연 로딩)

```java
@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "user_id")
private User user;

// 1단계: request만 조회
EquipmentRequest request = repository.findById(1L);
// ↑ 이 순간: request만 조회, user는 프록시 객체 (가짜)

System.out.println(request.getId());  // ✅ OK, user 조회 안 함

// 2단계: user 처음 사용할 때 조회
String userName = request.getUser().getName();
//                         ↑ 이 순간! user를 실제 사용 → DB 조회
```

**실행 SQL:**

```sql
-- 1단계: findById 시점
SELECT * FROM equipment_requests WHERE id = 1;

-- 2단계: getUser().getName() 시점
SELECT * FROM users WHERE id = ?;
```

---

### "필요할 때"의 정확한 의미

```java
@ManyToOne(fetch = FetchType.LAZY)
private User user;

EquipmentRequest request = repository.findById(1L);

// ❌ user 조회 안 됨
request.getId();
request.getStatus();

// ✅ 이 순간 user 조회!
request.getUser();  // 이것만으론 조회 안 됨 (프록시 반환)
request.getUser().getName();  // ← 이 순간 조회!
request.getUser().getEmail(); // ← 이미 조회됐으니 추가 쿼리 없음
```

**핵심:**

- **LAZY = User 객체의 필드를 실제로 사용하는 순간** 조회
- `getUser()`만 호출 → 조회 안 됨 (프록시 반환)
- `getUser().getName()` → 조회됨! (실제 데이터 필요)