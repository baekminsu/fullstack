# 관계형 모델과 SQL(Structured Query Language) 기본

PostgreSQL은 관계형 데이터베이스 관리 시스템입니다. 데이터를 테이블, 행, 열로 표현하고 SQL(Structured Query Language)로 조회하고 변경합니다.

## 관계형 모델

기본 용어:

- Table: 같은 구조의 데이터를 저장하는 관계
- Row: 하나의 데이터 기록
- Column: 데이터의 속성
- Primary Key: 행을 식별하는 기본 키
- Foreign Key: 다른 테이블의 행을 참조하는 외래 키

예시:

```sql
users
-----
id | email           | name
1  | a@example.com   | kim
2  | b@example.com   | lee
```

## 데이터베이스와 스키마

Database는 독립된 데이터 저장 공간입니다. Schema는 데이터베이스 안에서 테이블, 함수, 뷰를 묶는 논리적 namespace입니다.

```sql
create schema app;

create table app.users (
    id bigserial primary key,
    email text not null
);
```

작은 프로젝트에서는 기본 `public` 스키마를 쓰기도 하지만, 운영 시스템에서는 스키마를 나누어 권한과 소유를 명확히 할 수 있습니다.

## SELECT

```sql
select id, email
from users;
```

모든 열:

```sql
select *
from users;
```

실무에서는 필요한 열만 명시하는 편이 좋습니다. 데이터 전송량과 의도를 모두 줄일 수 있습니다.

## WHERE

```sql
select id, email
from users
where active = true;
```

조건:

```sql
where age >= 19
where email like '%@example.com'
where created_at between '2026-01-01' and '2026-12-31'
where deleted_at is null
```

`null` 비교는 `= null`이 아니라 `is null`을 사용합니다.

## ORDER BY

```sql
select id, title, created_at
from posts
order by created_at desc;
```

정렬 기준이 같을 때 결과 순서가 흔들릴 수 있습니다. 페이지네이션에서는 tie breaker를 추가합니다.

```sql
order by created_at desc, id desc
```

## LIMIT과 OFFSET

```sql
select id, title
from posts
order by id desc
limit 20 offset 40;
```

offset이 커지면 앞의 행을 건너뛰는 비용이 커집니다. 큰 목록에서는 keyset pagination을 검토합니다.

## INSERT

```sql
insert into users (email, name)
values ('a@example.com', 'kim');
```

생성된 id 반환:

```sql
insert into users (email, name)
values ('a@example.com', 'kim')
returning id;
```

## UPDATE

```sql
update users
set name = 'new name'
where id = 1;
```

`where` 없는 update는 전체 행을 바꿉니다. 운영에서 직접 SQL을 실행할 때는 항상 트랜잭션과 대상 행 수 확인을 습관화합니다.

## DELETE

```sql
delete from users
where id = 1;
```

서비스에서는 물리 삭제보다 soft delete를 쓰는 경우가 많습니다.

```sql
update users
set deleted_at = now()
where id = 1;
```

## UPSERT

PostgreSQL은 `on conflict`로 insert 충돌 시 update를 처리할 수 있습니다.

```sql
insert into user_login_counts (user_id, count)
values (1, 1)
on conflict (user_id)
do update set count = user_login_counts.count + 1;
```

중복 요청 방지나 카운터 갱신에서 유용합니다.

## 체크리스트

- [ ] Table, Row, Column, Primary Key, Foreign Key를 설명할 수 있다.
- [ ] `select *`보다 필요한 열을 명시한다.
- [ ] null 비교에 `is null`을 사용한다.
- [ ] update와 delete에는 항상 where 조건을 확인한다.
- [ ] `returning`과 `on conflict`의 용도를 안다.
