# Chapter 6 스프링 MVC의 Controller

## 특징

- `HttpServletRequest`, `HttpServletResponse`를 거의 사용할 필요 없이 필요한 기능 구현.
- 다양한 타입의 파라미터, 리턴 타입 처리 및 사용 가능.
- `GET`, `POST`등 전송방식을 Annotation으로 처리 가능.
- 상속/인터페이스 방식을 Annotation으로 대용 가능

## @Controller, @RequestMapping

- `@Controller` : Controller Class를 Spring의 Bean으로 자동 등록.
- `@RequestMapping` : 현 Class들의 모든 Method의 기본 Url경로 설정.
- `@GetMapping`,`@PostMapping` : 각각 `GET`, `POST`방식을 구분할때 사용.

## Parameter 수집

- Controller의 Method에 DTO Class를 Parameter로 사용하게 하면, DTO의 field의 setter method가 동작하면서 수집.
- ex. `int userId` field를 DTO에 선언 후, `void setUserId(int)` 혹은 `@Setter` Annotation을 설정하면, 이 Method가 자동으로 동작.
- 동일한 이름의 파리미터는 `ArrayList<>`등으로 수집 가능.
- DTO Class를 여러 개 받으려면 DTOList Clas를 따로 선언.

## Model

- JSP와 같은 View에 Controller에서 생성된 데이터를 담아 전달하는 존재.
- Spring MVC Controller는 Java Beans의 규칙에 맞는 객체는 자동으로 화면으로 전달해줌.
- Java Beans 규칙 : 생성자가 없거나, 빈 생성자를 가지고, getter/setter를 가진 Class의 객체.
- 따라서 일반적인 DTO는 Controller의 Parameter에 입력되면 자동으로 화면까지 전달됨.
- `@ModelAttribute` : 전달받은 기본 자료형 Parameter를 수동으로 Model에 담음.
- `RedirectAttribute` : Model과는 달리 화면에 한 번만 사용되는 데이터만 전달

## Controller의 return type

- String : View의 파일경로와 파일이름을 나타냄.
- void : 호출하는 URL과 동일한 이름의 View.
- VO, DTO : JSON 타입으로 객체가 변환되어 반환.
- ResponseEntity : Http header 정보 response용도로 사용.
- HttpHeaders : 응답에 내용없이 Http header만 전달.
