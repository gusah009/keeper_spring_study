# Chaper 17 Ajax 댓글 처리

### 데이터베이스 인덱스 설계

- p.428 - 429 그림 참조

- `tbl_reply where bno = 200 order by rno asc`
- 이러한 형식으로 접근시, PK_REPLY 테이블을 통해 가져오므로 중간 게시판의 번호는 건너뜀
- 성능에 문제가 있을 수 있으므로, 정렬 후에 검색 필요.

- `create index idx_reply on tbl_reply (bno desc, rno asc)`
- IDX_REPLY에서 bno 컬럼이 정렬되어 있으므로, 200에 해당하는 범위만 찾아서 반환.
