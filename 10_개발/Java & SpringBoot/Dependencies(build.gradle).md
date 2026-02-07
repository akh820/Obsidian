```groovy
dependencies {
    // ========== Initializr에서 이미 추가됨 ==========
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-security'
    implementation 'org.springframework.boot:spring-boot-starter-validation'
    runtimeOnly 'com.mysql:mysql-connector-j'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    
    // ========== 아래는 직접 추가해야 함 ==========
    
    // JWT (토큰 생성/검증)
    implementation 'io.jsonwebtoken:jjwt-api:0.12.3'
    runtimeOnly 'io.jsonwebtoken:jjwt-impl:0.12.3'
    runtimeOnly 'io.jsonwebtoken:jjwt-jackson:0.12.3'
    
    // Swagger (API 문서 자동 생성)
    implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.2.0'
    
    // AWS S3 (이미지 업로드) - 필요할 때만
    implementation 'software.amazon.awssdk:s3:2.20.26'
}
```

- 아래와 같이 변수로 버전 관리 가능
```groovy
// 변수로 버전 관리하면 편함
ext {
    jjwtVersion = '0.12.3'
}

dependencies {
    implementation "io.jsonwebtoken:jjwt-api:${jjwtVersion}"
    runtimeOnly "io.jsonwebtoken:jjwt-impl:${jjwtVersion}"
    runtimeOnly "io.jsonwebtoken:jjwt-jackson:${jjwtVersion}"
}
```
