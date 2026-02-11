![[Pasted image 20260207205114.png]]
###### Table 
- https://dbdiagram.io/d
Table users {

id Long [pk, increment]

username String [unique, not null]

password String [not null]

updated_at LocalDateTime

created_at LocalDateTime

}

  

Table posts {

id Long [pk, increment]

title String [not null, note: 'max 100']

content String [not null, note: 'TEXT']

user_id Long [not null, ref: > users.id]

created_at LocalDateTime

updated_at LocalDateTime

}

  

Table comments {

id Long [pk, increment]

content String [not null]

user_id Long [not null, ref: > users.id]

post_id Long [not null, ref: > posts.id]

updated_at LocalDateTime

created_at LocalDateTime

}

SpringSecurity 비활성화
[[Client -> Spring Boot HTTP 요청 처리순서]]

[[Js + React 설치(Vite)]]
