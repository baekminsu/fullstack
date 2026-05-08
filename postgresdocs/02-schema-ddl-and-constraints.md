# 스키마, DDL(Data Definition Language), 제약 조건

좋은 데이터 모델은 애플리케이션 코드보다 오래 살아남습니다. PostgreSQL의 제약 조건은 데이터 무결성을 지키는 강력한 장치입니다.

## DDL(Data Definition Language)

DDL(Data Definition Language)은 테이블, 인덱스, 스키마 같은 구조를 정의하는 SQL입니다.

테이블 생성:

```sql
create table users (
    id bigserial primary key,
    email text not null,
    name text not null,
    created_at timestamptz not null default now()
);
```

테이블 변경:

```sql
alter table users add column last_login_at timestamptz;
```

컬럼 삭제:

```sql
alter table users drop column last_login_at;
```

운영 DB에서 컬럼 삭제는 복구가 어렵기 때문에 단계적으로 진행합니다.

## 데이터 타입

자주 쓰는 타입:

- `bigint`: 큰 정수
- `bigserial`: 자동 증가 큰 정수
- `text`: 문자열
- `boolean`: 참거짓
- `numeric`: 정확한 숫자, 금액에 적합
- `timestamp`: 시간대 없는 시각
- `timestamptz`: 시간대 고려 시각
- `date`: 날짜
- `jsonb`: JSON(JavaScript Object Notation) 바이너리 저장
- `uuid`: UUID(Universally Unique Identifier)

시간은 특별한 이유가 없다면 `timestamptz`를 우선 고려합니다.

## Primary Key

PK(Primary Key)는 행을 고유하게 식별합니다.

```sql
create table posts (
    id bigserial primary key,
    title text not null
);
```

PK는 null일 수 없고 중복될 수 없습니다.

## Foreign Key

FK(Foreign Key)는 다른 테이블의 행을 참조합니다.

```sql
create table posts (
    id bigserial primary key,
    user_id bigint not null references users(id),
    title text not null
);
```

외래 키를 두면 존재하지 않는 사용자로 게시글을 만들 수 없습니다.

## NOT NULL

```sql
email text not null
```

필수값은 애플리케이션 검증뿐 아니라 DB에서도 `not null`로 막습니다.

## UNIQUE

```sql
create table users (
    id bigserial primary key,
    email text not null unique
);
```

동시 요청에서는 애플리케이션에서 먼저 중복 검사를 해도 race condition이 생길 수 있습니다. DB(Database) unique constraint가 마지막 방어선입니다.

## CHECK

```sql
create table products (
    id bigserial primary key,
    name text not null,
    price numeric(12, 2) not null check (price >= 0)
);
```

도메인 규칙 중 DB가 안정적으로 검증할 수 있는 것은 check constraint로 표현합니다.

## DEFAULT

```sql
created_at timestamptz not null default now()
```

생성 시각, 활성 여부, 카운트 기본값에 자주 사용합니다.

## 정규화

정규화는 중복을 줄이고 데이터 이상 현상을 막는 설계 방식입니다.

예시:

나쁜 구조:

```text
orders(id, user_email, user_name, product_name, product_price)
```

개선:

```text
users(id, email, name)
products(id, name, price)
orders(id, user_id)
order_items(id, order_id, product_id, price_snapshot)
```

주문 당시 가격처럼 이력 보존이 필요한 값은 일부러 snapshot으로 중복 저장할 수 있습니다. 이것은 반정규화가 아니라 도메인 요구사항일 수 있습니다.

## 마이그레이션 원칙

- 변경은 작게 나눕니다.
- 컬럼 추가, 코드 배포, 데이터 채우기, 제약 추가를 단계로 분리합니다.
- 운영 데이터 변경 전 백업과 롤백 방법을 확인합니다.
- DB 마이그레이션 파일은 git에 남깁니다.

## 체크리스트

- [ ] DDL(Data Definition Language)의 역할을 설명할 수 있다.
- [ ] 필수값에는 `not null`을 둔다.
- [ ] 중복 방지는 unique constraint로 최종 보장한다.
- [ ] FK(Foreign Key)를 둘지 말지 의식적으로 결정한다.
- [ ] 마이그레이션을 작은 단계로 나눈다.
