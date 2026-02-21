기본 터미널 명령어

\l          -- DB 목록
\c osm      -- osm DB로 전환 (use osm 대신)
\dt         -- 테이블 목록
\d 테이블명  -- 테이블 구조 확인
\q          -- 종료


| Java          | Postgre   | 비고                                                                                    |
| ------------- | --------- | ------------------------------------------------------------------------------------- |
| Integer       | INT       |                                                                                       |
| LocalDateTime | TIMESTAMP |                                                                                       |
| String        | VARCHAR   | @Column(length = 255) <br>private String status; // → VARCHAR(255) (length 사용안할시 기본값) |
| String        | TEXT      | @Column(columnDefinition = "TEXT")<br>private String description;  // → TEXT로 지정      |
| Boolean       | BOOLEAN   |                                                                                       |
