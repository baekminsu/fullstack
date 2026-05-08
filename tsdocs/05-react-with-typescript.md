# React와 TypeScript

React에서 TypeScript의 핵심은 props, state, event, API(Application Programming Interface) 응답, 서버 상태를 정확히 모델링하는 것입니다.

## props 타입

```tsx
type ButtonProps = {
  label: string;
  disabled?: boolean;
  onClick: () => void;
};

function Button({ label, disabled = false, onClick }: ButtonProps) {
  return (
    <button disabled={disabled} onClick={onClick}>
      {label}
    </button>
  );
}
```

선택 props에는 기본값을 줄 수 있습니다.

## children 타입

```tsx
import type { ReactNode } from "react";

type PanelProps = {
  title: string;
  children: ReactNode;
};

function Panel({ title, children }: PanelProps) {
  return (
    <section>
      <h2>{title}</h2>
      {children}
    </section>
  );
}
```

`ReactNode`는 문자열, 숫자, 요소, null 등 렌더링 가능한 값을 포함합니다.

## 이벤트 타입

```tsx
function SearchInput() {
  function handleChange(event: React.ChangeEvent<HTMLInputElement>) {
    console.log(event.target.value);
  }

  return <input onChange={handleChange} />;
}
```

form submit:

```tsx
function handleSubmit(event: React.FormEvent<HTMLFormElement>) {
  event.preventDefault();
}
```

## useState 타입

초기값으로 추론 가능한 경우:

```tsx
const [count, setCount] = useState(0);
```

초기값이 null인 경우:

```tsx
const [selectedUser, setSelectedUser] = useState<User | null>(null);
```

배열:

```tsx
const [users, setUsers] = useState<User[]>([]);
```

빈 배열은 타입을 명시하지 않으면 `never[]`로 추론될 수 있습니다.

## useReducer 타입

```tsx
type State = {
  email: string;
  password: string;
  submitting: boolean;
};

type Action =
  | { type: "changeEmail"; value: string }
  | { type: "changePassword"; value: string }
  | { type: "submitStart" }
  | { type: "submitEnd" };

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case "changeEmail":
      return { ...state, email: action.value };
    case "changePassword":
      return { ...state, password: action.value };
    case "submitStart":
      return { ...state, submitting: true };
    case "submitEnd":
      return { ...state, submitting: false };
  }
}
```

상태 전이가 많으면 discriminated union이 안전합니다.

## API(Application Programming Interface) 응답 타입

```typescript
type UserResponse = {
  id: number;
  email: string;
  nickname: string;
};

async function getUser(userId: number): Promise<UserResponse> {
  const response = await fetch(`/api/users/${userId}`);
  if (!response.ok) {
    throw new Error("사용자 조회 실패");
  }
  return response.json();
}
```

주의: TypeScript 타입은 서버 응답을 실제로 검증하지 않습니다. 신뢰할 수 없는 데이터는 런타임 검증을 고려합니다.

## 컴포넌트 상태 모델링

나쁜 예:

```tsx
const [loading, setLoading] = useState(false);
const [data, setData] = useState<User | null>(null);
const [error, setError] = useState<string | null>(null);
```

가능하지만 조합이 애매해집니다. 예를 들어 loading이 true인데 data도 있고 error도 있을 수 있습니다.

더 명확한 예:

```tsx
type UserState =
  | { status: "loading" }
  | { status: "success"; data: User }
  | { status: "error"; message: string };
```

상태 조합을 타입으로 제한하면 UI(User Interface) 분기가 안정됩니다.

## 체크리스트

- [ ] props 타입을 명확히 정의한다.
- [ ] event 타입을 HTML 요소에 맞게 사용한다.
- [ ] null 초기 state에는 명시적 타입을 준다.
- [ ] API 응답 타입과 런타임 검증의 차이를 이해한다.
- [ ] 로딩, 성공, 실패 상태를 discriminated union으로 표현할 수 있다.
