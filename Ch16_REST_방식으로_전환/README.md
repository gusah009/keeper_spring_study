# Chaper 16 REST 방식으로 전환

## REST

- Representational State Transfer
- 하나의 URI는 하나의 고유한 Resource를 대표
- 이에 대한 처리는 `GET`/`POST` 방식과 같은 추가적인 정보를 통해 결정
- ex) `/boards/123`은 123번 게시판만을 대표하며, `GET`방식을 통해 내부 게시물에 접근가능

## Annotations

### `@RestController`

- 해당 Controller가 REST방식으로 처리함을 의미

### `@ResponseBody`

- 데이터 전달 용도

### `@PathVariable`

- URL 경로에 있는 값을 파라미터로 추출

### `@CrossOrigin`

- Ajax의 크로스 도메인 문제를 해결

### `@RequestBody`

- Json 데이터를 원하는 타입으로 처리
  <br/>

## RestController

- REST방식은 순수 데이터를 전달, View로 전달하지 않음
- 기존과는 다르게 다양한 포맷의 데이터 전달가능.(Json, XML)
- ResponseEntity를 통해 데이터와 http 헤더의 상태 메세지를 함께 반환.
- PathVariable : '?'뒤에 파라미터를 추가하는 기존 쿼리스트링 방식과는 달리, URL일부를 활용.

```java
@GetMapping("/product/{cat}/{pid}")
public String[] getPath(
  @PathVariable("cat") String cat,
  @PathVariable("pid") Integer pid
  // ...
)
```

## URL vs URI

- Uniform Resource Locator / Identifier
- 혼동해서 사용해도 문제될 것 없음.
- URI가 식별자 역할이 강함.
