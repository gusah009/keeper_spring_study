# 스프링 13장

## MyBatis 처리
- MyBatis에서의 페이징 처리: 
  - 인라인뷰를 이용하는 SQL문을 작성. 
  - 필요한 파라미터를 지정하는 방식으로 페이징 처리를 함.
    - 페이징 처리를 위해 필요한 파라미터 :
      - 페이지 번호
      - 한 페이지 당 보여줄 데이터 개수

- 복잡한 SQL 문은 xxxMapper.xml에서 관리
  - `where rn < 10` 과 같이 부등호 기호를 사용할 때, xml 태그와 맞물려서 문제가 발생.
    <br>⇒ CDATA 섹션을 사용
  - CDATA 섹션 : 섹션 안의 쿼리문을 파싱하지 않고, 그대로 문자열로 인식함.
    <br> `<![CDATA[ 쿼리문 ]]>` 이렇게 사용
    
- SQL에서 파라미터 받는 방법
  - 확장성을 위해 파라미터는 객체 하나에 몰아서 관리 (`Criteria`)
  - interface(java): `Criteria`라는 객체에 담아서 파라미터 전달
  - xml: #{파라미터}로 받음
  ```java
  // Criteria.java
  public class Criteria {
    private int pageNum;
    private int amount;

    public Criteria() {
      this(1, 10);
    }

    public Criteria(int pageNum, int amount) {
      this.pageNum = pageNum;
      this.amount = amount;
    }
  }
  ```
  ```java
  // BoardMapper.java
  //...
  public interface BoardMapper {
	  //...
	  public List<BoardVO> getListWithPaging(Criteria cri);
      //...
  }
  ```
  ```sql
  <!--BoardMapper.xml-->
  <select id="getListWithPaging" resultType="org.zerock.domain.BoardVO">
	<![CDATA[ 
	select bno, title, content, writer, regdate, updatedate 
	from ( 
		select /*+INDEX_DESC(tbl_board pk_board) */ rownum rn, bno, title, content, 
		writer, regdate, updatedate 
		from tbl_board 
		where rownum <= #{pageNum} * #{amount} 
	) 
	where rn > (#{pageNum} -1) * #{amount} 
	]]> 
  </select>
  ```
