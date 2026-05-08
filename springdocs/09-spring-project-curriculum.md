# Java + Spring 프로젝트 커리큘럼

문법만 읽으면 실력이 늘지 않습니다. 각 단계는 작은 프로젝트로 검증합니다.

## 1단계: Java 콘솔 프로그램

주제:

- 계산기
- 가계부
- 단어장

필수 적용:

- package와 import
- 조건문과 반복문
- 메서드 분리
- 콘솔 입출력
- 파일 저장과 읽기

완료 기준:

- 잘못된 입력을 처리합니다.
- 파일이 없을 때 초기 상태로 시작합니다.
- 기능별 메서드 이름이 명확합니다.

## 2단계: Java 객체지향 프로그램

주제:

- 도서 대여
- 주문과 할인
- 좌석 예약

필수 적용:

- 클래스와 객체
- 캡슐화
- enum
- 예외
- 컬렉션
- 단위 테스트

완료 기준:

- 도메인 객체가 자기 규칙을 스스로 지킵니다.
- 서비스 클래스 하나에 모든 로직이 몰리지 않습니다.
- 테스트 이름이 요구사항을 설명합니다.

## 3단계: Spring REST(Representational State Transfer) API(Application Programming Interface)

주제:

- 회원과 게시글 API

필수 적용:

- Controller, Service, Repository
- 요청 DTO(Data Transfer Object), 응답 DTO
- Bean Validation
- 전역 예외 처리
- REST(Representational State Transfer) 상태 코드

완료 기준:

- Postman이나 HTTP(HyperText Transfer Protocol) 파일로 전체 API를 호출할 수 있습니다.
- 검증 실패 응답이 일관됩니다.
- 컨트롤러에 비즈니스 규칙이 없습니다.

## 4단계: Spring + PostgreSQL

주제:

- 게시판과 댓글

필수 적용:

- JPA(Java Persistence API) Entity
- Repository
- 트랜잭션
- 연관관계
- 페이지네이션
- N+1 문제 확인

완료 기준:

- SQL(Structured Query Language) 로그를 보고 실행 쿼리를 설명합니다.
- 목록 API에 페이지네이션이 있습니다.
- 조회 성능을 `EXPLAIN ANALYZE`로 확인합니다.

## 5단계: 인증과 권한

주제:

- 로그인과 관리자 기능

필수 적용:

- Spring Security
- 비밀번호 암호화
- 인증(Authentication)
- 인가(Authorization)
- 권한별 API 접근 제어

완료 기준:

- 로그인하지 않은 사용자는 보호 API에 접근할 수 없습니다.
- 일반 사용자는 관리자 API에 접근할 수 없습니다.
- 테스트로 권한 실패를 검증합니다.

## 6단계: 운영 가능한 API(Application Programming Interface) 서버

주제:

- 개인 지식 관리 서비스

필수 적용:

- 로깅
- 예외 코드 체계
- DB(Database) 마이그레이션
- 통합 테스트
- 배포 문서
- 장애 시나리오

완료 기준:

- README만 보고 실행할 수 있습니다.
- 장애 재현과 해결 과정을 문서화합니다.
- 주요 기능에 테스트가 있습니다.

## 매 단계 회고 질문

- 어떤 요구사항이 코드 구조를 어렵게 만들었는가?
- 테스트가 실제로 버그를 잡았는가?
- DB 쿼리를 눈으로 확인했는가?
- 예외와 실패 응답이 사용자에게 도움이 되는가?
- 다음 단계에서 버릴 습관은 무엇인가?
