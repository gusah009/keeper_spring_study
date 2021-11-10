# Chaper 15 검색 처리

## MyBatis의 동적 SQL

<br/>

### `<if>`

- 특정 조건이 true가 되면 포함된 SQL을 사용

- <details>
  <summary>예시</summary>
  <div markdown="1">

  ```xml
  <if test="type == 'T'.toString()">
    (title like '%'||#{keyword}||'%')
  </if>
  <if test="type == 'C'.toString()">
    (content like '%'||#{keyword}||'%')
  </if>
  <if test="type == 'W'.toString()">
    (writer like '%'||#{keyword}||'%')
  </if>
  ```

  </div>
  </details>

### `<choose>`

- 여러 상황들 중 하나의 상황에서만 동작
- 위의 모든 조건이 충족되지 않을 경우에 `<otherwise>` 사용

- <details>
  <summary>예시</summary>
  <div markdown="1">

  ```xml
  <choose>
  <when test="type == 'T'.toString()">
    (title like '%'||#{keyword}||'%')
  </when>
  <when test="type == 'C'.toString()">
    (content like '%'||#{keyword}||'%')
  </when>
  <when test="type == 'W'.toString()">
    (writer like '%'||#{keyword}||'%')
  </when>
  <otherwise>
    (title like '%'||#{keyword}||'%' OR content like '%'||#{keyword}||'%')
  </otherwise>
  </choose>
  ```

  </div>
  </details>

### `<where>`

- 태그 안쪽에서 SQL이 생성될 때만 `WHERE` 구문이 붙는다

- <details>
  <summary>예시</summary>
  <div markdown="1">

  ```SQL
  select * from tbl_board
    <where>
      <if test="bno != null">
       bno = #{bno}
      </if>
    </where>
  ```

  </div>
  </details>

### `<trim>`

- 하위에서 만들어지는 SQL문을 조사하여 앞에 추가적인 SQL을 넣는다
- `prefix`, `suffix`, `prefixOverrides`, `suffixOverrides` 속성을 지정할수 있다

- <details>
  <summary>예시</summary>
  <div markdown="1">

  ```SQL
  select * from tbl_board
    <where>
      <if test="bno != null">
        bno = #{bno}
      </if>
      <trim prifix = "and">
        rownum = 1
      </trim>
    </where>
  ```

  </div>
  </details>

### `<foreach>`

- List, 배열, 맵 등을 이용해서 루프를 처리할 수 있다.

- <details>
  <summary>예시</summary>
  <div markdown="1">

  ```java
  Map<String,String> map = new HashMap<>();
  map.put("T","TTTT");
  map.put("C","CCCC");
  ```

  ```SQL
  select * from tbl_board
    <trim prefix="where (" suffix=")" prefixOverrides="OR">
      <foreach item="val" index="key" collection="map">

        <trim prefix = "OR">
          <if test="key == 'C'.toString()">
            contest = #{val}
          </if>
          <if test="key == 'T'.toString()">
            title = #{val}
          </if>
          <if test="key == 'W'.toString()">
            writer = #{val}
          </if>
        </trim>
      </foreach>
    </trim>
  ```

  </div>
  </details>

<br/>

## 검색 조건 처리

<br/>

- `Criteria`에 `type`(검색조건), `keyword`(키워드) 변수 추가
- `BoardMapper.xml` 의 `getListWithPaging()`을 수정하여 동적 SQL 처리
- `<sql>` 과 `<include>` 를 사용하여 동일한 SQL의 일부를 재사용
- 화면에서는 `<option>` 으로 검색 조건을 고르고 `<input>` 으로 키워드 입력

<br/>

## UriComponentsBuilder를 이용하는 링크 생성

<br/>

- 여러 개의 파라미터들을 연결해서 URL의 형태로 만들어준다

```java
public String getListLink() {
  UriComponentsBuilder builder = UriComponentsBuilder.fromPath("")
    .queryParam("pageNum", this.pageNum)
    .queryParam("amount", this.getAmount())
    .queryParam("type", this.getType())
    .queryParam("keyword", this.getKeyword());

  return builder.toUriString();
}
```

만약

```java
Criteria cri = new Criteria();
cri.setPageNum(3);
cri.setAmount(20);
cri.setKeyword("abc");
cri.setType("TC");
```

이면 <br/>
`?pageNum=3&amount=20&type=TC&keyword=abc` 가 return 된다.
