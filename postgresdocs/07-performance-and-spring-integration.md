# 성능 튜닝과 Spring 연동

Spring 애플리케이션의 성능 문제는 Java 코드, 네트워크, connection pool, SQL(Structured Query Language), 인덱스가 함께 얽혀 나타납니다. PostgreSQL을 Spring과 함께 볼 수 있어야 원인을 좁힐 수 있습니다.

## 성능 분석 순서

1. 어떤 API가 느린지 확인합니다.
2. 서버 로그에서 처리 시간을 봅니다.
3. DB(Database) 쿼리 수와 실행 시간을 확인합니다.
4. 느린 SQL을 `EXPLAIN ANALYZE`로 봅니다.
5. 인덱스, 쿼리, 데이터 모델 중 무엇이 문제인지 판단합니다.
6. 수정 전후 시간을 비교합니다.

## JPA(Java Persistence API)와 SQL(Structured Query Language) 로그

개발 환경에서는 SQL 로그를 켜서 ORM(Object Relational Mapping)이 어떤 쿼리를 실행하는지 봅니다.

주의:

- 로그를 예쁘게 출력하는 것과 실제 바인딩 값 확인은 별도 설정일 수 있습니다.
- 운영에서 과도한 SQL 로그는 성능과 보안 문제가 됩니다.

## N+1 문제

게시글 목록 20개를 조회했는데 작성자 조회 쿼리가 20번 더 나가면 N+1 문제입니다.

해결:

- fetch join
- batch size
- DTO(Data Transfer Object) projection
- 필요한 데이터만 직접 조회

목록 API는 Entity 그래프를 그대로 반환하기보다 화면에 필요한 DTO를 조회하는 방식이 더 명확할 때가 많습니다.

## Connection pool

Spring Boot에서는 HikariCP가 기본 connection pool로 많이 사용됩니다.

볼 것:

- maximum pool size
- connection timeout
- idle timeout
- DB max connections
- 애플리케이션 인스턴스 수

예시:

```text
앱 4대 * pool 20 = 최대 80개 연결
```

DB의 허용 연결 수와 운영 도구 연결까지 고려해야 합니다.

## 트랜잭션 범위

나쁜 예:

```java
@Transactional
public void placeOrder(Command command) {
    Order order = orderRepository.save(...);
    paymentClient.pay(...); // 외부 API
    order.markPaid();
}
```

외부 API가 느리면 DB 트랜잭션과 connection을 오래 잡습니다.

개선 방향:

- DB 상태 변경과 외부 API 호출을 분리합니다.
- outbox pattern을 검토합니다.
- 실패 보상 흐름을 설계합니다.

## 대량 처리

JPA로 대량 insert/update를 처리하면 영속성 컨텍스트가 커져 메모리를 압박할 수 있습니다.

대안:

- batch size 설정
- 일정 단위로 flush와 clear
- JDBC batch
- SQL 직접 실행
- COPY 사용 검토

## 검색

단순 `like '%keyword%'`는 인덱스를 활용하기 어렵습니다.

대안:

- prefix 검색이면 B-Tree 조건 설계
- full text search 검토
- trigram index 검토
- 별도 검색 엔진 검토

검색 요구사항은 초기에 명확히 잡아야 합니다. "제목 검색"과 "본문 포함 유사 검색"은 비용이 다릅니다.

## 통계와 vacuum

PostgreSQL은 통계를 기반으로 실행 계획을 고릅니다. 데이터가 크게 바뀌면 통계가 중요합니다.

개념:

- analyze: 통계 갱신
- vacuum: dead tuple 정리
- autovacuum: 자동 vacuum

운영에서 autovacuum이 제대로 동작하지 않으면 테이블이 부풀고 쿼리가 느려질 수 있습니다.

## 체크리스트

- [ ] API 지연을 서버, DB, 네트워크로 나누어 본다.
- [ ] JPA가 만든 SQL을 직접 확인한다.
- [ ] N+1 문제를 쿼리 수로 확인한다.
- [ ] connection pool 크기를 DB 연결 한도와 함께 계산한다.
- [ ] 트랜잭션 안에서 외부 API를 오래 호출하지 않는다.
