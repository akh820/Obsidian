
>MySql 명령어 

- 버전 확인 및 설치 
```
mysql --version
```
- mysql 접속
```
sudo mysql -u root -p
```
- DataBase 생성
```
CREATE DATABASE resource_tracker CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```
- DataBase 삭제
```
DROP DATABASE resource_tracker_db;
```

**root 계정과 슈퍼계정(모든권한을 부여받음)의 차이는?**
root는 ( unix_socker, Mac 시스템 사용자 검증) 접속 방식(비밀번호로 접속)만 다르고, sudo 없이 편하게 쓰기 위해서

- 사용자 생성 및 권한 부여 예시
```
-- 새 사용자 생성 + 비밀번호 설정
CREATE USER 'dev'@'localhost' IDENTIFIED BY '1234';

-- 모든 권한 부여
GRANT ALL PRIVILEGES ON resource_tracker.* TO 'dev'@'localhost';

-- 슈퍼 관리자 (모든 DB, 권한 부여 가능)
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;

-- 적용
FLUSH PRIVILEGES;

-- 확인
SELECT user, host FROM mysql.user;

exit;
```

호스트 = 접속 주소
사용자 = ID
둘다 일치해야만 함
`GRANT 권한들 ON 범위 TO '사용자'@'호스트';`
