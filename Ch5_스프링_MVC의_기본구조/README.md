# Chapter 5 스프링 MVC의 기본 구조

## Spring MVC

- Spring은 코어 프레임워크에 여러 서브 프로젝트를 붙여 사용됨
- 원래 웹 앱을 목적으로 나온 프레임워크가 아님!
- Spring MVC도 이러한 서브 프로젝트중 하나

### 내부 구조

- `WebApplicationContext`에서 MVC 설정과 일반 설정이 분리됨
  - `ApplicationContext`

### Java 설정 이용

- `web.xml`, `servlet-context.xml`, `root-context.xml` 제거
- `pom.xml`에 `<plugin>` 태그 추가, `web.xml`이 없으므로.
- `GROUP_ID.config`(EX. `org.zerock.config`) 패키지 작성. `WebConfig.java`, `RootConfig.java`, `ServletConfig.java` 추가.

### 프로젝트 로딩 구조

- `web.xml` : Tomcat구동 관련설정
- `root-context.xml`, `servlet-context.xml` : Spring 관련설정

> 1. `web.xml` `<listener>` 태그의 `ContextLoaderListener`에서 프로젝트 구동시작.
> 2. `<context-param>` 태그에 등록된 `root-context.xml`을 처리, 그 내부의 Bean 설정들이 동작
> 3. 객체(Bean)들은 Spring의 영역(context)안에 생성됨. 객체 간의 의존성 처리.
> 4. `web.xml` `<servlet>` 태그의 `DispatcherServlet`(Servlet 관련 설정)동작
> 5. `DispatcherServlet` Class의 `XmlWebApplicationContext`가 `servlet-context.xml`을 로딩, 해석.
> 6. 이 과정에서 등록된 Bean들이 기존의 Bean들과 같이 연동.

### 기본 사상

- Spring MVC는 개발자들과 Servlet/JSP, 모델 2와 같은 기술을 분리함(신경쓰지 않게 함).
- 개발자들이 필요한 부분만을 집중할 수 있게 Servlet/JSP와 개발자 사이에 들어가 중재하는 계층이 됨.

### 모델 2와 Spring MVC

- Spring MVC는 모델 2 방식으로 처리됨.
- 로직과 화면을 분리하는 MVC 구조임.

> 1.  Request가 `DispatcherServlet`을 통해 처리.
> 2.  `HandlerMapping`이 해당 Request를 처리하는 Controller를 찾음. `@RequestMapping` Annotation이 그 기준이 됨.
> 3.  `HandlerAdapter`를 이용하여 해당 Controller 작동
> 4.  Controller가 개발자가 작성한 로직을 통해 Request를 처리 후, Model 객체에 데이터를 담아 View에 전달.
> 5.  Controller는 `ViewResolver`로 다양한 타입의 결과를 반환. 이는 어떤 View를 통해 처리하는 것이 좋을지 해석하는 역할.
> 6.  View는 데이터를 Jsp등을 통해 생성하는 역할. 만들어진 응답은 `DispatcherServlet`을 통해 전달.
