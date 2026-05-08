# Google Style Guide와 PostgreSQL SQL 규칙

기준일: 2026-05-09  
공식 출처: [Google Style Guides 목록](https://google.github.io/styleguide/)

현재 공개된 Google Style Guides 목록에는 PostgreSQL 또는 SQL(Structured Query Language) 전용 스타일 가이드가 없습니다. 따라서 이 문서는 Google Style Guides의 공통 목표인 일관성, 가독성, 유지보수성을 PostgreSQL SQL 작성 규칙으로 적용한 로컬 가이드입니다.

## 적용 원칙

공식 PostgreSQL 전용 Google 문서가 없으므로 아래 순서로 판단합니다.

1. 기존 프로젝트 SQL 스타일을 먼저 따릅니다.
2. 새 SQL은 이 문서의 로컬 규칙을 따릅니다.
3. ORM(Object Relational Mapping)이 생성하는 SQL도 로그에서 읽기 쉽게 분석합니다.
4. 성능 판단은 포맷이 아니라 `EXPLAIN ANALYZE` 결과로 합니다.
5. 운영 SQL은 읽기 쉬운 것보다 안전한 것이 먼저입니다.

## 키워드와 식별자

이 저장소에서는 SQL keyword를 소문자로 작성합니다.

```sql
select id, email
from users
where deleted_at is null
order by id desc;
```

테이블, 컬럼, 인덱스 이름은 `snake_case`를 사용합니다.

```sql
create table order_items (
    id bigserial primary key,
    order_id bigint not null,
    product_id bigint not null,
    quantity integer not null
);
```

따옴표가 필요한 대소문자 혼합 식별자는 피합니다.

```sql
-- Avoid
create table "OrderItems" (
    "OrderId" bigint
);
```

## 들여쓰기

DDL(Data Definition Language)에서는 컬럼 정의를 4칸 들여씁니다.

```sql
create table users (
    id bigserial primary key,
    email text not null unique,
    name text not null,
    created_at timestamptz not null default now()
);
```

SELECT는 절마다 줄을 나눕니다.

```sql
select
    p.id,
    p.title,
    u.email as author_email,
    count(c.id) as comment_count
from posts p
join users u on u.id = p.user_id
left join comments c on c.post_id = p.id
where p.status = 'PUBLISHED'
group by p.id, p.title, u.email
order by p.created_at desc, p.id desc
limit 20;
```

짧고 단순한 쿼리는 한 줄도 허용하지만, 조건이나 join이 들어가면 줄을 나눕니다.

## alias

테이블 alias는 짧되 의미가 유지되어야 합니다.

```sql
select p.id, p.title, u.email
from posts p
join users u on u.id = p.user_id;
```

복잡한 쿼리에서는 한 글자 alias보다 의미 있는 alias를 사용합니다.

```sql
select paid_orders.user_id, count(*)
from paid_orders
group by paid_orders.user_id;
```

컬럼 alias는 응답 필드 의미를 드러냅니다.

```sql
select count(*) as published_post_count
from posts
where status = 'PUBLISHED';
```

## 조건문

`and` 조건은 줄을 나눠 읽기 쉽게 만듭니다.

```sql
select id, email
from users
where active = true
  and deleted_at is null
  and email_verified = true;
```

`or` 조건은 괄호로 의도를 명확히 합니다.

```sql
select id, title
from posts
where status = 'PUBLISHED'
  and (
      title ilike :keyword
      or content ilike :keyword
  );
```

## join 규칙

join 조건은 `on`에 명확히 둡니다.

```sql
select p.id, c.id as comment_id
from posts p
left join comments c on c.post_id = p.id;
```

join 때문에 중복이 생겼다면 `distinct`로 덮기 전에 원인을 확인합니다.

```sql
-- 먼저 join cardinality를 확인한다.
select p.id, count(c.id)
from posts p
left join comments c on c.post_id = p.id
group by p.id;
```

## DDL 이름 규칙

제약 조건과 인덱스 이름은 목적이 보이게 짓습니다.

```sql
alter table users
add constraint uq_users_email unique (email);

alter table order_items
add constraint fk_order_items_order_id
foreign key (order_id) references orders(id);

create index idx_posts_status_created_at
on posts (status, created_at desc);
```

권장 prefix:

- `pk_`: primary key
- `fk_`: foreign key
- `uq_`: unique constraint 또는 unique index
- `idx_`: 일반 index
- `chk_`: check constraint

## migration SQL

운영 마이그레이션은 작게 나눕니다.

좋은 흐름:

1. nullable column을 추가합니다.
2. 애플리케이션이 새 컬럼을 채우게 배포합니다.
3. 기존 데이터를 backfill합니다.
4. `not null`과 index를 추가합니다.
5. 더 이상 쓰지 않는 컬럼은 별도 배포에서 제거합니다.

마이그레이션 파일 이름 예:

```text
V20260509_001__add_user_last_login_at.sql
```

이미 적용된 마이그레이션 파일은 수정하지 않습니다.

## 안전한 운영 SQL

운영에서 update와 delete를 실행할 때는 항상 트랜잭션으로 감쌉니다.

```sql
begin;

select count(*)
from users
where last_login_at < now() - interval '2 years';

update users
set dormant = true
where last_login_at < now() - interval '2 years';

-- 영향받은 행 수 확인 후
commit;
-- 또는 rollback;
```

대량 변경은 batch로 나눕니다.

```sql
update users
set dormant = true
where id in (
    select id
    from users
    where dormant = false
      and last_login_at < now() - interval '2 years'
    order by id
    limit 1000
);
```

## 성능 관련 스타일

쿼리 스타일은 실행 계획을 읽기 쉽게 해야 합니다.

좋은 습관:

- 필요한 컬럼만 select합니다.
- join 조건을 명확히 둡니다.
- pagination에는 안정적인 tie breaker를 둡니다.
- 조건과 정렬이 index 설계와 맞는지 확인합니다.
- 느린 쿼리는 `EXPLAIN ANALYZE` 결과를 문서화합니다.

```sql
select id, title, created_at
from posts
where status = 'PUBLISHED'
  and id < :last_seen_id
order by id desc
limit 20;
```

## Spring 연동 규칙

JPA(Java Persistence API) 또는 JDBC(Java Database Connectivity)에서 직접 SQL을 작성할 때도 같은 규칙을 유지합니다.

```java
@Query("""
    select p
    from PostEntity p
    where p.status = :status
    order by p.createdAt desc, p.id desc
    """)
List<PostEntity> findRecentPosts(@Param("status") PostStatus status);
```

긴 query string을 한 줄로 만들지 않습니다. SQL이 길어지면 repository method 이름도 함께 점검합니다.

## 체크리스트

- [ ] 공개 Google Style Guides에 PostgreSQL 전용 문서가 없다는 점을 알고 로컬 규칙을 적용한다.
- [ ] SQL keyword는 소문자로 작성한다.
- [ ] 테이블, 컬럼, 인덱스는 `snake_case`를 사용한다.
- [ ] 복잡한 SELECT는 절마다 줄을 나눈다.
- [ ] join 중복을 `distinct`로 덮기 전에 원인을 확인한다.
- [ ] 제약 조건과 인덱스 이름에 목적을 드러낸다.
- [ ] 운영 update/delete는 트랜잭션과 대상 행 수 확인을 거친다.
- [ ] 성능 판단은 `EXPLAIN ANALYZE`로 확인한다.

