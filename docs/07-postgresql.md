# PostgreSQL 학습 가이드

PostgreSQL은 단순 저장소가 아니라 데이터 무결성, 동시성, 검색, 분석의 중심입니다.

## 학습 순서

1. SQL(Structured Query Language) 기본
2. 스키마 설계
3. 제약 조건
4. 인덱스
5. 실행 계획
6. 트랜잭션과 격리 수준
7. 잠금과 교착 상태
8. 백업과 복구
9. 마이그레이션
10. 성능 튜닝

## 반드시 익힐 SQL(Structured Query Language)

- `SELECT`, `INSERT`, `UPDATE`, `DELETE`
- `JOIN`, `GROUP BY`, `HAVING`
- window function
- CTE
- `EXPLAIN`, `EXPLAIN ANALYZE`
- partial index
- unique constraint
- foreign key

## Spring과 연결되는 주제

- JPA(Java Persistence API) N+1 문제
- 영속성 컨텍스트와 트랜잭션
- 낙관적 잠금과 비관적 잠금
- 페이지네이션 성능
- schema migration
- connection pool 설정

## 연습 과제

1. 게시글, 댓글, 좋아요, 태그 스키마를 설계합니다.
2. 검색과 정렬 조건별 인덱스를 설계합니다.
3. `EXPLAIN ANALYZE`로 실행 계획을 비교합니다.
4. 동시에 좋아요를 누르는 상황을 재현합니다.
5. 중복 요청을 막는 DB(Database) 제약 조건을 추가합니다.
6. 마이그레이션 실패와 롤백 전략을 문서화합니다.

## 체크리스트

- [ ] 데이터 무결성을 애플리케이션 코드에만 맡기지 않는다.
- [ ] 인덱스를 추가하기 전 쿼리 패턴을 확인한다.
- [ ] 실행 계획을 읽고 병목을 설명할 수 있다.
- [ ] 트랜잭션 격리 수준의 차이를 예제로 설명할 수 있다.
- [ ] 운영 데이터 변경에는 백업과 롤백 계획이 있다.
