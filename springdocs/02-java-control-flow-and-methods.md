# 제어문, 메서드, 입출력 기본

프로그램은 값을 계산하고, 조건에 따라 분기하고, 반복하며, 외부 입력을 받아 결과를 출력합니다. 이 장은 Java의 기본 실행 흐름을 다룹니다.

## if 문

조건에 따라 실행할 코드를 선택합니다.

```java
if (age >= 19) {
    System.out.println("성인");
} else {
    System.out.println("미성년자");
}
```

조건이 많아질수록 의미 있는 메서드로 분리합니다.

```java
if (canIssueCoupon(user, order)) {
    couponService.issue(user, order);
}

private boolean canIssueCoupon(User user, Order order) {
    return user.isActive() && order.isPaid() && order.totalPrice() >= 50_000;
}
```

## switch 문

여러 값 중 하나를 선택할 때 사용합니다.

```java
switch (grade) {
    case "A":
        System.out.println("우수");
        break;
    case "B":
        System.out.println("보통");
        break;
    default:
        System.out.println("확인 필요");
}
```

enum과 함께 쓰면 문자열 오타를 줄일 수 있습니다.

```java
public enum OrderStatus {
    CREATED, PAID, SHIPPED, CANCELLED
}
```

## for 문

횟수가 명확한 반복에 사용합니다.

```java
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}
```

컬렉션 순회는 향상된 for 문을 자주 사용합니다.

```java
for (String name : names) {
    System.out.println(name);
}
```

## while 문

조건이 참인 동안 반복합니다.

```java
while (scanner.hasNextLine()) {
    String line = scanner.nextLine();
    if ("exit".equals(line)) {
        break;
    }
    System.out.println(line);
}
```

서버 코드에서는 무한 루프를 조심해야 합니다. 종료 조건, 타임아웃, 예외 처리가 없으면 CPU 사용률이 치솟을 수 있습니다.

## 메서드

메서드는 이름 있는 동작입니다.

```java
public int add(int a, int b) {
    return a + b;
}
```

메서드를 나누는 기준:

- 한 가지 책임을 갖는가?
- 이름이 의도를 드러내는가?
- 입력과 출력이 명확한가?
- 테스트하기 쉬운가?

## 메서드 오버로딩

같은 이름의 메서드를 매개변수 조합으로 구분하는 기능입니다.

```java
public User findUser(Long id) {
    return userRepository.findById(id);
}

public User findUser(String email) {
    return userRepository.findByEmail(email);
}
```

오버로딩이 많아지면 호출자가 어떤 메서드가 호출되는지 헷갈릴 수 있습니다. 의미가 다르면 이름을 다르게 짓는 편이 낫습니다.

## main 메서드

Java 애플리케이션의 시작점입니다.

```java
public static void main(String[] args) {
    System.out.println("start");
}
```

Spring Boot도 내부적으로 main 메서드에서 시작합니다.

```java
@SpringBootApplication
public class AppApplication {
    public static void main(String[] args) {
        SpringApplication.run(AppApplication.class, args);
    }
}
```

## 콘솔 출력

```java
System.out.println("한 줄 출력");
System.out.print("줄바꿈 없이 출력");
System.out.printf("name=%s age=%d%n", name, age);
```

서버 애플리케이션에서는 `System.out.println`보다 로거를 사용합니다. 로그 레벨, 시간, 요청 ID(Request Identifier), 스레드 이름 등을 함께 남길 수 있기 때문입니다.

## 콘솔 입력: Scanner

간단한 입력에는 `Scanner`를 사용합니다.

```java
Scanner scanner = new Scanner(System.in);

System.out.print("name: ");
String name = scanner.nextLine();

System.out.print("age: ");
int age = scanner.nextInt();
```

`nextInt()` 뒤에 `nextLine()`을 바로 쓰면 줄바꿈 문자가 남아 문제가 생길 수 있습니다. 초보 단계에서는 모든 입력을 `nextLine()`으로 받고 필요한 타입으로 변환하는 방식이 더 안전합니다.

```java
int age = Integer.parseInt(scanner.nextLine());
```

## 콘솔 입력: BufferedReader

입력이 많을 때는 `BufferedReader`가 빠릅니다.

```java
BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
String line = reader.readLine();
int count = Integer.parseInt(line);
```

알고리즘 문제 풀이에서는 `BufferedReader`와 `StringTokenizer` 조합을 자주 씁니다.

```java
StringTokenizer tokenizer = new StringTokenizer(reader.readLine());
int a = Integer.parseInt(tokenizer.nextToken());
int b = Integer.parseInt(tokenizer.nextToken());
```

## 입력값 검증

사용자 입력은 믿으면 안 됩니다.

```java
private int parsePositiveInt(String input) {
    int value = Integer.parseInt(input);
    if (value <= 0) {
        throw new IllegalArgumentException("양수만 입력할 수 있습니다.");
    }
    return value;
}
```

Spring에서는 요청 DTO(Data Transfer Object)에 Bean Validation을 적용합니다.

```java
public record CreateUserRequest(
    @NotBlank String email,
    @Size(min = 8, max = 50) String password
) {
}
```

## 체크리스트

- [ ] 조건문이 길어지면 의미 있는 메서드로 분리한다.
- [ ] 반복문에는 종료 조건이 명확히 있다.
- [ ] 메서드 이름이 내부 구현보다 목적을 설명한다.
- [ ] 입력값을 검증하지 않은 채 도메인 로직으로 넘기지 않는다.
- [ ] 콘솔 출력과 서버 로그의 차이를 설명할 수 있다.

