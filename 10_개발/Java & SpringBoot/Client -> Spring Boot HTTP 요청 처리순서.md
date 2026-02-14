Spring Boot에서 HTTP 요청이 처리되는 순서입니다:

```
[클라이언트 요청]
     ↓
1. Servlet Container (Tomcat 등)
     ↓
2. Filter Chain ← Spring Security가 여기 있음!
     │
     ├─ CharacterEncodingFilter (인코딩 처리)
     ├─ SecurityFilterChain ← SecurityConfig에서 설정한 것
     │   ├─ CSRF 체크
     │   ├─ 인증 체크
     │   └─ 권한 체크
     └─ 기타 Custom Filters
     ↓
3. DispatcherServlet (Spring MVC의 진입점)
     ↓
4. HandlerMapping (어떤 Controller를 실행할지 결정)
     ↓
5. Interceptor - preHandle() ← WebConfig에서 설정
     ↓
6. Controller ← 여기서 비즈니스 로직 실행
     ↓
7. Service Layer
     ↓
8. Repository Layer
     ↓
9. Interceptor - postHandle()
     ↓
10. ViewResolver (JSON 변환 등)
     ↓
11. Interceptor - afterCompletion()
     ↓
[클라이언트 응답]
```

**핵심 포인트:**

1. **Security가 Controller보다 먼저 실행됩니다**
    
    - Filter는 Servlet(Spring MVC) 앞단에서 동작
    - 인증/권한이 없으면 Controller에 도달하기 전에 막힘
2. **설정 로딩 순서 (애플리케이션 시작 시)**
    
    ```
    Application 시작
    → @Configuration 클래스들 로딩
    → @Bean 메서드 실행
    → SecurityConfig의 securityFilterChain() 실행
    → SecurityFilterChain 객체 생성되어 Filter로 등록
    → 이후 모든 HTTP 요청이 이 Filter를 거침
    ```
    
3. **실제 요청 예시:**
    
    ```
    GET /api/users
    
    ① SecurityFilterChain 통과
       - CSRF 체크 (비활성화 상태)
       - 인증 체크 (permitAll이라 통과)
    
    ② DispatcherServlet이 받음
    
    ③ /api/users를 처리하는 Controller 찾음
    
    ④ Controller 메서드 실행
    
    ⑤ 응답 반환
    ```
    

**지금 상태에서는:**

- SecurityConfig가 모든 요청을 `permitAll()`로 설정
- Security Filter는 있지만 아무것도 막지 않음
- 바로 Controller로 통과

**나중에 인증 추가하면:**

```java
.authorizeHttpRequests(auth -> auth
    .requestMatchers("/api/public/**").permitAll()
    .anyRequest().authenticated()  // 여기서 막힘!
)
```

이렇게 하면 인증 없는 요청은 Controller 도달 전에 401 에러로 끝납니다.

이해되셨나요?