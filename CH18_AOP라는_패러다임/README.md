# Chaper 18 AOP라는 패러다임

## AOP란?

<br/>

- `Aspect Oriented Programming`(관점 지향 프로그래밍)
- 개발자가 염두에 둬야할 일들을 별도의 관심사로 분리하고, 비즈니스 로직에 집중
- `관심사 + 비즈니스 로직` 으로 분리하고 컴파일 혹은 실행과정에서 결합

<br/>

## AOP 용어들

<br/>

### `Target`

- 순수한 비즈니스 로직
- 어떠한 관심사들과도 관계를 맺지 않는다

### `Proxy`

- Target을 전체적으로 감싸고 있는 존재
- 내부적으로 Target을 호출
- 중간에 필요한 관심사들을 거쳐서 호출하도록 작성된다.
- 대부분의 경우 auto-proxy(자동 생성) 방식을 이용한다.

### `JoinPoint`

- Target 객체가 가진 메서드

### `Pointcut`

- 관심사와 비즈니스 로직이 결합되는 지점을 결정하는 것
- 다양한 형태로 선언하여 사용할수 있다
- <details>
  <summary>예시</summary>
  <div markdown="1">

  - `execution(@execution)` : 매서드를 기준으로 Pointcut을 설정
  - `within(@within)` : 특정한 타입(클래스)을 기준으로 Pointcut을 설정
  - `this` : 주어진 인터페이스를 구현한 객체를 대상으로 Pointcut을 설정
  - `args(@args)` : 특정한 파라미터를 가지는 대상들만을 Pointcut으로 설정
  - `@ennotation` : 특정한 어노테이션이 적용된 대상들만을 Pointcut으로 설정

  </div>
  </details>

### `Aspect`

- 관심사 자체를 의미하는 추상적인 개념

### `Advice`

- 실제 걱정거리를 분리해 놓은 코드
- XML을 이용한 설정과 어노테이션을 이용한 설정(스프링 3버젼 이후)
- 동작 위치에 따라 구분 된다
  - `Before Advice` : Target이 JoinPoint를 호출하기전에 실행
  - `After Returning Advice` : 모든 실행이 정상적으로 이루어진 후에 실행
  - `After Throwing Advice` : 예외가 발생한 뒤에 실행
  - `After Advice` : 정상적으로 실행되거나 예외가 발생했을 때 실행
  - `Around Advice` : 매서드의 실행 자체를 제어, 매서드를 호출하고 결과나 예외를 처리할수 있다
