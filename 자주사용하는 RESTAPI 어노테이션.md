Spring REST API에서 자주 사용하는 어노테이션들을 정리해드릴게요:

## 1. HTTP 메서드 매핑

```java
@GetMapping("/users")           // 조회
@PostMapping("/users")          // 생성
@PutMapping("/users/{id}")      // 전체 수정
@PatchMapping("/users/{id}")    // 일부 수정
@DeleteMapping("/users/{id}")   // 삭제

// 또는 통합 방식
@RequestMapping(value = "/users", method = RequestMethod.GET)
```

---

## 2. 데이터 받는 어노테이션

### ① @RequestParam - 쿼리 파라미터

```java
// GET /api/search?keyword=java&page=1
@GetMapping("/search")
public String search(
    @RequestParam String keyword,
    @RequestParam(defaultValue = "1") int page
) { }
```

### ② @PathVariable - URL 경로

```java
// GET /api/users/123
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) { }

// GET /api/users/123/posts/456
@GetMapping("/users/{userId}/posts/{postId}")
public Post getPost(
    @PathVariable Long userId,
    @PathVariable Long postId
) { }
```

### ③ @RequestBody - JSON 본문

```java
// POST /api/users
// Body: {"name": "계홍", "age": 32}
@PostMapping("/users")
public User createUser(@RequestBody UserDto userDto) { }
```

### ④ @RequestHeader - HTTP 헤더

```java
@GetMapping("/users")
public List<User> getUsers(
    @RequestHeader("Authorization") String token
) { }
```

### ⑤ @CookieValue - 쿠키

```java
@GetMapping("/profile")
public String getProfile(
    @CookieValue("sessionId") String sessionId
) { }
```

---

## 3. 응답 관련

```java
@ResponseStatus(HttpStatus.CREATED)  // 201 상태 코드
@PostMapping("/users")
public User createUser(@RequestBody UserDto dto) { }

// ResponseEntity로 직접 제어
@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody UserDto dto) {
    User user = userService.create(dto);
    return ResponseEntity
        .status(HttpStatus.CREATED)
        .body(user);
}
```

---

## 4. 유효성 검증

```java
@PostMapping("/users")
public User createUser(@Valid @RequestBody UserDto dto) { }

// DTO 클래스
public class UserDto {
    @NotBlank(message = "이름은 필수입니다")
    private String name;
    
    @Min(value = 1, message = "나이는 1 이상이어야 합니다")
    private int age;
    
    @Email(message = "이메일 형식이 아닙니다")
    private String email;
}
```

---

## 5. 실전 조합 예시

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    // 전체 조회 (페이징)
    @GetMapping
    public List<User> getUsers(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size
    ) { }

    // 단건 조회
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) { }

    // 생성
    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public User createUser(@Valid @RequestBody UserDto dto) { }

    // 수정
    @PutMapping("/{id}")
    public User updateUser(
        @PathVariable Long id,
        @RequestBody UserDto dto
    ) { }

    // 일부 수정
    @PatchMapping("/{id}")
    public User patchUser(
        @PathVariable Long id,
        @RequestBody Map<String, Object> updates
    ) { }

    // 삭제
    @DeleteMapping("/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    public void deleteUser(@PathVariable Long id) { }

    // 검색 (복합 조건)
    @GetMapping("/search")
    public List<User> searchUsers(
        @RequestParam(required = false) String name,
        @RequestParam(required = false) Integer minAge,
        @RequestParam(required = false) Integer maxAge
    ) { }
}
```

---

## 6. 자주 쓰는 패턴 정리

|상황|어노테이션|예시 URL|
|---|---|---|
|목록 조회|`@GetMapping` + `@RequestParam`|`/api/users?page=1`|
|단건 조회|`@GetMapping` + `@PathVariable`|`/api/users/123`|
|생성|`@PostMapping` + `@RequestBody`|`/api/users`|
|전체 수정|`@PutMapping` + `@PathVariable` + `@RequestBody`|`/api/users/123`|
|일부 수정|`@PatchMapping` + `@PathVariable` + `@RequestBody`|`/api/users/123`|
|삭제|`@DeleteMapping` + `@PathVariable`|`/api/users/123`|

---

## 7. 헷갈리는 구분

**@RequestParam vs @PathVariable**

```java
// @RequestParam - 선택적, 여러 개 가능, 검색/필터링
GET /api/users?name=계홍&age=32

// @PathVariable - 필수, 리소스 식별
GET /api/users/123
```

**@RequestBody vs @RequestParam**

```java
// @RequestBody - POST/PUT으로 JSON 전송
POST /api/users
Body: {"name": "계홍"}

// @RequestParam - GET으로 쿼리 파라미터
GET /api/users?name=계홍
```

---

이 정도가 실무에서 90% 이상 사용하는 패턴입니다. 더 궁금한 거 있으면 물어보세요!