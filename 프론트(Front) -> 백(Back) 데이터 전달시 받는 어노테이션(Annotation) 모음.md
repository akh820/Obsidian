###  방식별 백엔드 어노테이션

|프론트 전송 방식|예시|백엔드|
|---|---|---|
|JSON body|`Content-Type: application/json` + body|`@RequestBody`|
|URL 쿼리스트링|`/search?keyword=abc`|`@RequestParam`|
|URL 경로|`/users/123`|`@PathVariable`|
|Form 데이터|`Content-Type: multipart/form-data`|`@ModelAttribute`|
