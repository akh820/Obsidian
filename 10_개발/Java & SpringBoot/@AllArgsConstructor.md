#jpa 
## @AllArgsConstructor 

**모든 필드를 파라미터로 받는 생성자 자동 생성**

```java
@AllArgsConstructor
// 자동 생성:
public User(String email, String password, String name, UserRole role) {
    this.email = email;
    this.password = password;
    this.name = name;
    this.role = role;
}
```

**왜 필요?**

- **@Builder가 내부적으로 AllArgsConstructor를 사용함**
- Builder 없이 AllArgs만 쓰면 단독으로도 사용 가능

```java
// AllArgs로 직접 생성 (Builder 없이도 가능)
User user = new User("test@test.com", "1234", "홍길동", UserRole.USER);
```

**실무:**

- Builder와 세트로 사용
- Builder가 AllArgs를 내부적으로 활용