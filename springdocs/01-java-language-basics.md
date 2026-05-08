# Java 언어 기본

Java는 JVM(Java Virtual Machine) 위에서 실행되는 정적 타입 언어입니다. 백엔드 개발에서 Java를 잘한다는 것은 문법을 아는 것뿐 아니라 타입, 객체, 예외, 스레드, 메모리 모델을 이용해 안정적인 서버 코드를 작성할 수 있다는 뜻입니다.

## JDK, JRE, JVM

- JDK(Java Development Kit): 개발 도구입니다. 컴파일러 `javac`, 실행 도구 `java`, 표준 라이브러리가 포함됩니다.
- JRE(Java Runtime Environment): Java 프로그램 실행 환경입니다.
- JVM(Java Virtual Machine): 바이트코드를 운영체제에 맞게 실행하는 가상 머신입니다.

개발자는 `.java` 파일을 작성하고, 컴파일러는 `.class` 바이트코드를 만듭니다. JVM은 이 바이트코드를 실행합니다.

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello Java");
    }
}
```

## package와 import

`package`는 클래스의 소속 위치를 나타냅니다. 파일 시스템의 폴더 구조와 맞추는 것이 일반적입니다.

```java
package com.example.user;

public class User {
}
```

`import`는 다른 패키지의 클래스를 현재 파일에서 짧은 이름으로 쓰기 위해 사용합니다.

```java
import java.time.LocalDate;
import java.util.List;

public class SignupRequest {
    private String email;
    private LocalDate birthDate;
    private List<String> interests;
}
```

처음 배울 때는 `import java.util.*;`처럼 와일드카드를 쓰기 쉽지만, 실무에서는 어떤 타입을 쓰는지 명확히 보이도록 구체 import를 선호합니다.

## 클래스 이름과 파일 이름

public class는 파일 이름과 같아야 합니다.

```java
// UserService.java
public class UserService {
}
```

하나의 파일에 여러 클래스를 둘 수는 있지만, 외부에서 공개되는 public class는 하나만 둡니다. 유지보수 관점에서는 한 파일에 하나의 중심 클래스를 두는 편이 좋습니다.

## 변수와 상수

변수는 값을 담는 이름입니다.

```java
int age = 25;
String name = "minsu";
boolean active = true;
```

상수는 `final`로 선언합니다.

```java
final int MAX_LOGIN_ATTEMPTS = 5;
```

Spring 설정값이나 도메인 정책값을 코드에 직접 흩뿌리면 변경이 어려워집니다. 의미 있는 이름을 가진 상수나 설정값으로 분리합니다.

## 기본 타입과 참조 타입

기본 타입:

- `byte`, `short`, `int`, `long`
- `float`, `double`
- `char`
- `boolean`

참조 타입:

- `String`
- 배열
- 클래스
- 인터페이스
- enum

기본 타입은 값 자체를 다루고, 참조 타입은 객체의 위치를 가리킵니다.

```java
int a = 10;
int b = a;
b = 20;
System.out.println(a); // 10

User u1 = new User("a@example.com");
User u2 = u1;
u2.changeEmail("b@example.com");
System.out.println(u1.getEmail()); // b@example.com
```

참조 타입을 공유하면 한쪽 변경이 다른 쪽에 영향을 줍니다. DTO(Data Transfer Object), Entity, Domain Model을 다룰 때 특히 조심해야 합니다.

## String

`String`은 불변 객체입니다. 한 번 만들어진 문자열은 내부 값이 바뀌지 않습니다.

```java
String name = "kim";
name = name.toUpperCase(); // 새 문자열을 반환
```

문자열을 반복해서 이어 붙일 때는 `StringBuilder`를 사용합니다.

```java
StringBuilder builder = new StringBuilder();
builder.append("Hello");
builder.append(" ");
builder.append("Java");
String result = builder.toString();
```

## 형 변환

작은 범위에서 큰 범위로는 자동 변환됩니다.

```java
int count = 10;
long total = count;
```

큰 범위에서 작은 범위로는 명시적 변환이 필요합니다.

```java
long total = 10L;
int count = (int) total;
```

실무에서는 숫자 타입 변환이 금액, 수량, 페이지 번호 버그를 만들 수 있습니다. 금액은 `BigDecimal`을 우선 고려합니다.

## 연산자

산술 연산자:

```java
int sum = 10 + 5;
int mod = 10 % 3;
```

비교 연산자:

```java
boolean adult = age >= 19;
```

논리 연산자:

```java
boolean canLogin = active && !locked;
```

문자열 비교는 `==`가 아니라 `equals`를 사용합니다.

```java
String role = "ADMIN";

if ("ADMIN".equals(role)) {
    System.out.println("관리자");
}
```

`"ADMIN".equals(role)`처럼 상수를 앞에 두면 `role`이 null이어도 `NullPointerException`이 발생하지 않습니다.

## 배열

배열은 같은 타입 값을 고정 길이로 저장합니다.

```java
int[] scores = {90, 80, 70};
System.out.println(scores[0]);
```

길이가 자주 바뀌는 데이터는 배열보다 `List`를 더 많이 사용합니다. HTTP(HyperText Transfer Protocol) 요청 목록, DB(Database) 조회 결과, 화면 목록 응답은 대부분 컬렉션으로 다룹니다.

## null

`null`은 참조가 없다는 뜻입니다.

```java
String name = null;
System.out.println(name.length()); // NullPointerException
```

null을 줄이는 방법:

- 생성자에서 필수 값을 받습니다.
- 빈 목록은 null 대신 `List.of()` 또는 `Collections.emptyList()`를 반환합니다.
- 반환값이 없을 수 있음을 표현하려면 `Optional`을 제한적으로 사용합니다.
- Bean Validation으로 요청값을 검증합니다.

## 기본 체크리스트

- [ ] package와 import의 역할을 설명할 수 있다.
- [ ] 기본 타입과 참조 타입의 차이를 예제로 설명할 수 있다.
- [ ] 문자열 비교에 `equals`를 사용한다.
- [ ] null 가능성을 코드 경계에서 줄인다.
- [ ] 금액과 수량 타입 선택을 의식적으로 한다.
