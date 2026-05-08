# TypeScript 시작

TypeScript는 JavaScript의 상위 집합입니다. JavaScript 코드는 대부분 TypeScript 코드가 될 수 있고, TypeScript는 컴파일 과정을 거쳐 JavaScript로 실행됩니다.

## TypeScript가 필요한 이유

JavaScript는 런타임에 타입 오류가 드러나는 경우가 많습니다.

```javascript
function printEmail(user) {
  console.log(user.email.toLowerCase());
}

printEmail(null); // 런타임 오류
```

TypeScript는 개발 시점에 많은 오류를 잡습니다.

```typescript
type User = {
  email: string;
};

function printEmail(user: User) {
  console.log(user.email.toLowerCase());
}
```

## 타입 주석

```typescript
const name: string = "minsu";
const age: number = 25;
const active: boolean = true;
```

타입 추론이 가능하면 굳이 적지 않아도 됩니다.

```typescript
const name = "minsu"; // string으로 추론
```

명확한 초기값이 있으면 타입 추론을 믿고, 함수 매개변수와 API(Application Programming Interface) 경계에는 타입을 명시하는 습관이 좋습니다.

## 기본 타입

```typescript
let title: string = "post";
let count: number = 10;
let published: boolean = false;
let nothing: null = null;
let notAssigned: undefined = undefined;
```

배열:

```typescript
const numbers: number[] = [1, 2, 3];
const names: Array<string> = ["kim", "lee"];
```

객체:

```typescript
const user: { id: number; email: string } = {
  id: 1,
  email: "a@example.com",
};
```

## any

`any`는 타입 검사를 끕니다.

```typescript
let value: any = "hello";
value.notExists.deep.call(); // 컴파일 오류가 나지 않음
```

`any`는 마이그레이션이나 외부 라이브러리 임시 대응에서 제한적으로만 사용합니다.

## unknown

`unknown`은 타입을 모르지만 검증 전에는 사용할 수 없게 막습니다.

```typescript
function parseJson(json: string): unknown {
  return JSON.parse(json);
}

const value = parseJson("{}");

if (typeof value === "object" && value !== null) {
  console.log(value);
}
```

외부 입력, JSON(JavaScript Object Notation), API 응답은 실제로 런타임 검증이 필요합니다.

## void와 never

`void`는 반환값이 없음을 의미합니다.

```typescript
function log(message: string): void {
  console.log(message);
}
```

`never`는 정상적으로 끝나지 않는 함수를 표현합니다.

```typescript
function fail(message: string): never {
  throw new Error(message);
}
```

## literal type

정해진 값 자체를 타입으로 사용할 수 있습니다.

```typescript
let role: "USER" | "ADMIN" = "USER";
```

권한, 상태, 탭 이름처럼 값의 목록이 정해져 있으면 문자열보다 리터럴 타입이 안전합니다.

## strict 모드

`strict`는 TypeScript의 엄격한 타입 검사를 켭니다.

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

처음에는 불편하지만 장기적으로 null, undefined, 암묵적 any 버그를 줄입니다.

## 체크리스트

- [ ] TypeScript가 런타임이 아니라 개발 시점 검사를 제공한다는 점을 이해한다.
- [ ] 타입 추론이 가능한 곳과 명시해야 하는 경계를 구분한다.
- [ ] `any`보다 `unknown`을 우선 고려한다.
- [ ] literal type으로 상태값을 제한할 수 있다.
- [ ] strict 모드의 목적을 설명할 수 있다.
