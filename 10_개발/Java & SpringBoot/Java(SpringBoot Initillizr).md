SpringBoot Initilizr : https://start.spring.io/

![[Pasted image 20260113172028.png]]
이대로 만들면 backend 폴더 생성
![[Pasted image 20260113172508.png]]

com.회사명/개인.프로젝트명
## 📦 Project (빌드 도구)

| 옵션                    | 설명                                                                                          |
| --------------------- | ------------------------------------------------------------------------------------------- |
| **Gradle - Groovy** ✅ | 빌드 스크립트를 <br><br>```<br>build.gradle<br>```<br><br> (Groovy 문법)로 작성. **가장 많이 사용**           |
| **Gradle - Kotlin**   | 빌드 스크립트를 <br><br>```<br>build.gradle.kts<br>```<br><br> (Kotlin 문법)로 작성. 타입 안전성 있지만 러닝커브 있음 |
| **Maven**             | ```<br>pom.xml<br>```<br><br>로 빌드. 예전 방식. XML이라 장황함                                         |
## 🗣️ Language (프로그래밍 언어)

|옵션|설명|
|---|---|
|**Java** ✅|가장 보편적. 대부분의 Spring 튜토리얼/자료가 Java 기준|
|**Kotlin**|간결한 문법. 안드로이드/최신 프로젝트에서 인기|
|**Groovy**|동적 타입 언어. 테스트 코드에서 가끔 사용|
## 📋 Project Metadata

### **Group** (패키지 그룹명)

com.example  →  com.yourcompany

- 회사/개인 도메인을 **역순**으로 입력
### **Artifact** (프로젝트명)

demo  →  equip-request

- 프로젝트 이름 (소문자, 하이픈 사용)
- JAR 파일명이 됨: 
    ```
    equip-request-0.0.1.jar
    ```

### **Name** (표시 이름)

- Artifact랑 보통 같게 설정
- 프로젝트 표시용 이름

### **Description** (설명)

- 프로젝트 설명. 나중에 수정 가능

### **Package name** (기본 패키지)

com.example.demo  →  backend (또는 com.angyehong.equiprequest)

- 자동 생성됨 (Group + Artifact)
- 메인 클래스가 이 패키지에 생성됨

## 📦 Packaging (패키징 방식)

| 옵션        | 설명                                                                              |
| --------- | ------------------------------------------------------------------------------- |
| **Jar** ✅ | 내장 톰캣 포함. <br><br>```<br>java -jar app.jar<br>```<br><br>로 바로 실행. **대부분 이걸 사용** |
| **War**   | 외부 톰캣 서버에 배포할 때 사용. 요즘 잘 안 씀                                                    |

**추천**: Jar (Docker, 클라우드 배포에 적합)