# Chapter 11 화면 처리

<br>

![스크린샷 2021-10-28 오후 9.37.36](/Users/woochang/Desktop/스크린샷 2021-10-28 오후 9.37.36.png)

- MVC 구성요소
  - Controller
    - 사실 서블릿, 클라이언트의 요청을 받아 분석한 후 알맞은 모델을 호출
    - 모델이 요청을 처리해 반환하면 클라이언트에게 결과를 보여주기 위해 JSP 선택
    - 전체 과정의 흐름 제어
  - Model
    - 데이터를 저장하거나 수정하거나 데이터베이스 연동과 같은 비즈니스 로직을 수행
    - 일반적으로 DAO나 VO로 되어 있음
  - View
    - 본질적으로 JSP
    - 화면 기능을 담당하여 모델에서 처리한 결과를 화면에 표시

<br>

#### includes 적용

- JSP를 작성할 때 마다 많은 양의 HTML 코드를 이용하는 것을 피하기 위해 JSP의 include 지시자를 활용해서 페이지 제작 시에 필요한 내용만을 작성할 수 있게 사전에 작업
- `header.jsp` : 페이지에서 핵심적인 부분이 아닌 영역 중에서 위쪽의 HTML 내용을 처리하기 위해 작성

<br>

#### Model 데이터 출력

- Model에 담긴 데이터를 JSTL을 이용해서 출력

- `JSTL` : 자바 서버 페이지 표준 라이브러리 (JavaServer Pages Standard Library)

  - JSP 페이지 내에서 자바 코드를 바로 사용하지 않고 로직을 내장하는 효율적인 방법을 사용

  - JSTL은 라이브러리이기 때문에 사용하기 전에 core를 추가

    ```jsp
    <%@ taglib uri="http://java.sun.com/jstl/core" prefix="c" %>
    ```
