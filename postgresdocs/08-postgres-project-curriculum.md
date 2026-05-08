# PostgreSQL 프로젝트 커리큘럼

PostgreSQL 실력은 SQL(Structured Query Language) 문법 암기가 아니라 데이터 모델링, 무결성, 성능, 동시성을 실제로 다루는 능력입니다.

## 1단계: SQL(Structured Query Language) 기본 데이터셋

주제:

- 사용자, 게시글, 댓글 테이블

필수 적용:

- select
- insert
- update
- delete
- where
- order by
- limit

완료 기준:

- 필요한 열만 조회합니다.
- update와 delete 전에 대상 행 수를 확인합니다.
- null 조건을 올바르게 처리합니다.

## 2단계: 스키마와 제약 조건

주제:

- 회원 가입과 게시글 작성 모델

필수 적용:

- PK(Primary Key)
- FK(Foreign Key)
- not null
- unique
- check
- default

완료 기준:

- 중복 이메일을 DB가 막습니다.
- 존재하지 않는 사용자로 게시글을 만들 수 없습니다.
- 가격이나 수량이 음수가 되지 않습니다.

## 3단계: 복잡한 조회

주제:

- 게시글 목록과 통계

필수 적용:

- join
- left join
- group by
- having
- CTE(Common Table Expression)
- window function

완료 기준:

- 댓글 수가 포함된 게시글 목록을 조회합니다.
- 사용자별 최신 게시글을 구합니다.
- 중복이 생긴 이유를 설명할 수 있습니다.

## 4단계: 인덱스와 실행 계획

주제:

- 검색과 페이지네이션 최적화

필수 적용:

- B-Tree index
- composite index
- partial index
- `EXPLAIN ANALYZE`
- keyset pagination

완료 기준:

- 인덱스 추가 전후 실행 계획을 비교합니다.
- offset pagination의 한계를 재현합니다.
- 인덱스가 쓰이지 않는 이유를 하나 이상 설명합니다.

## 5단계: 트랜잭션과 동시성

주제:

- 재고 차감과 쿠폰 발급

필수 적용:

- transaction
- row lock
- optimistic locking
- unique constraint
- idempotency key

완료 기준:

- 동시 요청으로 재고가 음수가 되지 않습니다.
- 같은 쿠폰이 중복 발급되지 않습니다.
- 실패 시 재시도 가능한 오류와 아닌 오류를 구분합니다.

## 6단계: 운영 훈련

주제:

- 운영 DB(Database) 점검과 마이그레이션

필수 적용:

- role과 권한
- 백업
- 복구
- migration
- long running query 확인
- connection pool 계산

완료 기준:

- 백업 파일로 새 DB를 복구할 수 있습니다.
- 애플리케이션 계정에 최소 권한만 부여합니다.
- 마이그레이션 배포 순서를 문서화합니다.
