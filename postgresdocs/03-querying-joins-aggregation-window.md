# 조회, 조인, 집계, 윈도우 함수

서비스의 대부분 화면은 데이터를 잘 조회하는 문제입니다. 단순 select를 넘어 조인, 집계, 윈도우 함수를 이해하면 복잡한 목록과 통계를 효율적으로 만들 수 있습니다.

## JOIN

join은 여러 테이블의 행을 연결합니다.

```sql
select p.id, p.title, u.email
from posts p
join users u on u.id = p.user_id;
```

## INNER JOIN

양쪽에 매칭되는 행만 반환합니다.

```sql
select p.id, c.id as comment_id
from posts p
join comments c on c.post_id = p.id;
```

댓글이 없는 게시글은 나오지 않습니다.

## LEFT JOIN

왼쪽 테이블의 행은 모두 유지합니다.

```sql
select p.id, p.title, count(c.id) as comment_count
from posts p
left join comments c on c.post_id = p.id
group by p.id, p.title;
```

댓글이 없는 게시글도 count 0으로 보여줄 수 있습니다.

## GROUP BY

집계를 위해 행을 그룹으로 묶습니다.

```sql
select user_id, count(*) as post_count
from posts
group by user_id;
```

조건:

```sql
select user_id, count(*) as post_count
from posts
group by user_id
having count(*) >= 10;
```

`where`는 그룹 전 행을 필터링하고, `having`은 그룹 결과를 필터링합니다.

## 집계 함수

- `count`
- `sum`
- `avg`
- `min`
- `max`

```sql
select
    count(*) as order_count,
    sum(total_price) as total_sales,
    avg(total_price) as average_order_price
from orders
where status = 'PAID';
```

## DISTINCT

중복을 제거합니다.

```sql
select distinct user_id
from orders;
```

무분별한 distinct는 잘못된 join을 숨길 수 있습니다. 왜 중복이 생겼는지 먼저 확인합니다.

## CTE

CTE(Common Table Expression)는 복잡한 쿼리를 이름 있는 중간 결과로 나눕니다.

```sql
with paid_orders as (
    select *
    from orders
    where status = 'PAID'
)
select user_id, count(*)
from paid_orders
group by user_id;
```

가독성을 높이는 데 좋지만, 성능은 실행 계획으로 확인해야 합니다.

## Subquery

```sql
select *
from users
where id in (
    select user_id
    from orders
    where status = 'PAID'
);
```

서브쿼리와 join 중 어떤 것이 더 좋은지는 데이터 크기, 인덱스, 실행 계획에 따라 달라집니다.

## Window function

윈도우 함수는 행을 유지하면서 집계나 순위를 계산합니다.

```sql
select
    id,
    user_id,
    total_price,
    row_number() over (
        partition by user_id
        order by created_at desc
    ) as order_rank
from orders;
```

사용 사례:

- 사용자별 최신 주문 찾기
- 카테고리별 순위
- 누적 합계
- 이전 행과 비교

## Pagination 쿼리

offset 방식:

```sql
select id, title
from posts
order by id desc
limit 20 offset 1000;
```

keyset 방식:

```sql
select id, title
from posts
where id < 5000
order by id desc
limit 20;
```

최신순 무한 스크롤은 keyset 방식이 유리한 경우가 많습니다.

## 체크리스트

- [ ] inner join과 left join의 결과 차이를 설명할 수 있다.
- [ ] where와 having의 차이를 안다.
- [ ] distinct로 중복 원인을 덮지 않는다.
- [ ] CTE(Common Table Expression)로 복잡한 쿼리를 나눌 수 있다.
- [ ] window function으로 순위와 누적 값을 계산할 수 있다.

