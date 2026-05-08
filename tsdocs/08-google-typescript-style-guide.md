# Google TypeScript Style Guide 적용 가이드

기준일: 2026-05-09  
공식 출처: [Google TypeScript Style Guide](https://google.github.io/styleguide/tsguide.html)

Google TypeScript Style Guide는 Google 내부 TypeScript 스타일 가이드를 공개 환경에 맞게 일부 조정한 문서입니다. 원문도 외부 프로젝트에는 그대로 맞지 않을 수 있다고 설명합니다. 이 문서는 React + TypeScript 학습 코드에서 우선 적용할 규칙을 정리합니다.

## 핵심 원칙

TypeScript 스타일의 목표는 타입을 많이 쓰는 것이 아니라, 새 독자가 코드를 안전하게 읽고 바꿀 수 있게 만드는 것입니다.

우선순위:

1. 타입으로 표현할 수 있는 상태는 타입으로 제한합니다.
2. 런타임 검증이 필요한 외부 입력은 타입만 믿지 않습니다.
3. 이름은 타입에 이미 들어 있는 정보를 반복하지 않습니다.
4. 컴파일 오류를 억지로 숨기지 않습니다.
5. 복잡한 타입보다 읽히는 타입을 우선합니다.

## 모듈과 import

ES module 문법을 사용합니다.

```typescript
import { getUser } from "./userApi";
import type { User } from "./userTypes";
```

타입만 가져올 때는 `import type`을 사용합니다.

```typescript
import type { UserResponse } from "@/api/user";
```

사용하지 않을 것:

```typescript
// Avoid
import x = require("mydep");

// Avoid
/// <reference path="./types.d.ts" />
```

## 배열과 객체 생성

Array constructor를 사용하지 않습니다.

```typescript
// Bad
const values = new Array(2);

// Good
const values = [2];
const emptyValues: number[] = [];
```

Object constructor도 사용하지 않습니다.

```typescript
// Bad
const user = new Object();

// Good
const user = {};
```

## 타입 이름과 식별자

식별자는 ASCII letters, digits, underscore, 드문 경우의 `$`를 사용합니다.

이름 원칙:

- 타입에 이미 들어 있는 정보를 이름에 중복하지 않습니다.
- private field에 앞뒤 underscore를 붙이지 않습니다.
- optional parameter에 `opt_` prefix를 붙이지 않습니다.
- interface 이름에 `I` prefix를 붙이지 않습니다.
- 이름은 새 독자가 이해할 수 있게 충분히 설명적이어야 합니다.

```typescript
// Bad
interface IUser {
  userEmail: string;
}

// Good
interface User {
  email: string;
}
```

## any와 unknown

`any`는 타입 검사를 꺼 버립니다. 외부 입력처럼 타입을 모르는 값은 `unknown`을 먼저 사용합니다.

```typescript
function parseJson(json: string): unknown {
  return JSON.parse(json);
}

const value = parseJson(responseText);

if (isUserResponse(value)) {
  renderUser(value);
}
```

타입 단언은 마지막 수단입니다.

```typescript
// Avoid
const user = value as User;
```

## @ts-ignore 금지

Google TypeScript Style Guide는 `@ts-ignore`, `@ts-expect-error`, `@ts-nocheck`를 사용하지 말라고 설명합니다.

```typescript
// Bad
// @ts-ignore
const user: User = externalValue;
```

대신 아래 중 하나를 선택합니다.

- 타입 정의를 고칩니다.
- 런타임 검증을 추가합니다.
- 제네릭이나 union type을 정확히 모델링합니다.
- 외부 라이브러리 타입 문제라면 별도 adapter를 둡니다.

## class와 private

Google TypeScript Style Guide는 JavaScript private field인 `#private` 대신 TypeScript visibility annotation을 사용하라고 설명합니다.

```typescript
class UserCache {
  private values = new Map<number, User>();

  get(userId: number): User | undefined {
    return this.values.get(userId);
  }
}
```

React 코드에서는 class보다 function component와 hooks를 기본으로 사용하지만, class를 만들 때는 위 규칙을 따릅니다.

## 세미콜론

ASI(Automatic Semicolon Insertion)에 의존하지 않습니다. 문장은 세미콜론으로 끝냅니다.

```typescript
const name = "minsu";
const age = 25;
```

포맷터를 쓰면 세미콜론 정책을 자동으로 유지할 수 있습니다.

## enum과 union

Google TypeScript Style Guide는 `const enum`을 사용하지 말라고 설명합니다.

```typescript
// Avoid
const enum OrderStatus {
  Created,
  Paid,
}
```

React 애플리케이션에서는 문자열 literal union을 많이 사용합니다.

```typescript
type OrderStatus = "CREATED" | "PAID" | "SHIPPED" | "CANCELLED";
```

값 목록과 타입을 함께 관리하려면 `as const`를 사용할 수 있습니다.

```typescript
const orderStatuses = ["CREATED", "PAID", "SHIPPED", "CANCELLED"] as const;
type OrderStatus = (typeof orderStatuses)[number];
```

## 에러와 상태 모델링

boolean state를 여러 개 조합하면 불가능한 상태가 생기기 쉽습니다.

```typescript
type AsyncState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; message: string };
```

컴포넌트는 상태별로 명확히 분기합니다.

```tsx
function UserPanel({ state }: { state: AsyncState<User> }) {
  switch (state.status) {
    case "idle":
      return null;
    case "loading":
      return <Loading />;
    case "success":
      return <UserCard user={state.data} />;
    case "error":
      return <ErrorPanel message={state.message} />;
  }
}
```

## 금지하거나 피할 것

- `@ts-ignore`, `@ts-expect-error`, `@ts-nocheck`
- `eval`
- `Function(...string)` constructor
- builtin object prototype 수정
- `with`
- unfiltered `for...in`
- `const enum`
- 의미 없는 type assertion
- 프로젝트에 맞지 않는 과도한 고급 타입

## 자동화 도구

추천:

- `tsc --noEmit`: 타입 체크
- ESLint TypeScript rules
- Prettier
- React hooks lint
- CI(Continuous Integration)에서 `typecheck`, `lint`, `test`, `build` 분리

## 체크리스트

- [ ] ES module import를 사용한다.
- [ ] 타입 전용 import는 `import type`을 사용한다.
- [ ] `any`보다 `unknown`과 타입 가드를 우선한다.
- [ ] `@ts-ignore` 계열 주석을 사용하지 않는다.
- [ ] interface 이름에 `I` prefix를 붙이지 않는다.
- [ ] `const enum`을 사용하지 않는다.
- [ ] 세미콜론을 명시한다.
- [ ] 불가능한 UI(User Interface) 상태를 union type으로 막는다.

