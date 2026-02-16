PostgreSQL과 MySQL의 기본 CRUD 쿼리는 거의 비슷해요. PostgreSQL 기준으로 설명드릴게요.

## CREATE (데이터 삽입)

```sql
-- 단일 행 삽입
INSERT INTO users (name, email, age) 
VALUES ('홍길동', 'hong@example.com', 25);

-- 여러 행 한번에 삽입
INSERT INTO users (name, email, age) 
VALUES 
  ('김철수', 'kim@example.com', 30),
  ('이영희', 'lee@example.com', 28);

-- PostgreSQL 전용: 삽입 후 결과 반환
INSERT INTO users (name, email, age) 
VALUES ('박민수', 'park@example.com', 32)
RETURNING id, name;
```

## READ (데이터 조회)

```sql
-- 전체 조회
SELECT * FROM users;

-- 특정 컬럼만 조회
SELECT name, email FROM users;

-- 조건부 조회
SELECT * FROM users WHERE age >= 25;

-- 정렬
SELECT * FROM users ORDER BY age DESC;

-- 페이징 (LIMIT/OFFSET)
SELECT * FROM users ORDER BY id LIMIT 10 OFFSET 20;

-- JOIN 예시
SELECT u.name, o.order_date, o.total_amount
FROM users u
INNER JOIN orders o ON u.id = o.user_id
WHERE u.age >= 25;

-- 집계 함수
SELECT COUNT(*) FROM users WHERE age >= 25;
SELECT AVG(age) as avg_age FROM users;
```

## UPDATE (데이터 수정)

```sql
-- 단일 행 수정
UPDATE users 
SET email = 'newemail@example.com' 
WHERE id = 1;

-- 여러 컬럼 수정
UPDATE users 
SET email = 'update@example.com', age = 26 
WHERE id = 1;

-- 조건에 맞는 여러 행 수정
UPDATE users 
SET age = age + 1 
WHERE age < 30;

-- PostgreSQL 전용: 수정 후 결과 반환
UPDATE users 
SET age = 35 
WHERE id = 1
RETURNING id, name, age;
```

## DELETE (데이터 삭제)

```sql
-- 특정 행 삭제
DELETE FROM users WHERE id = 1;

-- 조건에 맞는 여러 행 삭제
DELETE FROM users WHERE age < 20;

-- 전체 삭제 (주의!)
DELETE FROM users;

-- PostgreSQL 전용: 삭제 후 결과 반환
DELETE FROM users 
WHERE id = 1
RETURNING id, name;
```

## 테이블 생성 (보너스)

```sql
-- PostgreSQL
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  age INTEGER,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- MySQL (차이점: AUTO_INCREMENT)
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  age INT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

주요 차이점은 PostgreSQL의 `SERIAL`과 MySQL의 `AUTO_INCREMENT`, 그리고 PostgreSQL의 `RETURNING` 절 정도예요. 나머지 기본 CRUD는 거의 동일합니다. 그렇군요