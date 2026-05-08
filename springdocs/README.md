# Spring + Java 상세 학습 문서

이 폴더는 Java 기본 문법부터 Spring Boot 기반 백엔드 개발까지 순서대로 공부하기 위한 세부 문서입니다. 책 한 권을 처음부터 끝까지 따라가는 느낌을 목표로 하되, 단순 문법 나열보다 실제 백엔드 개발에서 어디에 쓰이는지 함께 정리합니다.

## 읽는 순서

1. [01-java-language-basics.md](./01-java-language-basics.md): Java 실행 구조, package, import, 변수, 타입, 연산자
2. [02-java-control-flow-and-methods.md](./02-java-control-flow-and-methods.md): 조건문, 반복문, 메서드, 입출력 기본
3. [03-java-oop-and-exceptions.md](./03-java-oop-and-exceptions.md): 클래스, 객체지향, 예외 처리
4. [04-java-collections-generics-lambda-stream.md](./04-java-collections-generics-lambda-stream.md): 컬렉션, 제네릭, 람다, 스트림
5. [05-java-io-file-io-and-concurrency.md](./05-java-io-file-io-and-concurrency.md): 콘솔 입출력, 파일 입출력, 스레드
6. [06-spring-core-and-web.md](./06-spring-core-and-web.md): Spring Core, DI(Dependency Injection), MVC(Model View Controller), REST(Representational State Transfer) API(Application Programming Interface)
7. [07-spring-data-jpa-and-transactions.md](./07-spring-data-jpa-and-transactions.md): JPA(Java Persistence API), 트랜잭션, 영속성, 쿼리
8. [08-spring-security-testing-observability.md](./08-spring-security-testing-observability.md): 보안, 테스트, 로깅, 관측 가능성
9. [09-spring-project-curriculum.md](./09-spring-project-curriculum.md): 단계별 프로젝트와 체크리스트

## 학습 산출물

- 각 장의 예제 코드를 직접 작성합니다.
- 예제마다 "왜 필요한가", "실무에서 어떤 버그를 막는가"를 한 줄로 기록합니다.
- Spring 기능은 반드시 HTTP(HyperText Transfer Protocol) 요청, DB(Database) 변경, 테스트 중 하나와 연결합니다.
