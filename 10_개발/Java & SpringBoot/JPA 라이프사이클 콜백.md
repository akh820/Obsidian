#jpa 
@PrePersist    // INSERT 직전
@PostPersist   // INSERT 직후

@PreUpdate     // UPDATE 직전
@PostUpdate    // UPDATE 직후

@PreRemove     // DELETE 직전
@PostRemove    // DELETE 직후

@PostLoad      // DB에서 조회한 직후

**라이프사이클이란?** 
엔티티가 생성되고 DB에 저장되고 수정되고 삭제되는 생명주기를 말합니다.
## 실무 사용 빈도

**자주 쓰는 것 (⭐⭐⭐):**

- `@PrePersist` - 생성 시간 자동 설정
- `@PreUpdate` - 수정 시간 자동 설정

**가끔 쓰는 것 (⭐):**

- `@PostLoad` - 조회 후 추가 처리 필요할 때

**거의 안 쓰는 것:**

- `@PostPersist` - INSERT 후 처리 (드물게)
- `@PreRemove` - 삭제 전 로깅
- `@PostRemove` - 삭제 후 처리
- `@PostUpdate` - 수정 후 처리