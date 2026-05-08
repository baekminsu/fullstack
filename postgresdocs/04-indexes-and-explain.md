# 인덱스와 실행 계획

인덱스는 조회 성능을 높이는 자료구조입니다. 하지만 모든 컬럼에 인덱스를 만들면 쓰기 성능과 저장 공간이 나빠집니다. 인덱스는 쿼리 패턴을 보고 설계해야 합니다.

## B-Tree 인덱스

PostgreSQL의 기본 인덱스는 B-Tree입니다. 동등 비교와 범위 검색, 정렬에 널리 사용됩니다.

```sql
create index idx_posts_created_at
on posts (created_at);
```

자주 쓰는 조건:

```sql
where created_at >= '2026-01-01'
order by created_at desc
```

## 복합 인덱스

여러 컬럼을 함께 인덱스로 만듭니다.

```sql
create index idx_posts_status_created_at
on posts (status, created_at desc);
```

컬럼 순서가 중요합니다. `status`로 먼저 필터링하고 `created_at`으로 정렬하는 쿼리에 적합합니다.

## Unique index

```sql
create unique index uq_users_email
on users (email);
```

unique constraint와 비슷하게 중복을 막습니다. 부분 unique index처럼 더 섬세한 조건이 필요할 때 index로 직접 만들 수 있습니다.

## Partial index

조건을 만족하는 행만 인덱싱합니다.

```sql
create index idx_posts_published_created_at
on posts (created_at desc)
where status = 'PUBLISHED';
```

게시된 글만 자주 조회한다면 전체 게시글보다 작은 인덱스로 충분할 수 있습니다.

## Expression index

표현식 결과에 인덱스를 만듭니다.

```sql
create index idx_users_lower_email
on users (lower(email));
```

사용 쿼리:

```sql
select *
from users
where lower(email) = lower('A@EXAMPLE.COM');
```

## 인덱스가 항상 쓰이지 않는 이유

- 테이블이 너무 작아 순차 스캔이 더 빠릅니다.
- 조건 선택도가 낮습니다.
- 쿼리 표현식이 인덱스와 다릅니다.
- 통계가 오래되었습니다.
- 정렬 방향이나 컬럼 순서가 맞지 않습니다.

## EXPLAIN

실행 계획을 확인합니다.

```sql
explain
select *
from posts
where status = 'PUBLISHED'
order by created_at desc
limit 20;
```

실제 실행까지 확인:

```sql
explain analyze
select *
from posts
where status = 'PUBLISHED'
order by created_at desc
limit 20;
```

운영 DB에서 `explain analyze`는 실제 실행을 하므로 update, delete에는 특히 조심합니다.

## 실행 계획에서 볼 것

- Seq Scan: 순차 스캔
- Index Scan: 인덱스를 따라 테이블 조회
- Index Only Scan: 인덱스만으로 조회
- Bitmap Index Scan: 여러 조건 조합에 사용될 수 있음
- Nested Loop: 작은 결과끼리 조인에 유리
- Hash Join: 큰 데이터 조인에 사용될 수 있음
- Sort: 정렬 비용

## 선택도

선택도는 조건이 얼마나 적은 행을 골라내는지입니다.

예시:

- `where id = 1`: 선택도 높음
- `where active = true`: 대부분 true면 선택도 낮음

선택도가 낮은 컬럼 단독 인덱스는 효과가 적을 수 있습니다.

## 인덱스 설계 절차

1. 실제 느린 쿼리를 찾습니다.
2. where, join, order by, limit을 확인합니다.
3. `EXPLAIN ANALYZE`로 현재 계획을 봅니다.
4. 후보 인덱스를 만듭니다.
5. 실행 계획과 시간을 비교합니다.
6. 쓰기 비용과 저장 공간을 고려합니다.

## 체크리스트

- [ ] B-Tree 인덱스가 어떤 쿼리에 적합한지 설명할 수 있다.
- [ ] 복합 인덱스의 컬럼 순서를 쿼리 패턴으로 결정한다.
- [ ] partial index와 expression index의 용도를 안다.
- [ ] `EXPLAIN ANALYZE`로 실제 시간을 확인한다.
- [ ] 인덱스 추가가 쓰기 성능에 주는 비용을 고려한다.

