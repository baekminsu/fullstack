# Google Java Style Guide 적용 가이드

기준일: 2026-05-09  
공식 출처: [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)

이 문서는 공식 Google Java Style Guide를 Java + Spring 학습 코드에 적용하기 위한 요약입니다. 원문은 Java 언어의 소스 파일, import, 포맷팅, 이름, Javadoc 규칙을 다룹니다. 여기서는 Spring 프로젝트에서 바로 적용할 규칙을 우선 정리합니다.

## 핵심 원칙

Google Java Style Guide는 Java 소스 파일이 Google Style이라고 불리려면 해당 문서의 규칙을 따라야 한다고 정의합니다. 단순히 예쁘게 보이는 포맷이 아니라, 큰 코드베이스에서 읽기 쉽고 일관된 코드를 만들기 위한 규칙입니다.

Spring 프로젝트에서는 아래 순서로 우선 적용합니다.

1. 파일 구조와 import를 일관되게 유지합니다.
2. 자동 포매터로 들여쓰기와 줄 길이 논쟁을 줄입니다.
3. 이름은 줄임말보다 의미를 드러내게 짓습니다.
4. 주석은 코드가 말하지 못하는 의도와 제약만 설명합니다.
5. 기존 파일을 수정할 때는 주변 코드의 지역 스타일도 존중합니다.

## 소스 파일 기본

파일 이름:

- top-level class는 파일 하나에 하나만 둡니다.
- public top-level class 이름과 파일 이름을 일치시킵니다.
- Java 파일 확장자는 `.java`입니다.

인코딩:

- 소스 파일은 UTF-8을 사용합니다.
- 들여쓰기에는 tab을 쓰지 않습니다.
- 일반 공백은 ASCII space를 사용합니다.

Spring 예시:

```java
package com.example.user.application;

public class SignupService {
}
```

## 파일 구조 순서

일반 Java 소스 파일은 아래 순서를 따릅니다.

1. 라이선스 또는 copyright 정보가 있으면 맨 위
2. package 선언
3. import 선언
4. 정확히 하나의 top-level class 선언

각 섹션 사이에는 빈 줄 하나를 둡니다.

```java
package com.example.user.application;

import com.example.user.domain.User;
import com.example.user.domain.UserRepository;

public class SignupService {
}
```

## import 규칙

와일드카드 import는 사용하지 않습니다.

```java
// Bad
import java.util.*;

// Good
import java.util.List;
import java.util.Map;
```

static import와 일반 import는 그룹을 나누고, 각 그룹 안에서는 ASCII 정렬을 유지합니다.

```java
import static org.assertj.core.api.Assertions.assertThat;

import java.time.Clock;
import java.util.List;
```

Spring 프로젝트에서 IDE(Integrated Development Environment)의 optimize imports 기능을 켜 두면 이 규칙을 유지하기 쉽습니다.

## 포맷팅

들여쓰기:

- block이 열릴 때마다 2칸 들여씁니다.
- tab을 쓰지 않습니다.

```java
public class User {
  public boolean isActive() {
    return true;
  }
}
```

중괄호:

- 비어 있지 않은 block은 K&R(Kernighan and Ritchie) 스타일을 따릅니다.
- `if`, `else`, `for`, `do`, `while`에는 본문이 한 줄이어도 중괄호를 사용합니다.

```java
if (user.isActive()) {
  user.login();
}
```

줄 길이:

- Java 코드는 100자 제한을 기준으로 합니다.
- package 선언과 import는 예외입니다.
- 긴 코드는 억지로 줄바꿈하기보다 메서드나 지역 변수로 분리할 수 있는지 먼저 봅니다.

## 선언 규칙

한 줄에 하나의 문장만 둡니다.

```java
// Bad
int age = 20; String name = "kim";

// Good
int age = 20;
String name = "kim";
```

변수는 한 선언에 하나만 둡니다.

```java
// Bad
int width, height;

// Good
int width;
int height;
```

지역 변수는 블록 시작에 몰아두지 말고, 처음 필요한 지점 가까이에 선언합니다.

## 이름 규칙

일반 원칙:

- 이름은 새로 읽는 사람이 이해할 수 있게 충분히 설명적이어야 합니다.
- 프로젝트 내부 사람만 아는 약어를 피합니다.
- 문자 몇 개를 아끼려고 의미를 잃지 않습니다.

일반 Java 명명:

- class, interface, enum: `UpperCamelCase`
- method, field, local variable: `lowerCamelCase`
- constant: `UPPER_SNAKE_CASE`
- package: 모두 소문자

Spring 예시:

```java
public class OrderPaymentService {
  private static final int MAX_RETRY_COUNT = 3;

  public PaymentResult payOrder(Order order) {
    int retryCount = 0;
    return paymentClient.pay(order, retryCount);
  }
}
```

## Javadoc과 주석

Javadoc은 공개 API(Application Programming Interface), 복잡한 도메인 규칙, 사용자가 반드시 알아야 하는 제약을 설명할 때 사용합니다.

```java
/**
 * 주문 결제를 시도한다.
 *
 * <p>결제 승인 이후 실패하면 보상 트랜잭션이 필요하다.
 */
public PaymentResult pay(Order order) {
  return paymentClient.pay(order);
}
```

일반 주석은 코드가 이미 말하는 내용을 반복하지 않습니다.

```java
// Bad: user를 저장한다.
userRepository.save(user);

// Good: 외부 결제 승인 전에는 주문 상태를 확정하지 않는다.
order.markPaymentPending();
```

## Spring 프로젝트 적용 규칙

Controller:

- HTTP(HyperText Transfer Protocol) 요청과 응답 변환에 집중합니다.
- 도메인 규칙을 길게 넣지 않습니다.
- 요청 DTO(Data Transfer Object)와 응답 DTO 이름을 명확히 둡니다.

Service 또는 UseCase:

- 한 사용자 목표를 표현하는 이름을 사용합니다.
- 트랜잭션 경계를 명확히 합니다.
- 외부 API 호출과 DB(Database) 상태 변경의 순서를 의식적으로 설계합니다.

Repository:

- JPA(Java Persistence API) 메서드 이름이 길어지면 명시 쿼리나 별도 구현을 고려합니다.
- 조회 목적이 분명한 메서드 이름을 사용합니다.

테스트:

- 테스트 메서드 이름은 요구사항을 설명합니다.
- static import는 AssertJ, JUnit처럼 테스트 가독성을 높이는 경우에 제한적으로 사용합니다.

## 자동화 도구

추천:

- `google-java-format`: 포맷팅 자동화
- IDE import 정리
- Checkstyle 또는 Spotless: CI(Continuous Integration)에서 스타일 확인

스타일 규칙은 리뷰에서 손으로 싸우는 주제가 아니라 도구로 처리할 수 있어야 합니다.

## 체크리스트

- [ ] top-level class는 파일 하나에 하나만 둔다.
- [ ] 와일드카드 import를 쓰지 않는다.
- [ ] 들여쓰기는 2칸, tab은 사용하지 않는다.
- [ ] Java 줄 길이는 100자를 기준으로 한다.
- [ ] `if`, `for`, `while`에 중괄호를 생략하지 않는다.
- [ ] 이름은 의미를 드러내고 불명확한 약어를 피한다.
- [ ] Javadoc은 공개 계약과 중요한 제약을 설명한다.
- [ ] 포맷팅은 `google-java-format` 같은 도구로 자동화한다.

