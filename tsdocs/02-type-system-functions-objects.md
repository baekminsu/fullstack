# 함수, 객체, 타입 별칭, 인터페이스

TypeScript의 실용성은 함수 입력과 출력, 객체 구조, API(Application Programming Interface) 계약을 명확히 표현하는 데서 나옵니다.

## 함수 타입

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

반환 타입은 추론되지만, 공개 함수나 복잡한 함수는 명시하면 의도가 분명해집니다.

```typescript
async function getUser(userId: number): Promise<User> {
  const response = await fetch(`/api/users/${userId}`);
  return response.json();
}
```

## 선택적 매개변수

```typescript
function greet(name: string, nickname?: string) {
  return nickname ? `${name}(${nickname})` : name;
}
```

`nickname?: string`은 `string | undefined`와 비슷합니다.

## 기본값

```typescript
function listPosts(page = 1, size = 20) {
  return { page, size };
}
```

기본값이 있으면 타입은 추론됩니다.

## 객체 타입

```typescript
type User = {
  id: number;
  email: string;
  nickname?: string;
};
```

선택 속성은 없을 수 있습니다. 사용할 때 확인해야 합니다.

```typescript
if (user.nickname) {
  console.log(user.nickname.toUpperCase());
}
```

## readonly

```typescript
type User = {
  readonly id: number;
  email: string;
};
```

`readonly`는 TypeScript 수준에서 재할당을 막습니다. 런타임에서 완전한 불변을 보장하는 것은 아닙니다.

## type alias

타입 별칭은 타입에 이름을 붙입니다.

```typescript
type UserId = number;

type UserResponse = {
  id: UserId;
  email: string;
};
```

유니언, 튜플, 함수 타입 등 다양한 타입 표현에 유연합니다.

## interface

인터페이스는 객체의 구조를 정의합니다.

```typescript
interface UserRepository {
  findById(id: number): Promise<User>;
  save(user: User): Promise<void>;
}
```

객체 구조와 구현 계약을 표현할 때 자연스럽습니다.

## type과 interface 선택

일반 기준:

- 유니언, 리터럴 조합, 복잡한 타입 변환은 type이 편합니다.
- 객체 계약과 확장은 interface가 자연스럽습니다.
- 팀에서 하나의 기준을 정하면 더 중요합니다.

React props는 둘 다 가능합니다.

```typescript
type ButtonProps = {
  label: string;
  onClick: () => void;
};
```

## 배열과 튜플

배열:

```typescript
const users: User[] = [];
```

튜플:

```typescript
const position: [number, number] = [10, 20];
```

React hook 반환 타입처럼 위치별 의미가 정해져 있으면 튜플을 사용할 수 있습니다.

## 함수 타입 별칭

```typescript
type SubmitHandler = (values: SignupFormValues) => Promise<void>;
```

사용:

```typescript
type SignupFormProps = {
  onSubmit: SubmitHandler;
};
```

## 체크리스트

- [ ] 함수 매개변수와 반환값 타입을 명확히 표현한다.
- [ ] 선택 속성을 사용할 때 undefined 가능성을 처리한다.
- [ ] `readonly`가 컴파일 단계 보호라는 점을 이해한다.
- [ ] type alias와 interface의 사용 기준을 설명할 수 있다.
- [ ] 튜플이 배열보다 적합한 상황을 안다.
