
**주요 4가지:**

1. **IDENTITY** ⭐ - DB의 AUTO_INCREMENT 사용 (MySQL/MariaDB)
2. **SEQUENCE** ⭐ - DB 시퀀스 사용 (Oracle/PostgreSQL)
3. **TABLE** - 별도 테이블로 관리 (거의 안 씀)
4. **AUTO** - JPA가 자동 선택 (명시적으로 쓰는 게 좋음)

**실무에서:**

- MySQL/MariaDB → **IDENTITY** 사용
- Oracle/PostgreSQL → **SEQUENCE** 사용
- TABLE은 성능 나빠서 거의 안 씀

**당신 프로젝트:** MariaDB니까 `IDENTITY` 맞음 ✅