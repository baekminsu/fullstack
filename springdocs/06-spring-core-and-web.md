# Spring Core와 Web

Spring은 객체 생성, 의존성 연결, 트랜잭션, 웹 요청 처리, 보안, 테스트를 일관된 방식으로 지원하는 프레임워크입니다. Spring Boot는 Spring 애플리케이션을 빠르게 시작하도록 자동 설정과 실행 환경을 제공합니다.

## IoC와 DI

IoC(Inversion of Control)는 객체 생성과 제어 흐름의 주도권을 개발자 코드가 아니라 컨테이너가 갖는 구조입니다.

DI(Dependency Injection)는 필요한 의존 객체를 외부에서 주입받는 방식입니다.

```java
@Service
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

생성자 주입을 기본으로 사용합니다. 필수 의존성이 누락되면 애플리케이션 시작 시점에 알 수 있고, 테스트에서도 명확합니다.

## Bean

Bean은 Spring 컨테이너가 관리하는 객체입니다.

```java
@Component
public class EmailSender {
}
```

대표 애너테이션:

- `@Component`: 일반 컴포넌트
- `@Service`: 비즈니스 서비스
- `@Repository`: 저장소
- `@Controller`: MVC(Model View Controller) 컨트롤러
- `@RestController`: REST(Representational State Transfer) API(Application Programming Interface) 컨트롤러

## Configuration

직접 Bean을 등록할 때 사용합니다.

```java
@Configuration
public class AppConfig {
    @Bean
    public Clock clock() {
        return Clock.systemUTC();
    }
}
```

시간, 외부 API 클라이언트, 암호화 도구처럼 명시적으로 구성해야 하는 객체에 유용합니다.

## AOP

AOP(Aspect Oriented Programming)는 여러 곳에 흩어지는 공통 관심사를 분리하는 방식입니다. 로깅, 트랜잭션, 보안 검사가 대표적입니다.

Spring의 `@Transactional`도 AOP 기반으로 동작합니다.

```java
@Transactional
public Long createOrder(CreateOrderCommand command) {
    Order order = orderFactory.create(command);
    orderRepository.save(order);
    return order.getId();
}
```

## MVC(Model View Controller) 요청 흐름

MVC는 화면 또는 API 요청을 Controller, Model, View 역할로 나누는 패턴입니다. REST API에서는 View 대신 JSON(JavaScript Object Notation) 응답을 주로 반환합니다.

요청 흐름:

1. 클라이언트가 HTTP(HyperText Transfer Protocol) 요청을 보냅니다.
2. DispatcherServlet이 요청을 받습니다.
3. HandlerMapping이 컨트롤러 메서드를 찾습니다.
4. 컨트롤러가 요청 DTO(Data Transfer Object)를 받습니다.
5. 서비스가 유스케이스를 실행합니다.
6. 응답 DTO를 JSON으로 변환해 반환합니다.

## Controller

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @PostMapping
    public ResponseEntity<UserResponse> create(@Valid @RequestBody CreateUserRequest request) {
        UserResponse response = userService.create(request);
        return ResponseEntity.status(HttpStatus.CREATED).body(response);
    }
}
```

컨트롤러는 HTTP 변환 계층입니다. 비즈니스 규칙을 길게 넣지 않습니다.

## Request DTO(Data Transfer Object)와 Response DTO(Data Transfer Object)

요청과 응답은 도메인 객체와 분리합니다.

```java
public record CreateUserRequest(
    @NotBlank String email,
    @Size(min = 8, max = 50) String password
) {
}

public record UserResponse(
    Long id,
    String email
) {
}
```

Entity를 그대로 응답하면 DB(Database) 구조와 API 계약이 강하게 묶입니다.

## Validation

Bean Validation으로 요청값을 검증합니다.

```java
public record CreatePostRequest(
    @NotBlank String title,
    @Size(max = 5000) String content
) {
}
```

검증 실패 응답은 일관된 형식으로 만듭니다.

```json
{
  "code": "VALIDATION_ERROR",
  "message": "요청값이 올바르지 않습니다.",
  "fields": [
    { "name": "title", "reason": "비어 있을 수 없습니다." }
  ]
}
```

## 예외 처리

```java
@RestControllerAdvice
public class ApiExceptionHandler {
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidation(MethodArgumentNotValidException e) {
        return ResponseEntity.badRequest()
            .body(ErrorResponse.validation(e.getBindingResult()));
    }
}
```

예외 처리를 중앙화하면 컨트롤러가 성공 흐름에 집중할 수 있습니다.

## REST(Representational State Transfer) API(Application Programming Interface) 설계

REST(Representational State Transfer)는 리소스를 URI(Uniform Resource Identifier)로 표현하고 HTTP 메서드로 행위를 나타내는 스타일입니다.

예시:

- `GET /api/posts`: 목록 조회
- `POST /api/posts`: 생성
- `GET /api/posts/{id}`: 단건 조회
- `PATCH /api/posts/{id}`: 일부 수정
- `DELETE /api/posts/{id}`: 삭제

응답 상태 코드:

- `200 OK`: 조회 또는 수정 성공
- `201 Created`: 생성 성공
- `204 No Content`: 응답 본문 없는 성공
- `400 Bad Request`: 요청값 오류
- `401 Unauthorized`: 인증 필요
- `403 Forbidden`: 권한 없음
- `404 Not Found`: 리소스 없음
- `409 Conflict`: 상태 충돌

## 체크리스트

- [ ] IoC(Inversion of Control)와 DI(Dependency Injection)를 구분해 설명할 수 있다.
- [ ] 생성자 주입을 기본으로 사용한다.
- [ ] Controller에 비즈니스 규칙을 넣지 않는다.
- [ ] 요청 DTO와 응답 DTO를 도메인 객체와 구분한다.
- [ ] REST(Representational State Transfer) API의 URI와 상태 코드가 일관된다.
