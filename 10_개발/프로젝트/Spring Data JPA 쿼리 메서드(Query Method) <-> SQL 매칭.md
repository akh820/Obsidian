**핵심 패턴:**

```
find   + By    + 필드명  + 조건     + OrderBy + 필드명 + Asc/Desc
조회     조건     email    Containing   정렬       name     오름차순
```

### 기본 조회

|메서드|SQL|
|---|---|
|`findById(1L)`|`SELECT * FROM users WHERE id = 1`|
|`findAll()`|`SELECT * FROM users`|
|`findByEmail("a@a.com")`|`SELECT * FROM users WHERE email = 'a@a.com'`|
|`findByName("홍길동")`|`SELECT * FROM users WHERE name = '홍길동'`|

### 조건 조합

|메서드|SQL|
|---|---|
|`findByNameAndEmail(name, email)`|`WHERE name = ? AND email = ?`|
|`findByNameOrEmail(name, email)`|`WHERE name = ? OR email = ?`|
|`findByRoleAndNameContaining(role, name)`|`WHERE role = ? AND name LIKE '%?%'`|

### 비교 연산

|메서드|SQL|
|---|---|
|`findByAgeGreaterThan(20)`|`WHERE age > 20`|
|`findByAgeLessThan(30)`|`WHERE age < 30`|
|`findByAgeBetween(20, 30)`|`WHERE age BETWEEN 20 AND 30`|
|`findByAgeGreaterThanEqual(20)`|`WHERE age >= 20`|

### 문자열 검색

|메서드|SQL|
|---|---|
|`findByNameContaining("홍")`|`WHERE name LIKE '%홍%'`|
|`findByNameStartingWith("홍")`|`WHERE name LIKE '홍%'`|
|`findByNameEndingWith("동")`|`WHERE name LIKE '%동'`|
|`findByNameLike("%길%")`|`WHERE name LIKE '%길%'`|

### NULL 체크

|메서드|SQL|
|---|---|
|`findByEmailIsNull()`|`WHERE email IS NULL`|
|`findByEmailIsNotNull()`|`WHERE email IS NOT NULL`|

### 정렬

|메서드|SQL|
|---|---|
|`findAllByOrderByNameAsc()`|`ORDER BY name ASC`|
|`findAllByOrderByCreatedAtDesc()`|`ORDER BY created_at DESC`|
|`findByRoleOrderByNameAsc(role)`|`WHERE role = ? ORDER BY name ASC`|

### 개수/존재 여부

|메서드|SQL|
|---|---|
|`countByRole(ADMIN)`|`SELECT COUNT(*) FROM users WHERE role = 'ADMIN'`|
|`existsByEmail("a@a.com")`|`SELECT COUNT(*) > 0 FROM users WHERE email = ?`|

### 삭제

| 메서드                        | SQL                                 |
| -------------------------- | ----------------------------------- |
| `deleteById(1L)`           | `DELETE FROM users WHERE id = 1`    |
| `deleteByEmail("a@a.com")` | `DELETE FROM users WHERE email = ?` |

### 제한 (LIMIT)

|메서드|SQL|
|---|---|
|`findFirstByOrderByCreatedAtDesc()`|`ORDER BY created_at DESC LIMIT 1`|
|`findTop3ByRoleOrderByNameAsc(role)`|`WHERE role = ? ORDER BY name ASC LIMIT 3`|

---

