# 트랜잭션, 잠금, 동시성

동시성은 백엔드 개발자가 반드시 이해해야 하는 주제입니다. 애플리케이션 코드가 맞아 보여도 동시에 실행되면 데이터가 깨질 수 있습니다.

## 트랜잭션

트랜잭션은 여러 SQL을 하나의 작업 단위로 묶습니다.

```sql
begin;

update products
set stock = stock - 1
where id = 1;

insert into orders (user_id, product_id)
values (10, 1);

commit;
```

실패하면 rollback합니다.

```sql
rollback;
```

## ACID

ACID(Atomicity, Consistency, Isolation, Durability)는 트랜잭션의 핵심 성질입니다.

- Atomicity: 모두 성공하거나 모두 실패합니다.
- Consistency: 데이터 규칙이 깨지지 않습니다.
- Isolation: 동시에 실행되는 작업이 서로에게 어떻게 보이는지 정합니다.
- Durability: 커밋된 결과가 보존됩니다.

## Isolation level

격리 수준은 동시 트랜잭션 간 보이는 데이터를 조절합니다.

PostgreSQL에서 자주 다루는 수준:

- Read Committed: 커밋된 데이터만 읽습니다.
- Repeatable Read: 트랜잭션 동안 같은 조회는 같은 결과를 봅니다.
- Serializable: 직렬 실행처럼 동작하도록 보장합니다.

격리 수준이 높을수록 충돌과 비용이 늘 수 있습니다.

## Lost update

동시에 같은 값을 읽고 각각 업데이트하면 한쪽 변경이 사라질 수 있습니다.

나쁜 흐름:

1. A가 stock 10을 읽습니다.
2. B가 stock 10을 읽습니다.
3. A가 stock 9로 저장합니다.
4. B가 stock 9로 저장합니다.

두 번 차감했는데 결과가 9가 됩니다.

## 원자적 update

```sql
update products
set stock = stock - 1
where id = 1
  and stock > 0;
```

영향받은 행 수가 1이면 성공, 0이면 재고 부족으로 판단할 수 있습니다.

## Row lock

```sql
select *
from products
where id = 1
for update;
```

`for update`는 선택한 행에 잠금을 걸어 다른 트랜잭션의 동시 수정을 막습니다.

잠금은 오래 잡을수록 대기와 장애 가능성을 높입니다. 외부 API(Application Programming Interface) 호출을 트랜잭션 안에서 오래 수행하지 않습니다.

## Deadlock

교착 상태는 서로가 가진 잠금을 기다리며 진행하지 못하는 상황입니다.

예시:

1. 트랜잭션 A가 상품 1을 잠급니다.
2. 트랜잭션 B가 상품 2를 잠급니다.
3. A가 상품 2를 기다립니다.
4. B가 상품 1을 기다립니다.

예방:

- 같은 순서로 자원에 접근합니다.
- 트랜잭션을 짧게 유지합니다.
- 불필요한 잠금을 줄입니다.
- 실패 시 재시도 정책을 둡니다.

## Optimistic locking

낙관적 잠금은 version 컬럼으로 충돌을 감지합니다.

```sql
update posts
set title = 'new title',
    version = version + 1
where id = 1
  and version = 3;
```

영향받은 행이 0이면 누군가 먼저 수정한 것입니다.

## Idempotency

멱등성은 같은 요청을 여러 번 보내도 결과가 한 번 처리된 것과 같게 만드는 성질입니다.

결제, 주문, 쿠폰 발급에서는 idempotency key를 저장해 중복 처리를 막습니다.

```sql
create table idempotency_keys (
    key text primary key,
    response jsonb not null,
    created_at timestamptz not null default now()
);
```

## 체크리스트

- [ ] ACID(Atomicity, Consistency, Isolation, Durability)를 설명할 수 있다.
- [ ] lost update를 예제로 재현할 수 있다.
- [ ] 원자적 update와 row lock의 차이를 안다.
- [ ] deadlock 예방 방법을 설명할 수 있다.
- [ ] 중요한 요청에는 멱등성 전략을 고려한다.
