![[Pasted image 20260207205114.png]]
[[연수기간중 개발 재활 가이드?]]
어떨때 Entity로 받고 어떨때 DTO로 받는지?
Controller  →  Service  →  Repository
   ↑              ↑              ↑
  DTO 받음      변환 처리      Entity 저장
  DTO 응답      Entity→DTO

DTO에 @NoArgsConstructor가 필요한이유,
Jackson이 JSON을 객체로 만들 때 **기본 생성자로 먼저 객체를 만들고** 값을 넣어주기 때문에 필요

###### Table 
- https://dbdiagram.io/d
Table users {

id Long [pk, increment]

username String [unique, not null]

password String [not null]

updated_at LocalDateTime

created_at LocalDateTime

}

  

Table posts {

id Long [pk, increment]

title String [not null, note: 'max 100']

content String [not null, note: 'TEXT']

user_id Long [not null, ref: > users.id]

created_at LocalDateTime

updated_at LocalDateTime

}

  

Table comments {

id Long [pk, increment]

content String [not null]

user_id Long [not null, ref: > users.id]

post_id Long [not null, ref: > posts.id]

updated_at LocalDateTime

created_at LocalDateTime

}

SpringSecurity 비활성화

[[Client -> Spring Boot HTTP 요청 처리순서]]

[[Js + React 설치 명령어(Vite)]]
[[자주사용하는 RESTAPI 어노테이션]]
axios res.data 가 리턴 값
react 컴포넌트는 대문자
변수 사용 x useState 사용

vscode에서 cmd + 클릭 안될때 
jsconfig.json 파일 생성후 리로드

이동시 라우터
라우터 설치
###### SecurityConfig
###### WebConfig
```java
package com.example.javajsreact.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")  // /api로 시작하는 경로만
                .allowedOrigins(
                        "http://localhost:5173",  // 개발: Vite
                        "http://localhost:3000"   // 개발: CRA (필요시)
                )
                .allowedMethods("GET", "POST", "PUT", "DELETE", "PATCH")
                .allowedHeaders("*")
                .allowCredentials(true);  // 쿠키/인증 정보 포함 허용
    }
}
```

###### vite.config.js
```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

// https://vite.dev/config/
export default defineConfig({
  plugins: [react()],//jsx 파일 처리해줌
  server: { // server 옵션은 vide dev 실행 시에만 동작
    proxy: {
      "/api": { // /api 시작하는 요청을 target으로 전달
        target: "http://localhost:8080",
        changeOrigin: true,
      },
    },
  },
});

```
