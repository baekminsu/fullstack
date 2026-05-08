# Spring 백엔드 학습 가이드

Spring은 애너테이션을 외우는 프레임워크가 아니라 웹 요청, 객체 생명주기, 트랜잭션, 보안을 일관된 방식으로 다루는 도구입니다.

## 학습 순서

1. Java 기본기와 예외 처리
2. Spring IoC와 Bean 생명주기
3. Spring MVC 요청 처리
4. Validation과 예외 응답
5. JPA(Java Persistence API)와 트랜잭션
6. Spring Security
7. 테스트
8. 배포와 관측 가능성

## 반드시 이해할 질문

- Controller, Service, Repository는 왜 나누는가?
- 트랜잭션 경계는 어디에 두는가?
- JPA Entity는 도메인 모델인가, 영속성 모델인가?
- Lazy loading은 언제 문제가 되는가?
- 인증과 인가는 어디서 분리되는가?
- 테스트 슬라이스와 통합 테스트는 언제 쓰는가?

## 프로젝트 과제

1. 회원 가입과 로그인
2. 게시글 CRUD
3. 댓글과 대댓글
4. 검색과 페이지네이션
5. 파일 업로드
6. 관리자 권한
7. 감사 로그
8. 배치 작업

각 기능마다 아래 산출물을 남깁니다.

- API(Application Programming Interface) 명세
- DB(Database) 변경
- 성공 및 실패 테스트
- 예외 응답 예시
- 회고

## 테스트 전략

- Domain: 순수 단위 테스트
- Application Service: mocking을 최소화한 유스케이스 테스트
- Repository: 실제 PostgreSQL 또는 Testcontainers 기반 통합 테스트
- Controller: 요청, 응답, 검증, 권한 테스트
- End-to-End: 핵심 사용자 흐름만 얇게 테스트

## 체크리스트

- [ ] HTTP(HyperText Transfer Protocol) 상태 코드와 에러 응답 형식이 일관된다.
- [ ] 트랜잭션 범위가 너무 넓거나 좁지 않다.
- [ ] 엔티티 연관관계가 쿼리 성능을 망치지 않는다.
- [ ] 보안 정책이 컨트롤러 곳곳에 흩어져 있지 않다.
- [ ] 운영 로그가 문제 원인을 찾을 만큼 충분하다.
