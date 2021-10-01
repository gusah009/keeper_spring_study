# 스프링 7장

# 스프링 MVC 프로젝트의 기본 구성

웹 프로젝트는 보통 3-tier(티어) 방식으로 구성.

각 부분에 유지보수가 용이하기 위해 사용함.

![Untitled](https://user-images.githubusercontent.com/26597702/135571696-a970b132-ae7b-4d05-b971-d7d83a8eb69f.png)

1. Presentation Tier
    
    화면에 보여주는 기술을 사용하는 영역. Servlet/JSP나 MVC가 담당하는 영역임.
    
2. Business Tier
    
    순수한 비즈니스 로직을 담고 있는 영역.
    
3. Persistence Tier
    
    데이터를 어떤 방식으로 보관하고 사용하는가에 대한 설계가 들어가는 영역.
    

### MVC를 3tier에 대입했을 때

![Untitled 1](https://user-images.githubusercontent.com/26597702/135571604-6cc17c37-e77d-4876-a7e4-f331a8cc9b37.png)

### MVC 모델과 3tier 방식의 차이점?

mvc모델은 **디자인 패턴**의 일종이고, 3tier는 **소프트웨어 아키택처**의 일종이다.

![Untitled 2](https://user-images.githubusercontent.com/26597702/135571565-a32489b9-4728-4d10-a08f-6ffaf904218d.png)

![Untitled 3](https://user-images.githubusercontent.com/26597702/135571635-17262174-f668-4a1e-ab03-10f1dff443a3.png)

출처: [http://www.tonymarston.net/php-mysql/3-tier-architecture.html](http://www.tonymarston.net/php-mysql/3-tier-architecture.html)

## Naming Convention (명명 규칙)

- xxxController : 스프링 MVC에서 동작하는 Controller 클래스
- xxxService, xxxServiceImpl : 비즈니스를 담당하는 인터페이스는 'xxxService'를 이용하고, 인터페이스를 구현한 클래스는 'xxxServiceImpl'이라는 이름을 사용함.
- xxxDAO, xxxRepository : DAO(Data Access Object)라는 이름으로 데이터 보관 영역을 따로 구성함. 하지만 이 책에서는 MyBatis의 Mapper 인터페이스를 사용함.
- VO, DTO : 데이터를 담고 있는 객체. 차이점은 VO는 Read-Only 목적이 강함.

### 패키지의 Naming Convention

**org.zerock**

- config : 프로젝트 설정 관련
- controller : 컨트롤러
- service : 서비스 & 서비스 구현 클래스
- domain : VO, DTO 클래스
- persistence : MyBatis Mapper 인터페이스
- exception : 예외 처리
- aop : AOP 관련
- security : Security 관련
- util : 각종 유틸리티 클래스 관련

## 프로젝트를 위한 요구사항

요구사항은 온전한 문장으로 정리하는 것이 좋음

*Ex) 고객은 새로운 게시물을 등록할 수 있어야 한다.*

### 요구사항 화면 설계

목업과 함께 설계하는 경우가 많다

각 화면을 설계하는 단계에서 사용자가 입력해야 하는 값과 함께 GET/POST 방식에 대해 언급해 두는 것이 좋다.

![Untitled 4](https://user-images.githubusercontent.com/26597702/135571647-6a45ce03-7991-4845-8ebe-d728e164e8a3.png)
