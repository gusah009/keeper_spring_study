# 스프링 9장

# 비즈니스 계층

#### 비즈니스 계층?
![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbRKzjn%2FbtqHoqiwtuG%2Fkm0VKjT5YI65M6kjyiQVV1%2Fimg.png)

###### 고객의 요구 사항을 반영하는 계층
영속 계층이 **DB**를 기준으로 구현하지만, 비즈니스 계층은 **로직**을 기준으로 처리

일반적으로 비즈니스 영역의 객체들은 __서비스(Service)__ 라는 용어를 사용하고, spring에서 구현할때에도 해당 annotation을 사용해주어야 인식이 된다.

![image](https://user-images.githubusercontent.com/43947871/136182617-324a0a61-52bd-4ecc-9abf-232d2f69351d.png)

#### 특징

1. 프레젠테이션 계층과 데이터 엑세스 계층(= 영속 계층 Persistence Tier)사이를 연결하는 역할, 두 계층이 직접적으로 통신 X
2. **트랜잭션** 처리
3. 다른 계층들과 통신하기 위한 인터페이스 제공
4. 애플리케이션 비즈니스 로직 처리와 관련된 도메인 모델의 적합성 검증
<hr/>

## 트랜잭션?
어떠한 일련의 작업을 의미.
이 일련의 작업들은 모두 에러가 없이 끝나야하는데, 만약 에러가 발생했을때, 그 지점 이전까지의 내용은 모두 복구되어야 함.
-> 데이터에 대한 무결성을 유지하기 위한 처리 방법 = 트랜잭션 처리
- source: https://freehoon.tistory.com/110

해당 내용은 책 19장에서 자세히 다룸
