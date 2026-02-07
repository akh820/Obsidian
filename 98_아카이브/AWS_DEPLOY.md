# AWS 배포 가이드 (S3 + EC2)

## 아키텍처
```
[사용자] → [S3] ← React 정적 파일
              ↓ API 호출
           [EC2 t3.micro]
              ├── Spring Boot (Docker)
              └── MySQL (Docker)
```

**예상 비용**: $100 크레딧으로 약 6-8개월 운영 가능

---

## 1️⃣ EC2 인스턴스 생성

### AWS Console에서:
1. **EC2** → **Launch Instance**
2. 설정:
   - Name: `equip-request-server`
   - AMI: **Ubuntu 22.04 LTS** (Free tier eligible)
   - Type: **t3.micro** (~$8/월)
   - Key pair: 새로 생성 → `.pem` 파일 다운로드
   - Security Group 규칙:
     | Type | Port | Source |
     |------|------|--------|
     | SSH | 22 | My IP |
     | HTTP | 80 | 0.0.0.0/0 |
     | Custom TCP | 8080 | 0.0.0.0/0 |
   - Storage: **20GB gp3**

---

## 2️⃣ EC2 서버 설정

```bash
# ============================================
# 1. SSH 접속
# ============================================
# -i: identity file (키 파일 지정)
# ubuntu: Ubuntu AMI의 기본 사용자명
# @뒤: EC2 인스턴스의 Public IP 주소
ssh -i ~/.ssh/equip-request-key.pem ubuntu@52.78.184.75

# ============================================
# 2. 시스템 패키지 업데이트
# ============================================
# apt update: 패키지 목록 최신화
# apt upgrade: 설치된 패키지들 최신 버전으로 업그레이드
# -y: 모든 질문에 자동으로 yes 응답
sudo apt update && sudo apt upgrade -y

# ============================================
# 3. Docker 및 필수 도구 설치
# ============================================
# docker.io: Docker 엔진 (컨테이너 실행 환경)
# docker-compose: 여러 컨테이너를 한번에 관리하는 도구
# git: 소스코드 버전 관리 및 GitHub에서 클론용
sudo apt install docker.io docker-compose git -y

# ============================================
# 4. Docker 서비스 시작 및 자동 시작 설정
# ============================================
# start: Docker 서비스 즉시 시작
# enable: 서버 재부팅 시 Docker 자동 시작
sudo systemctl start docker
sudo systemctl enable docker

# ============================================
# 5. Docker 권한 설정
# ============================================
# ubuntu 유저를 docker 그룹에 추가
# 이렇게 하면 sudo 없이 docker 명령어 사용 가능
sudo usermod -aG docker ubuntu

# ============================================
# 6. 재접속 (그룹 변경 적용)
# ============================================
# 그룹 변경은 새 세션에서만 적용됨
# exit로 나간 후 다시 SSH 접속 필요
exit
ssh -i ~/.ssh/equip-request-key.pem ubuntu@52.78.184.75
```

---

## 3️⃣ 백엔드 배포

```bash
# ============================================
# 1. 프로젝트 클론
# ============================================
# GitHub에서 소스코드 다운로드
git clone https://github.com/akh820/equip-request.git
cd equip-request

# ============================================
# 2. 환경변수 파일 생성
# ============================================
# .env.example을 복사해서 .env 파일 생성
# .env 파일에는 비밀번호, API 키 등 민감한 정보 저장
cp .env.example .env

# nano 편집기로 .env 파일 열기
# Railway에서 사용하던 값들을 여기에 입력
# 저장: Ctrl+O → Enter, 종료: Ctrl+X
nano .env

# ============================================
# 3. Docker Compose로 서비스 시작
# ============================================
# up: 컨테이너 생성 및 시작
# -d: 백그라운드에서 실행 (detached mode)
# --build: 이미지 새로 빌드 (코드 변경 시 필수)
docker-compose up -d --build

# ============================================
# 4. 로그 확인 (문제 발생 시 디버깅용)
# ============================================
# logs: 컨테이너 로그 출력
# -f: 실시간으로 로그 계속 출력 (follow)
# backend: 특정 서비스만 로그 확인
# 종료: Ctrl+C
docker-compose logs -f backend
```

### 동작 확인:
```bash
# API 테스트 - 200 OK 또는 JSON 응답 오면 성공
curl http://localhost:8080/api/equipments
```

---

## 4️⃣ 프론트엔드 빌드 & S3 배포

### 로컬에서 실행 (본인 맥에서):

```bash
# 프론트엔드 폴더로 이동
cd frontend

# ============================================
# 환경변수 설정
# ============================================
# VITE_API_URL: 프론트엔드가 API 호출할 백엔드 주소
# YOUR_EC2_PUBLIC_IP를 실제 EC2 IP로 변경
# 예: http://52.78.184.75:8080/api
echo "VITE_API_URL=http://52.78.184.75:8080/api" > .env.production

# ============================================
# 프로덕션 빌드
# ============================================
# dist/ 폴더에 최적화된 정적 파일 생성
npm run build
```

### S3 버킷 생성 (AWS Console):
1. **S3** → **Create bucket**
   - Name: `equip-request-frontend` (전 세계 유일해야 함)
   - Region: `ap-northeast-2` (서울)
2. **Properties** → **Static website hosting** → Enable
   - Index document: `index.html` (메인 페이지)
   - Error document: `index.html` (SPA 라우팅용, 404시에도 index.html로)
3. **Permissions** → **Block public access** → 모두 해제 (퍼블릭 접근 허용)
4. **Bucket policy** 추가 (버킷 정책 탭에서):
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::equip-request-frontend/*"
  }]
}
```
> ⚠️ `equip-request-frontend`를 본인 버킷명으로 변경!

### 파일 업로드:
```bash
# ============================================
# AWS CLI로 S3에 업로드
# ============================================
# sync: 변경된 파일만 업로드 (효율적)
# --delete: S3에만 있고 로컬에 없는 파일 삭제
aws s3 sync dist/ s3://equip-request-frontend --delete

# 또는 AWS Console에서 dist/ 폴더 내용 직접 드래그&드롭
```

---

## 5️⃣ 접속 확인

| 서비스 | URL |
|--------|-----|
| **Frontend** | `http://equip-request-frontend.s3-website.ap-northeast-2.amazonaws.com` |
| **Backend API** | `http://YOUR_EC2_PUBLIC_IP:8080/api` |
| **Swagger** | `http://YOUR_EC2_PUBLIC_IP:8080/swagger-ui.html` |

---

## 🔧 유용한 명령어

```bash
# ============================================
# 로그 관련
# ============================================
docker-compose logs -f              # 전체 로그 실시간 확인
docker-compose logs -f backend      # 백엔드만 로그 확인
docker-compose logs --tail=100      # 최근 100줄만 보기

# ============================================
# 컨테이너 관리
# ============================================
docker-compose restart              # 전체 재시작
docker-compose restart backend      # 백엔드만 재시작
docker-compose down                 # 전체 중지 (컨테이너 삭제)
docker-compose up -d                # 다시 시작
docker-compose up -d --build        # 코드 변경 후 재빌드 & 시작

# ============================================
# 상태 확인
# ============================================
docker-compose ps                   # 실행 중인 컨테이너 목록
docker stats                        # CPU/메모리 사용량 실시간

# ============================================
# MySQL 관련
# ============================================
# MySQL 컨테이너 접속 (비밀번호는 .env 파일에 설정한 값)
docker exec -it equip-mysql mysql -u root -p

# 데이터베이스 백업 (로컬에 backup.sql 파일 생성)
docker exec equip-mysql mysqldump -u root -p equipdb > backup.sql

# 백업 파일로 복원
docker exec -i equip-mysql mysql -u root -p equipdb < backup.sql
```

---

## 6️⃣ CI/CD 설정 (GitHub Actions)

### GitHub Secrets 설정:
Repository → Settings → Secrets and variables → Actions → New repository secret

#### 🔐 Backend 환경변수 (자동으로 .env 파일 생성됨)
| Secret Name | 설명 | 예시 값 |
|-------------|------|---------|
| `MYSQL_ROOT_PASSWORD` | MySQL 루트 비밀번호 | `your_secure_password` |
| `JWT_SECRET` | JWT 시크릿 키 (32자 이상) | `your-256-bit-secret-key...` |
| `JWT_ACCESS_EXPIRATION` | 액세스 토큰 만료 (ms) | `3600000` |
| `JWT_REFRESH_EXPIRATION` | 리프레시 토큰 만료 (ms) | `604800000` |
| `AWS_S3_ACCESS_KEY` | S3용 IAM Access Key | `AKIAIOSFODNN7EXAMPLE` |
| `AWS_S3_SECRET_KEY` | S3용 IAM Secret Key | `wJalrXUtnFEMI...` |
| `AWS_S3_REGION` | S3 리전 | `ap-northeast-2` |
| `AWS_S3_BUCKET` | S3 버킷명 (이미지 업로드용) | `equip-images` |

#### 🚀 CI/CD용 Secrets
| Secret Name | 설명 | 예시 값 |
|-------------|------|---------|
| `EC2_HOST` | EC2 Public IP | `52.78.184.75` |
| `EC2_SSH_KEY` | .pem 파일 내용 전체 복사 | `-----BEGIN RSA PRIVATE KEY-----...` |
| `AWS_ACCESS_KEY_ID` | CI/CD용 IAM Access Key | `AKIAIOSFODNN7EXAMPLE` |
| `AWS_SECRET_ACCESS_KEY` | CI/CD용 IAM Secret Key | `wJalrXUtnFEMI...` |
| `S3_BUCKET_NAME` | Frontend S3 버킷명 | `equip-request-frontend` |
| `VITE_API_URL` | Backend API URL | `http://52.78.184.75:8080/api` |

### 동작 방식:
- `backend/**` 변경 → EC2에 자동 배포
- `frontend/**` 변경 → S3에 자동 업로드
- 둘 다 변경 → 둘 다 배포

---

## ⚠️ 주의사항

1. **EC2 Public IP 변경**: EC2 중지/시작하면 IP 바뀜 → Elastic IP 할당 권장 (무료)
2. **보안**: 8080 포트 대신 nginx로 80 포트 프록시 권장
3. **백업**: 주기적으로 MySQL 백업 필수

---

## 🆘 문제 해결

### Docker 권한 에러
```bash
# "permission denied" 에러 발생 시
sudo usermod -aG docker ubuntu
exit  # 재접속 필요
```

### 컨테이너가 계속 재시작될 때
```bash
# 로그 확인
docker-compose logs backend

# 환경변수 확인
cat .env
```

### MySQL 연결 안 될 때
```bash
# MySQL 컨테이너 상태 확인
docker-compose ps

# MySQL 로그 확인
docker-compose logs mysql
```
