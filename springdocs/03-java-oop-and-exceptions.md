# 객체지향과 예외 처리

객체지향 프로그래밍은 데이터와 행동을 함께 묶어 변경에 강한 구조를 만드는 방식입니다. Java 백엔드에서는 도메인 규칙을 어디에 둘지 결정하는 능력이 객체지향 실력과 직결됩니다.

## 클래스와 객체

클래스는 설계도이고 객체는 실행 중 만들어진 실제 인스턴스입니다.

```java
public class User {
    private final String email;
    private String nickname;

    public User(String email, String nickname) {
        this.email = email;
        this.nickname = nickname;
    }

    public void changeNickname(String nickname) {
        if (nickname == null || nickname.isBlank()) {
            throw new IllegalArgumentException("닉네임은 비어 있을 수 없습니다.");
        }
        this.nickname = nickname;
    }
}
```

필드는 private으로 숨기고, 객체가 허용하는 행동만 public 메서드로 공개합니다.

## 생성자

생성자는 객체를 유효한 상태로 시작하게 만듭니다.

```java
public User(String email) {
    if (email == null || !email.contains("@")) {
        throw new IllegalArgumentException("이메일 형식이 올바르지 않습니다.");
    }
    this.email = email;
}
```

생성자에서 필수값을 검증하면 이후 코드에서 null이나 잘못된 상태를 덜 걱정할 수 있습니다.

## 캡슐화

캡슐화는 필드를 숨기는 문법이 아니라 변경 가능성이 높은 결정을 객체 내부에 숨기는 설계 원칙입니다.

나쁜 예:

```java
if (user.getPoint() >= order.getTotalPrice()) {
    user.setPoint(user.getPoint() - order.getTotalPrice());
}
```

좋은 예:

```java
user.usePoint(order.getTotalPrice());
```

포인트가 충분한지, 차감 후 최소값이 얼마인지 같은 규칙은 `User`가 갖는 편이 자연스럽습니다.

## 상속

상속은 부모 클래스의 특성과 행동을 물려받습니다.

```java
public class Animal {
    public void move() {
        System.out.println("move");
    }
}

public class Dog extends Animal {
}
```

실무에서는 상속보다 조합을 선호하는 경우가 많습니다. 상속은 부모 변경이 자식에게 강하게 전파되기 때문입니다.

## 인터페이스

인터페이스는 구현보다 역할을 표현합니다.

```java
public interface PasswordEncoder {
    String encode(String rawPassword);
    boolean matches(String rawPassword, String encodedPassword);
}
```

애플리케이션 서비스는 구체 구현이 아니라 인터페이스에 의존할 수 있습니다.

```java
public class SignupService {
    private final PasswordEncoder passwordEncoder;

    public SignupService(PasswordEncoder passwordEncoder) {
        this.passwordEncoder = passwordEncoder;
    }
}
```

이렇게 하면 테스트에서 가짜 구현을 넣거나, 운영에서 다른 암호화 구현으로 바꾸기 쉽습니다.

## enum

enum은 정해진 값 목록을 타입으로 표현합니다.

```java
public enum OrderStatus {
    CREATED, PAID, SHIPPED, CANCELLED
}
```

문자열 상태값은 오타와 잘못된 값이 들어오기 쉽습니다. 주문 상태, 회원 등급, 권한처럼 값의 종류가 정해져 있으면 enum을 먼저 고려합니다.

## record

record는 불변 데이터 전달 객체를 간결하게 만들 수 있습니다.

```java
public record UserResponse(Long id, String email, String nickname) {
}
```

DTO(Data Transfer Object)나 조회 응답처럼 값 묶음이 필요한 경우에 적합합니다. 도메인 규칙과 복잡한 상태 변경이 필요한 객체를 무조건 record로 만들지는 않습니다.

## 예외

예외는 정상 흐름으로 처리할 수 없는 실패를 표현합니다.

```java
throw new IllegalArgumentException("잘못된 요청입니다.");
```

Checked Exception은 컴파일러가 처리를 강제합니다.

```java
public String readFile(String path) throws IOException {
    return Files.readString(Path.of(path));
}
```

Unchecked Exception은 `RuntimeException` 계열입니다. 도메인 규칙 위반이나 잘못된 프로그래밍 상태를 표현할 때 자주 사용합니다.

## 커스텀 예외

실패 의미가 중요하면 이름 있는 예외를 만듭니다.

```java
public class UserNotFoundException extends RuntimeException {
    public UserNotFoundException(Long userId) {
        super("사용자를 찾을 수 없습니다. userId=" + userId);
    }
}
```

Spring에서는 `@ControllerAdvice`에서 예외를 HTTP(HyperText Transfer Protocol) 응답으로 변환합니다.

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> handle(UserNotFoundException e) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
            .body(new ErrorResponse("USER_NOT_FOUND", e.getMessage()));
    }
}
```

## SOLID 첫 언급 규칙

약어는 처음 나올 때 풀네임을 함께 씁니다.

- SRP(Single Responsibility Principle): 하나의 모듈은 하나의 변경 이유를 가져야 합니다.
- OCP(Open Closed Principle): 확장에는 열려 있고 변경에는 닫혀 있어야 합니다.
- LSP(Liskov Substitution Principle): 하위 타입은 상위 타입을 대체할 수 있어야 합니다.
- ISP(Interface Segregation Principle): 사용하지 않는 인터페이스에 의존하지 않아야 합니다.
- DIP(Dependency Inversion Principle): 고수준 정책은 저수준 구현에 직접 의존하지 않아야 합니다.

## 체크리스트

- [ ] 객체 생성 시 유효하지 않은 상태를 막는다.
- [ ] 필드를 외부에서 직접 바꾸지 못하게 한다.
- [ ] 인터페이스는 실제 교체 가능성이 있는 경계에 둔다.
- [ ] 예외 이름이 실패 의미를 드러낸다.
- [ ] 약어는 첫 언급 시 풀네임을 병기한다.
