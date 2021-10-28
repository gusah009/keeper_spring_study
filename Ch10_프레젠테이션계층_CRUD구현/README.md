# Chapter 10 프레젠테이션(웹) 계층의 CRUD 구현

<br>

### Controller의 작성

|   Task    |       URL       | Method | Parameter |     From      | URL 이동 |
| :-------: | :-------------: | :----: | :-------: | :-----------: | :------: |
| 전체 목록 |   /board/list   |  GET   |           |               |          |
| 등록 처리 | /board/register |  POST  | 모든 항목 | 입력화면 필요 |   이동   |
|   조회    |   /board/get    |  GET   |  bno=123  |               |          |
| 삭제 처리 |  /board/remove  |  POST  |    bno    | 입력화면 필요 |   이동   |
| 수정 처리 |  /board/modify  |  POST  | 모든 항목 | 입력화면 필요 |   이동   |

- 원하는 기능을 호출하는 방식에 대해 다음과 같이 테이블로 정리한 후 코드를 작성하는 것이 효율적

<br>

#### BoardController

- `@Controller` : 스프링 빈으로 인식
- `@RequestMapping` : /board로 시작하는 모든 처리를 하도록 지정
- `@AllArgsConstructor` : 모든 필드 값을 파라미터로 가지는 생성자 생성

<br>

#### 기존과 다른 테스트

- 기존은 Tomcat과 같은 WAS(Web Application Server)를 실행하여 테스트 진행
- `@WebAppConfiguration` 
  - 통합 테스트를 위한 어플리케이션 컨텍스트가 `WebApplicationContext`여야 하는 경우 사용
  - `Mock` 서블릿 컨텍스트는 WebApplicationContext의 테스트에 사용되는 서블렛 컨텍스트 역할 수행
- `MockMvc` : 가짜 URL과 파라미터 등을 브라우저에서 사용하는 것처럼 만들어서 Controller 실행
  - `perform()` : 요청을 전송하는 역할, 결과로 ResultAction 객체를 받음
  - `MockMvcRequestBuilders` : 실제 요청 빌더를 만드는데 사용할 수 있는 정적 메서드 제공
    - ex) `get()`, `post()` 등
  - `andReturn()` : 테스트한 결과 객체를 받을 때 사용