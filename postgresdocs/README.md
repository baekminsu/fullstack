# PostgreSQL 상세 학습 문서

PostgreSQL은 관계형 데이터베이스이자 서비스의 데이터 무결성을 지키는 핵심 시스템입니다. 이 폴더는 SQL(Structured Query Language) 기초부터 인덱스, 트랜잭션, 운영, Spring 연동까지 순서대로 다룹니다.

## 읽는 순서

1. [01-relational-model-and-sql-basics.md](./01-relational-model-and-sql-basics.md): 관계형 모델, SQL 기본
2. [02-schema-ddl-and-constraints.md](./02-schema-ddl-and-constraints.md): 스키마, DDL(Data Definition Language), 제약 조건
3. [03-querying-joins-aggregation-window.md](./03-querying-joins-aggregation-window.md): 조회, 조인, 집계, 윈도우 함수
4. [04-indexes-and-explain.md](./04-indexes-and-explain.md): 인덱스, 실행 계획
5. [05-transactions-locks-concurrency.md](./05-transactions-locks-concurrency.md): 트랜잭션, 잠금, 동시성
6. [06-postgres-administration-backup-security.md](./06-postgres-administration-backup-security.md): 운영, 백업, 권한, 보안
7. [07-performance-and-spring-integration.md](./07-performance-and-spring-integration.md): 성능 튜닝과 Spring 연동
8. [08-postgres-project-curriculum.md](./08-postgres-project-curriculum.md): 단계별 프로젝트

## 학습 원칙

- SQL을 ORM(Object Relational Mapping) 뒤에 숨기지 말고 직접 읽습니다.
- 제약 조건은 애플리케이션 검증의 보조가 아니라 데이터 무결성의 마지막 방어선입니다.
- 성능 문제는 `EXPLAIN ANALYZE`로 확인한 뒤 판단합니다.
