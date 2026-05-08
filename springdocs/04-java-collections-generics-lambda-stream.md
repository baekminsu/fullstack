# 컬렉션, 제네릭, 람다, 스트림

서버 애플리케이션은 목록 조회, 필터링, 그룹핑, 변환을 계속 수행합니다. Java 컬렉션과 스트림을 이해하면 API(Application Programming Interface) 응답 조립, 도메인 계산, 테스트 데이터 생성이 쉬워집니다.

## List

`List`는 순서가 있는 컬렉션입니다.

```java
List<String> names = new ArrayList<>();
names.add("kim");
names.add("lee");
System.out.println(names.get(0));
```

조회 결과의 순서가 중요하면 `List`를 사용합니다. 페이지네이션 결과, 댓글 목록, 주문 상품 목록이 대표적입니다.

## Set

`Set`은 중복을 허용하지 않습니다.

```java
Set<String> tags = new HashSet<>();
tags.add("spring");
tags.add("spring");
System.out.println(tags.size()); // 1
```

중복 제거가 목적이면 `Set`을 고려합니다. 단, 객체의 중복 기준은 `equals`와 `hashCode`에 의해 결정됩니다.

## Map

`Map`은 key와 value를 저장합니다.

```java
Map<Long, User> usersById = new HashMap<>();
usersById.put(1L, new User("a@example.com"));
User user = usersById.get(1L);
```

DB에서 여러 사용자와 주문을 조회한 뒤 메모리에서 묶을 때 자주 사용합니다.

```java
Map<Long, List<Order>> ordersByUserId = orders.stream()
    .collect(Collectors.groupingBy(Order::getUserId));
```

## 불변 컬렉션

```java
List<String> roles = List.of("USER", "ADMIN");
```

불변 컬렉션은 외부에서 변경되면 안 되는 값을 표현할 때 좋습니다. 단, `List.of()`는 null을 허용하지 않습니다.

## 제네릭

제네릭은 타입을 매개변수처럼 다루는 기능입니다.

```java
public class ApiResponse<T> {
    private final T data;

    public ApiResponse(T data) {
        this.data = data;
    }
}
```

사용 예:

```java
ApiResponse<UserResponse> response = new ApiResponse<>(userResponse);
```

제네릭을 쓰면 컴파일 시점에 타입 오류를 잡을 수 있습니다.

## Optional

`Optional`은 값이 있을 수도 없을 수도 있음을 표현합니다.

```java
Optional<User> user = userRepository.findByEmail(email);
```

좋은 사용:

```java
User user = userRepository.findByEmail(email)
    .orElseThrow(() -> new UserNotFoundException(email));
```

주의할 점:

- 필드 타입으로 남용하지 않습니다.
- 컬렉션은 `Optional<List<T>>`보다 빈 목록을 반환합니다.
- null을 숨기는 용도로 쓰면 안 됩니다.

## 람다

람다는 익명 함수를 간결하게 표현합니다.

```java
names.forEach(name -> System.out.println(name));
```

메서드 참조:

```java
names.forEach(System.out::println);
```

## 함수형 인터페이스

추상 메서드가 하나인 인터페이스입니다.

```java
@FunctionalInterface
public interface DiscountPolicy {
    int discount(int price);
}
```

람다로 구현할 수 있습니다.

```java
DiscountPolicy policy = price -> price / 10;
```

## Stream

스트림은 컬렉션 데이터를 선언적으로 처리합니다.

```java
List<String> activeUserEmails = users.stream()
    .filter(User::isActive)
    .map(User::getEmail)
    .toList();
```

자주 쓰는 연산:

- `filter`: 조건에 맞는 값만 남깁니다.
- `map`: 다른 값으로 변환합니다.
- `sorted`: 정렬합니다.
- `distinct`: 중복을 제거합니다.
- `collect`: 결과를 컬렉션이나 Map으로 모읍니다.

## 스트림 사용 기준

스트림이 좋은 경우:

- 필터링과 변환 흐름이 명확할 때
- 중간 컬렉션을 줄이고 싶을 때
- 결과가 새 컬렉션일 때

반복문이 더 좋은 경우:

- 중간에 복잡한 상태 변경이 많을 때
- 예외 처리와 로깅이 많이 섞일 때
- 디버깅을 한 줄씩 해야 할 때

## 체크리스트

- [ ] List, Set, Map의 선택 이유를 설명할 수 있다.
- [ ] equals와 hashCode가 Set, Map key에 미치는 영향을 이해한다.
- [ ] 제네릭으로 API 응답 타입을 안전하게 표현할 수 있다.
- [ ] Optional을 반환값 경계에서 제한적으로 사용한다.
- [ ] 스트림이 가독성을 떨어뜨리면 반복문을 선택한다.
