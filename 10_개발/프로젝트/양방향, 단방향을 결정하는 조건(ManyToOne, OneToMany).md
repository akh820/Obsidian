단방향, ManyToOne 만 cascade 가능
[[양방향 예시 코드]]
- [[양방향 기준]]

| 기능                     | 양방향 (현재)           | 단방향 (OneToMany 제거)       |
| ---------------------- | ------------------ | ------------------------ |
| **request.getItems()** | ✅ 가능               | ❌ 불가능                    |
| **cascade 저장**         | ✅ 자동               | ❌ 수동 저장 필요               |
| **items 조회**           | request.getItems() | itemRepository.findBy... |
| **코드 복잡도**             | 약간 높음              | 낮음                       |
| **DB 쿼리**              | 동일                 | 동일                       |

// 1:N 관계 기본 전략

// 부모-자식 생명주기가 같고, 항상 함께 조회
→ 양방향 + cascade (Order ↔ OrderItem)

// 부모와 자식이 독립적이고, 가끔 조회
→ 단방향 (Post ↔ Comment는 단방향도 OK)

// 참조만 필요 (조회 거의 없음)
→ 단방향 (User ↔ EquipmentRequest)