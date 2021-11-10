# Chapter 14 페이징 화면 처리

## 페이징 처리에 필요한 정보

| 변수이름  | 저장하는 데이터                | 계산법                                  | 위치          |
| --------- | ------------------------------ | --------------------------------------- | ------------- |
| pageNum   | 현제 페이지 번호               |                                         | Criteria.java |
| amount    | 한 페이지에 출력하는 데이터 수 |                                         | Criteria.java |
| total     | 전체 데이터 수                 |                                         | PageDTO.java  |
| realEnd   | 끝 번호                        | (int)(Math.ceil((total \* 1.0)/amount)) | PageDTO.java  |
| endPage   | 현제 페이지의 끝 번호          | (int)(Math.ceil(page/10.0)) \* 10       | PageDTO.java  |
| startPage | 현제 페이지의 시작 번호        | endPage - 9                             | PageDTO.java  |
| prev      | 이전으로 이동 가능한지 여부    | startPage > 1                           | PageDTO.java  |
| next      | 다음으로 이동 가능한지 여부    | endPage < realEnd                       | PageDTO.java  |

<br/>

## 페이징 처리 , JSP에서 출력

- BoardController에서 pageMaker로 전달

```java
@GetMapping("/list")
public void list(Criteria cri, Model model) {

    log.info("list: " + cri);
    model.addAttribute("list", service.getList(cri));
    model.addAttribute("pageMaker", new PageDTO(cri, 123));
    //total은 임시로 123 대입
}
```

- JSP에서 EL(${})로 불러온다 ( <c:out>이 더 좋으나 간단하게 사용하기 위해서)
- 자바스크립트로 페이지 번호 이벤트를 처리한다

<br/>

## 페이지 이동 처리

- 조회 페이지 이동
  - /board/list? **pageNum=2&amount=10** 에서 /board/get? **pageNum=2&amount=10&** bno=2359285 로 이동
  - BoardController의 get() 메소드에 Criteria를 파라미터로 추가

<br/>

- 수정과 삭제 처리
  - BoardController의 modify(), remove() 메소드에 Criteria를 파라미터로 추가

<br/>

## 전체 데이터의 개수 처리

- SQL을 통해 전체 개수를 구한다

```SQL
select count(*) from tbl_board where bno > 0
```

- BoardService에 getTotal() 메소드를 추가하여 BoardController에서 호출

```java
@GetMapping("/list")
public void list(Criteria cri, Model model) {

    log.info("list: " + cri);
    model.addAttribute("list", service.getList(cri));

    int total = service.getTotal(cri);

    log. info("total: "+total);

    model.addAttribute("pageMaker", new PageDTO(cri,total));
}
```
