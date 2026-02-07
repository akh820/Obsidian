#Java #MySql 
### 1️⃣ 숫자 타입

| Java       | MySQL    | 범위                                                     | 설명                |
| ---------- | -------- | ------------------------------------------------------ | ----------------- |
| Long       | BIGINT   | -9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807 | **약 922경** (64비트) |
| Integer    | INT      | -2,147,483,648 ~ 2,147,483,647                         | **약 21억** (32비트)  |
| Short      | SMALLINT | -32,768 ~ 32,767                                       | **약 3만** (16비트)   |
| Byte       | TINYINT  | -128 ~ 127                                             | **256개** (8비트)    |
| Double     | DOUBLE   | 소수점 15자리                                               | 부동소수점             |
| Float      | FLOAT    | 소수점 7자리                                                | 부동소수점             |
| BigDecimal | DECIMAL  | 정확한 소수                                                 | 금액 계산용            |

---

### 2️⃣ 문자열 타입

| Java   | MySQL        | 최대 길이                | 설명                |
| ------ | ------------ | -------------------- | ----------------- |
| String | VARCHAR(255) | 255자                 | 기본값               |
| String | VARCHAR(n)   | n자                   | @Column(length=n) |
| String | TEXT         | 65,535자 (64KB)       | 긴 글               |
| String | LONGTEXT     | 4,294,967,295자 (4GB) | 엄청 긴 글            |

---

### 3️⃣ 날짜/시간 타입

| Java          | MySQL     | 범위                      | 설명           |
| ------------- | --------- | ----------------------- | ------------ |
| LocalDateTime | DATETIME  | 1000-01-01 ~ 9999-12-31 | 날짜+시간(초까지)   |
| LocalDate     | DATE      | 1000-01-01 ~ 9999-12-31 | 날짜만          |
| LocalTime     | TIME      | -838:59:59 ~ 838:59:59  | 시간만          |
| Instant       | TIMESTAMP | 1970-01-01 ~ 2038-01-19 | UTC 기준 타임스탬프 |
LocalDateTime 사용시 마이크로초까지 원할 시
```Java
@Column(columnDefinition = "DATETIME(6)")
private LocalDateTime createdAt;
```

---

### 4️⃣ 불린/기타

|Java|MySQL|값|설명|
|---|---|---|---|
|Boolean|TINYINT(1)|0 또는 1|true/false|
|Enum|VARCHAR|문자열|Enum.name()|
|byte[]|BLOB|바이너리|이미지/파일|