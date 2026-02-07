# 🎯 Equip-Request 프로젝트 2주 완성 복습 가이드

> **목표**: AI 도움 없이 처음부터 직접 코딩하면서 풀스택 개발 실력 향상

---

## 📋 사전 준비

### 필수 환경
- Java 17+, Node.js 18+, MariaDB/MySQL
- IntelliJ IDEA / VS Code
- Postman (API 테스트용)

### 학습 방식
1. **코드를 보지 말고** 먼저 직접 작성해보기
2. 막히면 기존 코드 **일부만** 참고
3. 완성 후 기존 코드와 **비교 분석**

---

# 📅 1주차: Backend (Spring Boot)

## Day 1: 프로젝트 설정 & Entity 설계

### 📖 학습 목표
- Spring Boot 프로젝트 초기화
- JPA Entity 설계 원칙 이해

### ✍️ 직접 구현할 것

**1. 새 프로젝트 생성**
```bash
# Spring Initializr 또는 start.spring.io에서 생성
# Dependencies: Spring Web, Spring Data JPA, Spring Security, Lombok, Validation
```

**2. Entity 직접 설계해보기**
```
User
├── id (Long, PK)
├── email (String, unique)
├── password (String, BCrypt)
├── name (String)
├── role (Enum: USER, ADMIN)
└── createdAt, updatedAt (BaseEntity 상속)

Equipment (비품)
├── id, name, description, category
├── imageUrl, stock, available
└── createdAt, updatedAt

EquipmentRequest (신청)
├── id, user (ManyToOne)
├── status (Enum: PENDING, APPROVED, REJECTED)
├── reason, adminComment
├── requestItems (OneToMany)
└── createdAt, updatedAt

RequestItem (신청 항목)
├── id, request (ManyToOne)
├── equipment (ManyToOne)
└── quantity
```

### 🧠 핵심 개념 체크
- [ ] `@Entity`, `@Table` 어노테이션 용도
- [ ] `@ManyToOne`, `@OneToMany` 관계 설정
- [ ] `@MappedSuperclass`로 BaseEntity 만들기
- [ ] Lombok `@Builder`, `@Getter`, `@NoArgsConstructor` 활용

### 📁 참고 파일
- `backend/src/main/java/backend/domain/*.java`

---

## Day 2: Repository & Service Layer

### 📖 학습 목표
- Spring Data JPA Repository 패턴 이해
- Service Layer에서 비즈니스 로직 분리

### ✍️ 직접 구현할 것

**1. Repository 인터페이스 작성**
```java
public interface EquipmentRepository extends JpaRepository<Equipment, Long> {
    // 직접 작성해볼 메서드들:
    // - 카테고리별 조회
    // - 이름 키워드 검색
    // - 사용 가능한 비품만 조회
}
```

**2. Service 클래스 구현**
```java
@Service
@RequiredArgsConstructor
@Transactional(readOnly = true)
public class EquipmentService {
    // CRUD 메서드 구현
    // @Transactional 어디에 붙이는지 이해하기
}
```

### 🧠 핵심 개념 체크
- [ ] `JpaRepository` 기본 제공 메서드들
- [ ] Query Method 네이밍 규칙 (`findBy...`, `countBy...`)
- [ ] `@Transactional(readOnly = true)` vs `@Transactional`
- [ ] 생성자 주입 (`@RequiredArgsConstructor`)

### 📁 참고 파일
- `backend/src/main/java/backend/repository/*.java`
- `backend/src/main/java/backend/service/EquipmentService.java`

---

## Day 3: REST Controller & DTO

### 📖 학습 목표
- RESTful API 설계 원칙
- Request/Response DTO 패턴

### ✍️ 직접 구현할 것

**1. Controller 설계**
```java
@RestController
@RequestMapping("/api/equipment")
public class EquipmentController {
    // GET /api/equipment - 전체 조회
    // GET /api/equipment/{id} - 단건 조회
    // POST /api/equipment - 등록 (Admin)
    // PUT /api/equipment/{id} - 수정 (Admin)
    // DELETE /api/equipment/{id} - 삭제 (Admin)
}
```

**2. Inner Class DTO 패턴**
```java
// Controller 안에 static inner class로 Request/Response 정의
@Getter
@AllArgsConstructor
public static class EquipmentResponse {
    private Long id;
    private String name;
    // ...
}
```

### 🧠 핵심 개념 체크
- [ ] `@RequestBody`, `@PathVariable`, `@RequestParam`
- [ ] `ResponseEntity<T>` 활용법
- [ ] HTTP 상태 코드 적절히 반환하기
- [ ] `@RequestMapping` vs 개별 메서드 어노테이션

### 📁 참고 파일
- `backend/src/main/java/backend/controller/EquipmentController.java`

---

## Day 4: JWT 인증 시스템

### 📖 학습 목표
- JWT 토큰 구조 이해
- Spring Security + JWT 연동

### ✍️ 직접 구현할 것

**1. JwtUtil 클래스**
```java
@Component
public class JwtUtil {
    // generateAccessToken(userId, email, role)
    // generateRefreshToken(userId)
    // validateToken(token)
    // getUserIdFromToken(token)
}
```

**2. JwtAuthenticationFilter**
```java
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    // Authorization 헤더에서 토큰 추출
    // 토큰 검증 후 SecurityContext에 인증 정보 저장
}
```

**3. AuthController**
```java
// POST /api/auth/signup - BCrypt로 비밀번호 암호화
// POST /api/auth/login - 토큰 발급
// POST /api/auth/refresh - 토큰 갱신
```

### 🧠 핵심 개념 체크
- [ ] JWT 구조: Header.Payload.Signature
- [ ] Access Token vs Refresh Token 역할 구분
- [ ] `OncePerRequestFilter` 동작 원리
- [ ] `SecurityContextHolder` 사용법
- [ ] BCrypt 단방향 암호화

### 📁 참고 파일
- `backend/src/main/java/backend/util/JwtUtil.java`
- `backend/src/main/java/backend/filter/JwtAuthenticationFilter.java`
- `backend/src/main/java/backend/controller/AuthController.java`

---

## Day 5: Spring Security 설정

### 📖 학습 목표
- SecurityFilterChain 커스텀 설정
- CORS 설정

### ✍️ 직접 구현할 것

**1. SecurityConfig**
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    // 경로별 접근 권한 설정
    // /api/auth/** - 모두 허용
    // /api/admin/** - ADMIN만 허용
    // 그 외 - 인증 필요
}
```

**2. CorsConfig**
```java
// 개발/운영 환경별 CORS 설정
// 환경변수로 Allowed Origins 관리
```

### 🧠 핵심 개념 체크
- [ ] `SecurityFilterChain` Bean 정의
- [ ] `authorizeHttpRequests()` 패턴 매칭
- [ ] CSRF 비활성화 이유 (Stateless JWT)
- [ ] Filter 순서의 중요성

### 📁 참고 파일
- `backend/src/main/java/backend/config/SecurityConfig.java`
- `backend/src/main/java/backend/config/CorsConfig.java`

---

## Day 6: S3 이미지 업로드

### 📖 학습 목표
- AWS S3 SDK 사용법
- MultipartFile 처리

### ✍️ 직접 구현할 것

**1. S3 설정**
```java
@Configuration
public class S3Config {
    // S3Client Bean 생성
    // 환경변수에서 credentials 로드
}
```

**2. S3Service**
```java
@Service
public class S3Service {
    // uploadFile(MultipartFile) - 업로드 후 URL 반환
    // deleteFile(String url) - 파일 삭제
}
```

**3. Controller에서 @RequestPart 사용**
```java
@PostMapping(consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
public ResponseEntity<?> create(
    @RequestPart("data") CreateRequest request,
    @RequestPart(value = "image", required = false) MultipartFile image
)
```

### 🧠 핵심 개념 체크
- [ ] `@RequestPart` vs `@RequestParam`
- [ ] S3 버킷 권한 설정
- [ ] UUID로 고유 파일명 생성
- [ ] 파일 삭제 시 예외 처리

### 📁 참고 파일
- `backend/src/main/java/backend/config/S3Config.java`
- `backend/src/main/java/backend/service/S3Service.java`

---

## Day 7: 비품 신청 시스템 & 1주차 복습

### 📖 학습 목표
- 복잡한 비즈니스 로직 구현
- 1주차 전체 흐름 정리

### ✍️ 직접 구현할 것

**1. EquipmentRequestService**
```java
// 신청 생성: 여러 비품을 한번에 신청
// 재고 차감 로직 포함
// 승인/거절 처리: 거절 시 재고 복원
```

**2. 관리자 전용 엔드포인트**
```java
// GET /api/requests/admin/all - 전체 신청 조회
// POST /api/requests/admin/{id}/approve
// POST /api/requests/admin/{id}/reject
```

### 🧠 1주차 종합 체크리스트
- [ ] Entity → Repository → Service → Controller 흐름 이해
- [ ] JWT 인증 전체 플로우
- [ ] @Transactional 동작 원리
- [ ] 예외 처리 전략

---

# 📅 2주차: Frontend (React + TypeScript)

## Day 8: 프로젝트 설정 & 기본 구조

### 📖 학습 목표
- Vite + React + TypeScript 설정
- 폴더 구조 설계

### ✍️ 직접 구현할 것

**1. 프로젝트 생성**
```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install
```

**2. 추가 라이브러리 설치**
```bash
npm install axios zustand @tanstack/react-query react-router tailwindcss
```

**3. 폴더 구조 직접 설계**
```
src/
├── components/     # 재사용 가능한 UI 컴포넌트
├── pages/          # 페이지 컴포넌트
├── stores/         # Zustand 상태 관리
├── lib/            # API, 유틸리티
├── types/          # TypeScript 타입 정의
└── layouts/        # 레이아웃 컴포넌트
```

### 🧠 핵심 개념 체크
- [ ] `tsconfig.json` path alias 설정 (`@/...`)
- [ ] Tailwind CSS 설정
- [ ] ESLint + Prettier 설정

### 📁 참고 파일
- `frontend/package.json`
- `frontend/vite.config.ts`
- `frontend/src/` 폴더 구조

---

## Day 9: Zustand 상태 관리

### 📖 학습 목표
- Zustand로 전역 상태 관리
- persist 미들웨어로 localStorage 연동

### ✍️ 직접 구현할 것

**1. authStore.ts**
```typescript
interface AuthState {
  user: User | null;
  accessToken: string | null;
  refreshToken: string | null;
  isAuthenticated: boolean;
  
  setAuth: (user: User, accessToken: string, refreshToken: string) => void;
  logout: () => void;
  updateToken: (accessToken: string) => void;
}

export const useAuthStore = create<AuthState>()(
  persist(
    (set) => ({
      // 구현해보기
    }),
    { name: 'auth-storage' }
  )
);
```

**2. cartStore.ts**
```typescript
interface CartState {
  items: CartItem[];
  
  addItem: (equipment: Equipment, quantity: number) => void;
  removeItem: (equipmentId: number) => void;
  updateQuantity: (equipmentId: number, quantity: number) => void;
  clearCart: () => void;
}
```

### 🧠 핵심 개념 체크
- [ ] `create<T>()(...)` 문법 이해
- [ ] `persist` 미들웨어 동작
- [ ] Store 액션에서 `set()` 사용법
- [ ] Store 간 연동 (로그아웃 시 카트 초기화)

### 📁 참고 파일
- `frontend/src/stores/authStore.ts`
- `frontend/src/stores/cartStore.ts`

---

## Day 10: Axios 인터셉터 & API 설정

### 📖 학습 목표
- Axios 인스턴스 커스터마이징
- 자동 토큰 첨부 & 갱신

### ✍️ 직접 구현할 것

**1. api.ts**
```typescript
const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL || '/api',
});

// Request Interceptor: 토큰 자동 첨부
api.interceptors.request.use((config) => {
  // localStorage에서 토큰 가져와서 헤더에 추가
});

// Response Interceptor: 401 에러 처리
api.interceptors.response.use(
  (response) => response,
  async (error) => {
    // 401이면 refresh 시도 또는 로그인 페이지로
  }
);
```

### 🧠 핵심 개념 체크
- [ ] `axios.create()` 장점
- [ ] Request/Response Interceptor 차이
- [ ] Token Refresh 로직 이해
- [ ] 환경 변수 `import.meta.env` 사용

### 📁 참고 파일
- `frontend/src/lib/api.ts`

---

## Day 11: React Router & 페이지 라우팅

### 📖 학습 목표
- React Router v7 사용법
- 인증 보호 라우팅

### ✍️ 직접 구현할 것

**1. 라우터 설정**
```typescript
const router = createBrowserRouter([
  {
    path: '/',
    element: <MainLayout />,
    children: [
      { index: true, element: <HomePage /> },
      { path: 'equipment', element: <EquipmentListPage /> },
      { path: 'login', element: <LoginPage /> },
      // 인증 필요한 라우트
      { path: 'cart', element: <ProtectedRoute><CartPage /></ProtectedRoute> },
    ],
  },
]);
```

**2. ProtectedRoute 컴포넌트**
```typescript
function ProtectedRoute({ children }: { children: ReactNode }) {
  const { isAuthenticated } = useAuthStore();
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }
  
  return children;
}
```

### 🧠 핵심 개념 체크
- [ ] `createBrowserRouter` 사용법
- [ ] Outlet과 nested routes
- [ ] `<Navigate>` vs `useNavigate()`
- [ ] 라우트 보호 패턴

### 📁 참고 파일
- `frontend/src/router/index.tsx`
- `frontend/src/layouts/MainLayout.tsx`

---

## Day 12: TanStack Query (React Query)

### 📖 학습 목표
- 서버 상태 관리와 캐싱
- useQuery, useMutation 사용법

### ✍️ 직접 구현할 것

**1. QueryClient 설정**
```typescript
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000, // 5분
      retry: 1,
    },
  },
});
```

**2. Custom Hooks 작성**
```typescript
// 비품 목록 조회
export const useEquipments = () => {
  return useQuery({
    queryKey: ['equipments'],
    queryFn: async () => {
      const res = await api.get('/equipment');
      return res.data;
    },
  });
};

// 비품 신청
export const useCreateRequest = () => {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: async (data: CreateRequestDto) => {
      return api.post('/requests', data);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['myRequests'] });
    },
  });
};
```

### 🧠 핵심 개념 체크
- [ ] `queryKey` 설계 전략
- [ ] `staleTime` vs `gcTime`
- [ ] `invalidateQueries`로 캐시 갱신
- [ ] `isLoading`, `isError`, `data` 상태 처리

### 📁 참고 파일
- `frontend/src/main.tsx` (QueryClientProvider)
- 페이지 컴포넌트들에서 useQuery 사용 예시

---

## Day 13: 폼 처리 & 로그인/회원가입

### 📖 학습 목표
- React Hook Form 사용법
- 로그인 후 상태 업데이트

### ✍️ 직접 구현할 것

**1. LoginPage.tsx**
```typescript
export default function LoginPage() {
  const navigate = useNavigate();
  const { setAuth } = useAuthStore();
  
  const onSubmit = async (data: LoginForm) => {
    const response = await api.post('/auth/login', data);
    
    setAuth(
      { id: response.data.id, name: response.data.name, ... },
      response.data.accessToken,
      response.data.refreshToken
    );
    
    navigate('/');
  };
  
  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* 입력 필드들 */}
    </form>
  );
}
```

**2. 유효성 검사**
```typescript
const { register, handleSubmit, formState: { errors } } = useForm<LoginForm>({
  defaultValues: { email: '', password: '' }
});

// <input {...register('email', { required: '이메일을 입력하세요' })} />
```

### 🧠 핵심 개념 체크
- [ ] `useForm` hook 사용법
- [ ] `register`, `handleSubmit`, `formState`
- [ ] 로그인 성공 후 상태 업데이트 순서
- [ ] 에러 메시지 표시

### 📁 참고 파일
- `frontend/src/pages/LoginPage.tsx`
- `frontend/src/pages/SignupPage.tsx`

---

## Day 14: 고급 기능 & 최종 통합

### 📖 학습 목표
- i18n 다국어 지원
- 이미지 업로드
- 전체 통합 테스트

### ✍️ 직접 구현할 것

**1. i18n 설정**
```typescript
// src/lib/i18n.ts
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';
import LanguageDetector from 'i18next-browser-languagedetector';

import ko from '../locales/ko/translation.json';
import ja from '../locales/ja/translation.json';

i18n
  .use(LanguageDetector)
  .use(initReactI18next)
  .init({
    resources: { ko: { translation: ko }, ja: { translation: ja } },
    fallbackLng: 'ko',
  });
```

**2. 이미지 업로드 컴포넌트**
```typescript
const handleImageChange = (e: ChangeEvent<HTMLInputElement>) => {
  const file = e.target.files?.[0];
  if (file) {
    setImageFile(file);
    // Preview 생성
    const reader = new FileReader();
    reader.onload = () => setPreview(reader.result as string);
    reader.readAsDataURL(file);
  }
};
```

### 🧠 2주차 종합 체크리스트
- [ ] Zustand + persist 상태 관리
- [ ] Axios 인터셉터로 토큰 자동 처리
- [ ] TanStack Query로 서버 상태 관리
- [ ] 폼 처리 + 유효성 검사
- [ ] 다국어 지원 구현

### 📁 참고 파일
- `frontend/src/lib/i18n.ts`
- `frontend/src/locales/` 폴더
- `frontend/src/components/ImageUpload.tsx`

---

# 🏁 복습 완료 후 체크리스트

## Backend 역량
- [ ] Spring Boot 프로젝트 세팅부터 할 수 있다
- [ ] JPA Entity 관계 설계 가능
- [ ] JWT 인증 흐름 설명 가능
- [ ] REST API 설계 원칙 이해
- [ ] S3 파일 업로드 구현 가능

## Frontend 역량
- [ ] Vite + React + TypeScript 세팅 가능
- [ ] Zustand로 전역 상태 관리
- [ ] Axios 인터셉터 설정 가능
- [ ] TanStack Query 활용
- [ ] 인증 플로우 구현 가능

## 통합 역량
- [ ] 프론트-백엔드 연동 전체 흐름 이해
- [ ] CORS 문제 해결 가능
- [ ] 환경 변수 분리 관리
- [ ] Docker 기본 이해

---

> 💡 **팁**: 막힐 때는 1시간 정도 고민하고, 그래도 안되면 기존 코드를 참고하세요. 핵심은 "왜 이렇게 구현했는지" 이해하는 것입니다!
