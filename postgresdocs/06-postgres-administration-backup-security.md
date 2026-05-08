# 운영, 백업, 권한, 보안

개발자가 PostgreSQL을 운영 수준으로 이해하려면 백업, 복구, 권한, 연결 수, 마이그레이션을 알아야 합니다.

## role과 user

PostgreSQL에서는 role이 사용자와 권한 그룹 역할을 모두 할 수 있습니다.

```sql
create role app_user login password 'change-me';
```

권한 부여:

```sql
grant usage on schema public to app_user;
grant select, insert, update, delete on all tables in schema public to app_user;
```

운영에서는 애플리케이션 계정에 슈퍼유저 권한을 주지 않습니다.

## 최소 권한 원칙

Principle of Least Privilege는 필요한 권한만 부여하는 원칙입니다.

예시:

- 애플리케이션 계정: 필요한 테이블 DML(Data Manipulation Language) 권한
- 마이그레이션 계정: DDL(Data Definition Language) 권한
- 조회 계정: select 권한
- 백업 계정: 백업에 필요한 권한

## 연결 수

DB(Database) 연결은 비용이 큽니다. Spring의 connection pool과 PostgreSQL의 `max_connections`를 함께 봐야 합니다.

확인:

```sql
select count(*)
from pg_stat_activity;
```

현재 연결:

```sql
select pid, usename, state, query
from pg_stat_activity
order by backend_start desc;
```

## long running query

오래 실행 중인 쿼리를 확인합니다.

```sql
select
    pid,
    now() - query_start as duration,
    state,
    query
from pg_stat_activity
where state <> 'idle'
order by duration desc;
```

운영에서 쿼리를 종료하기 전에는 영향 범위를 확인합니다.

## 백업

논리 백업:

```bash
pg_dump -h localhost -U app_user -d app_db -f backup.sql
```

복구:

```bash
psql -h localhost -U app_user -d app_db -f backup.sql
```

대용량 운영 DB는 물리 백업, WAL(Write Ahead Log) 보관, PITR(Point In Time Recovery)을 검토합니다.

## 마이그레이션

마이그레이션 도구:

- Flyway
- Liquibase

원칙:

- 이미 적용된 마이그레이션 파일을 수정하지 않습니다.
- 새 변경은 새 파일로 추가합니다.
- 운영 배포 전 staging에서 검증합니다.
- 긴 lock을 잡는 DDL은 분리합니다.

## 보안

기본 원칙:

- DB를 인터넷에 직접 노출하지 않습니다.
- 강한 비밀번호와 비밀 관리 도구를 사용합니다.
- 애플리케이션 로그에 민감 데이터를 남기지 않습니다.
- SQL(Structured Query Language) injection을 막기 위해 parameter binding을 사용합니다.

나쁜 예:

```java
String sql = "select * from users where email = '" + email + "'";
```

좋은 예:

```sql
select *
from users
where email = ?;
```

## 모니터링 지표

볼 것:

- 연결 수
- 느린 쿼리
- lock 대기
- deadlock 횟수
- cache hit ratio
- 테이블과 인덱스 크기
- replication lag
- 디스크 사용량

## 체크리스트

- [ ] 애플리케이션 DB 계정에 최소 권한만 준다.
- [ ] 현재 연결과 오래 실행 중인 쿼리를 확인할 수 있다.
- [ ] 백업과 복구 절차를 실제로 테스트한다.
- [ ] 마이그레이션 파일을 수정하지 않고 새 파일로 추가한다.
- [ ] SQL injection을 parameter binding으로 막는다.
