# 12장 : 오라클 데이터베이스 페이징 처리

## 실행 계획
- 실행 계획(execution plan)
  <br>SQL을 데이터베이스에서 처리하는 방식.
- 데이터베이스에서 SQL이 처리되는 과정:
    1. SQL 파싱
        <br>SQL 구문에 오류가 있는지, SQL을 실행해야 하는 대상 객체(테이블, 제약 조건, 권한 등)가 존재하는지 검사.
    2. SQL 최적화
        <br>SQL이 실행되는데 필요한 비용(cost)를 계산. 
        <br>이 계산된 값을 기초로 해서 어떤 방식으로 실행하는 것이 가장 좋다는 것을 판단하는 실행 계획(execution plan)을 세우게 됨.
    3. SQL 실행
        <br>세워진 실행 계획을 통해서 메모리상에서 데이터를 읽거나, 물리적인 공간에서 데이터를 로딩하는 작업을 하게 됨.
<p align=center>
<img src="https://user-images.githubusercontent.com/36250213/140465319-2bc5da51-0a67-4a16-b931-258095727b72.png"/>
</p>

## 인덱스, RowID
- 인덱스를 활용
    - PK == 식별자 == index
    - 이미 정렬된 index를 이용해서 정렬 비용을 최소화.
    - 인덱스(pk_board)는 항상 정렬되어 있고, 실제 테이블은 순서가 뒤죽박죽 되어 있음.
        <br>⇒ 인덱스와 실제 테이블을 ROWID를 이용해서 연결.
        <p align=center>
        <img src="https://user-images.githubusercontent.com/36250213/140456993-c1967d7f-cfc7-4a85-a5eb-84d49421d89e.png" width="400px"/>
        </p>

## ROWID, ROWNUM

- ROWID : 데이터베이스 내의 (물리) 주소에 해당됨. 데이터가 입력될 때 자동으로 부여된다.
    - ROWID 구조
      |구조|	길이|	설명|
      |----|----|----|
      |오브젝트 번호|	1~6|	오브젝트(Object) 별로 유일한 값을 가지고 있으며, 해당 오브젝트가 속해 있는 값이다.|
      |상대 파일 번호|	7~9|	테이블스페이스(Tablespace)에 속해 있는 데이터 파일에 대한 상대 파일번호이다.|
      |블록 번호|	10~15|	데이터 파일 내부에서 어느 블록에 데이터가 있는지 알려준다.|
      |데이터 번호|	16~18|	데이터 블록에 데이터가 저장되어 있는 순서를 의미한다.|
    - 위 표와 같이, 데이터가 어떤 데이터 파일, 어느 블록에 저장되어 있는지 알 수 있다.

- ROWNUM : Select 결과 집합에 대한 가상의 순번, 테이블에서 데이터를 가져올 때 붙는 (논리) 번호. 
    <br>정렬 후 추출하는 경우와 추출 후 정렬을 할 경우의 결과 ROWNUM의 순서는 다르다.

## 오라클 힌트(hint)
- 힌트(hint): 데이터베이스에 select문을 어떻게 처리하고 싶은지 알려주는 것.
    <br>(힌트 구문 시작: `/*+`, 마무리: `*/`)
    - 힌트를 사용하지 않았을 때:
        ```java
        select * from tbl_table order by bno desc;
        ```
        => 데이터베이스의 상황에 따라서 인덱스가 아니라 전체 테이블을 정렬하는 방식으로 동작할 수도 있음.
        <br><br>
    - 힌트를 사용했을 때:
        ```java
        select /*+INDEX_DSC (tbl_board pk_board) */* from tbl_board;
        ```
        ⇒ 개발자가 데이터베이스가 어떻게 움직여야 하는지 명시해줌.
        <br>⇒ order by 조건이 없어도 동일한 결과가 나옴.
        <br>⇒ 힌트 : tbl_board 테이블에 pk_board 인덱스를 역순으로 이용해달라
            
- 힌트 문법 종류
    - FULL 힌트
        <br>테이블 전체를 스캔할 것으로 명시함.
        - 예시: `select /*+ FULL(tbl_board) */ * from tbl_board order by bno desc;`
          <br>→ TBL_BOARD를 FULL로 스캔 + sort가 적용됨. (느림)
        
    - INDEX_ASC, INDEX_DSC 힌트
        <br>인덱스를 순서대로/역순으로 이용함.
        <br>주로 order by를 위해서 사용됨. (sort과정을 생략하기 위한 용도)
        - 예시: `select /*+ INDEX_ASC(tbl_board pk_board) */ * from tbl_board where bno > 0;`
          <br>→ pk_board로 접근 + rowid를 사용해서 tbl_board에 접근 (빠름)

## 인라인 뷰 
- 인라인뷰(Inline-View)
    - 뷰(View) : 복잡한 SELECT처리를 하나의 뷰로 생성(`create view [뷰이름]`)하고, 사용자들은 뷰를 통해서 복잡하게 만들어진 결과를 마치 하나의 테이블처럼 쉽게 조회할 수 있음.
    - 인라인뷰 : 이러한 뷰의 작성을 별도로 하지 않고, FROM 구문 안에 바로 작성하는 형태

## 페이징 처리
- 페이징 처리
  <br> 출력될 페이지의 데이터만 나눠서 가져오는 것.
  <br> 주로 페이지 번호를 이용, '계속 보기'의 형태로 구현하는 등의 방법으로 구현.
  
1. 1페이지에 나오는 데이터 10개 추출
    <br>⇒ 아래처럼 rownum을 활용.
    ```sql
    // 1페이지에 데이터 10개 추출할 때
    select /*+INDEX_DESC(tbl_board pk_board)*/  // 내림차순 정렬로 접근
        rownum rn, bno, title, content
    from
        tbl_board
    where rownum <= 10                          // 정렬 후 추출한 데이터 중, 상위 10개
    ```
    
2. 2페이지의 데이터 10개 추출 
    <br>`where rownum > 10 and rownum <= 20` 이렇게 접근하면 안됨.
    <br>⇒ where에 의해 데이터가 걸러짐. → 항상 데이터가 추출되는 rownum은 1일 것이기 때문에, 아무 결과도 나오지 않게 됨.
    <br>⇒ 따라서 인라인뷰를 사용해서 아래처럼 추출함.
    ```sql
    // 2페이지에 데이터 10개 추출하고 싶을 때
    select
        bno, title, content
    from(
        select /*+INDEX_DESC(tbl_board pk_board)*/ // 내림차순 정렬
            rownum rn, bno, title, content
        from
            tbl_board
        where rownum <= 20                      // 정렬 후 추출한 데이터 중, 상위 20개
        )
    where rn > 10;                              // 20개 중, 뒤쪽 10개를 뽑음
    ```

- 각 페이지에 나올 데이터 추출 방법 - 과정 정리
    1. 필요한 순서로 정렬된 데이터에 ROWNUM을 붙인다.
        <br>`select /*+ (hint) */ rownum rn, ..`
    2. 처음부터 해당 페이지의 데이터를 'rownum ≤ 30'과 같은 조건을 이용해서 구한다.
        <br>`where rownum <= 30`
    3. 구해놓은 데이터를 하나의 테이블처럼 간주하고, 인라인뷰로 처리한다.
        <br>`from ( .. )`
    4. 인라인뷰에서 필요한 데이터만을 남긴다.
        <br>`select from( .. ) where rn > 10`
