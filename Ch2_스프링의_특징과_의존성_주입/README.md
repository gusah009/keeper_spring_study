# Chapter 2 스프링의 특징과 의존성 주입

## 특징

### POJO

- Plain Old Java Object
- 별도의 API/라이브러리 없이 일반적인 Java코드를 이용하여 객체 구성

### AOP

- 횡단관심사를 모듈로 분리
- 횡단 관심사 : 핵심 비즈니스 로직은 아니지만, 그래도 처리해야 하는 것들, 보안/로그 등
- AspectJ 문법을 통해 작성

### 트랜잭션 지원

- 트랜잭션 처리 : 데이터에 대한 무결성을 유지하기 위한 방법
- Annotation이나 XML로 설정가능

### 의존성 주입

- 미리 당겨오는 import와 달리 필요할 때 객체를 가져옴
- `root-context.xml`에서 지정된 태그의 패키지를 스캔
- `@Component`설정된 클래스의 객체를 생성
- 객체가 필요한 클래스에서의 `@Autowired` 설정을 보고 객체를 주입

## 기본 Annotations

### Lombok 관련

- `@Setter`
- `@Getter`
- `@Data`
- `@Log4j`

### Spring 관련

- `@Autowired`
- `@Component`

### Test 관련

- `@RunWith`
- `@ContextConfigure`
- `@Test`
