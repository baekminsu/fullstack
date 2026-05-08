# 유니언, 제네릭, 타입 좁히기

TypeScript의 강점은 가능한 상태를 타입으로 제한하고, 코드 흐름에 따라 타입을 좁혀 안전하게 사용하는 데 있습니다.

## Union type

유니언 타입은 여러 타입 중 하나를 허용합니다.

```typescript
type UserId = number | string;
```

상태 모델링에 유용합니다.

```typescript
type LoadState =
  | "idle"
  | "loading"
  | "success"
  | "error";
```

## Discriminated union

구분 필드를 가진 유니언입니다.

```typescript
type AsyncState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; message: string };
```

사용:

```typescript
function renderUser(state: AsyncState<User>) {
  switch (state.status) {
    case "idle":
      return "대기";
    case "loading":
      return "로딩";
    case "success":
      return state.data.email;
    case "error":
      return state.message;
  }
}
```

성공 상태에서만 data가 존재한다는 사실을 타입이 보장합니다.

## 타입 좁히기

`typeof`:

```typescript
function print(value: string | number) {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}
```

`in`:

```typescript
function getLabel(item: User | Team) {
  if ("email" in item) {
    return item.email;
  }
  return item.name;
}
```

사용자 정의 타입 가드:

```typescript
function isUser(value: unknown): value is User {
  return typeof value === "object"
    && value !== null
    && "email" in value;
}
```

## 제네릭

제네릭은 타입을 매개변수처럼 받아 재사용합니다.

```typescript
function identity<T>(value: T): T {
  return value;
}
```

API(Application Programming Interface) 응답:

```typescript
type ApiResponse<T> = {
  data: T;
  message: string;
};

type UserResponse = ApiResponse<User>;
```

## 제네릭 제약

```typescript
function getId<T extends { id: number }>(value: T): number {
  return value.id;
}
```

제약을 걸면 제네릭 내부에서 특정 속성을 안전하게 사용할 수 있습니다.

## keyof

객체 타입의 key를 타입으로 가져옵니다.

```typescript
type UserKey = keyof User; // "id" | "email" | ...
```

사용:

```typescript
function getValue<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```

## exhaustiveness check

모든 상태를 처리했는지 확인합니다.

```typescript
function assertNever(value: never): never {
  throw new Error(`Unexpected value: ${value}`);
}

function label(status: OrderStatus): string {
  switch (status) {
    case "CREATED":
      return "생성";
    case "PAID":
      return "결제";
    default:
      return assertNever(status);
  }
}
```

상태가 추가되면 컴파일 오류로 누락을 발견할 수 있습니다.

## 체크리스트

- [ ] 유니언으로 가능한 상태를 제한한다.
- [ ] discriminated union으로 로딩, 성공, 실패를 표현할 수 있다.
- [ ] 타입 가드로 unknown 값을 안전하게 좁힌다.
- [ ] 제네릭으로 재사용 가능한 응답 타입을 만든다.
- [ ] switch에서 누락된 상태를 컴파일 시점에 잡는다.
