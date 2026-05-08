# 고급 타입 패턴

고급 타입은 강력하지만 남용하면 팀 전체의 이해 비용을 높입니다. 목표는 타입 퍼즐이 아니라 중복을 줄이고 잘못된 상태를 막는 것입니다.

## Utility type

TypeScript가 제공하는 기본 유틸리티 타입입니다.

`Partial<T>`:

```typescript
type UserPatch = Partial<User>;
```

모든 속성을 선택적으로 만듭니다.

`Pick<T, K>`:

```typescript
type UserSummary = Pick<User, "id" | "email">;
```

`Omit<T, K>`:

```typescript
type CreateUserRequest = Omit<User, "id">;
```

`Record<K, V>`:

```typescript
type ErrorMessages = Record<string, string>;
```

## Mapped type

기존 타입의 key를 순회해 새 타입을 만듭니다.

```typescript
type ReadonlyFields<T> = {
  readonly [K in keyof T]: T[K];
};
```

폼 에러 타입:

```typescript
type FormErrors<T> = {
  [K in keyof T]?: string;
};

type SignupErrors = FormErrors<SignupFormValues>;
```

## Conditional type

조건에 따라 타입을 선택합니다.

```typescript
type ApiResult<T> = T extends Error
  ? { ok: false; error: T }
  : { ok: true; data: T };
```

복잡한 conditional type은 읽기 어려워질 수 있으므로 공용 라이브러리 수준에서 신중히 사용합니다.

## infer

조건부 타입 안에서 타입을 추론합니다.

```typescript
type UnwrapPromise<T> = T extends Promise<infer R> ? R : T;

type User = UnwrapPromise<Promise<{ id: number }>>;
```

## satisfies

값의 타입을 검사하되 값 자체의 좁은 타입은 유지합니다.

```typescript
const routes = {
  home: "/",
  posts: "/posts",
  settings: "/settings",
} satisfies Record<string, string>;
```

설정 객체, 라우트 맵, 권한 맵에서 유용합니다.

## as const

값을 리터럴 타입으로 고정합니다.

```typescript
const orderStatuses = ["CREATED", "PAID", "SHIPPED"] as const;

type OrderStatus = (typeof orderStatuses)[number];
```

상태 목록을 값과 타입으로 함께 관리할 수 있습니다.

## 브랜딩 타입

같은 number라도 의미가 다른 값을 구분하고 싶을 때 사용합니다.

```typescript
type Brand<T, B> = T & { readonly __brand: B };

type UserId = Brand<number, "UserId">;
type PostId = Brand<number, "PostId">;
```

ID를 섞어 쓰는 실수를 줄일 수 있지만 코드가 복잡해질 수 있으므로 핵심 도메인에 제한적으로 적용합니다.

## 타입 설계 원칙

- 타입이 도메인 규칙을 설명해야 합니다.
- 런타임 검증이 필요한 경계는 별도로 처리합니다.
- 공용 타입은 변경 파급이 크므로 이름을 신중히 짓습니다.
- 너무 똑똑한 타입보다 읽히는 타입이 낫습니다.

## 체크리스트

- [ ] Utility type으로 중복 타입을 줄일 수 있다.
- [ ] mapped type으로 폼 에러 타입을 만들 수 있다.
- [ ] conditional type의 비용을 이해한다.
- [ ] `satisfies`와 `as const`의 차이를 안다.
- [ ] 고급 타입을 공용 경계에 남용하지 않는다.

