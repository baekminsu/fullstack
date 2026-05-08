# state, effect, form

React 애플리케이션의 대부분 버그는 상태가 어디에 있어야 하는지 잘못 판단해서 생깁니다. state와 effect는 강력하지만 남용하면 데이터 흐름이 흐려집니다.

## useState

상태를 선언합니다.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );
}
```

이전 상태를 기준으로 업데이트할 때는 함수형 업데이트를 사용합니다.

```jsx
setCount((prev) => prev + 1);
```

## 상태 위치

상태를 둘 위치:

- 한 컴포넌트에서만 쓰면 그 컴포넌트에 둡니다.
- 형제 컴포넌트가 공유하면 공통 부모로 올립니다.
- URL(Uniform Resource Locator)에 남아야 하면 라우터 상태나 query string에 둡니다.
- 서버에서 온 데이터는 서버 상태 관리 대상으로 봅니다.

상태를 전역으로 올리는 것은 마지막 선택지입니다.

## 객체 상태 업데이트

```jsx
const [form, setForm] = useState({
  email: "",
  password: "",
});

function handleEmailChange(event) {
  setForm((prev) => ({
    ...prev,
    email: event.target.value,
  }));
}
```

기존 객체를 직접 수정하지 않습니다.

## useEffect

effect는 렌더링 결과가 화면에 반영된 뒤 실행되는 부수 효과를 다룹니다.

```jsx
useEffect(() => {
  document.title = `count: ${count}`;
}, [count]);
```

대표 용도:

- 외부 시스템 구독
- 타이머 등록
- 브라우저 API(Application Programming Interface) 호출
- 컴포넌트 생명주기와 연결된 작업

서버 데이터 요청은 전용 라이브러리를 쓰는 편이 더 나을 때가 많습니다.

## cleanup

```jsx
useEffect(() => {
  const id = setInterval(() => {
    console.log("tick");
  }, 1000);

  return () => {
    clearInterval(id);
  };
}, []);
```

이벤트 리스너, 타이머, 구독은 정리하지 않으면 메모리 누수나 중복 실행을 만들 수 있습니다.

## dependency array

```jsx
useEffect(() => {
  loadUser(userId);
}, [userId]);
```

의존성 배열은 effect가 어떤 값 변화에 반응해야 하는지 말합니다. 경고를 억지로 끄기보다 effect 내부에서 쓰는 값을 정확히 파악합니다.

## controlled form

입력값을 React state로 관리합니다.

```jsx
function SignupForm() {
  const [email, setEmail] = useState("");

  return (
    <input
      value={email}
      onChange={(event) => setEmail(event.target.value)}
    />
  );
}
```

장점:

- 입력값 검증이 쉽습니다.
- 제출 전 상태를 확인하기 쉽습니다.
- 서버 검증 메시지와 연결하기 쉽습니다.

## form submit

```jsx
function SignupForm() {
  const [form, setForm] = useState({ email: "", password: "" });
  const [error, setError] = useState(null);

  async function handleSubmit(event) {
    event.preventDefault();
    setError(null);

    try {
      await signup(form);
    } catch (error) {
      setError(error.message);
    }
  }

  return (
    <form onSubmit={handleSubmit}>
      <input value={form.email} onChange={handleEmailChange} />
      <input value={form.password} onChange={handlePasswordChange} />
      {error && <p role="alert">{error}</p>}
      <button type="submit">가입</button>
    </form>
  );
}
```

## useReducer

상태 전이가 복잡하면 reducer를 고려합니다.

```jsx
function reducer(state, action) {
  switch (action.type) {
    case "changeEmail":
      return { ...state, email: action.value };
    case "submitStart":
      return { ...state, submitting: true, error: null };
    case "submitError":
      return { ...state, submitting: false, error: action.error };
    default:
      return state;
  }
}
```

여러 state가 함께 바뀌는 폼, 마법사 UI, 복잡한 필터에서 유용합니다.

## 체크리스트

- [ ] state를 필요한 가장 가까운 위치에 둔다.
- [ ] 객체와 배열 상태를 불변 방식으로 업데이트한다.
- [ ] effect는 외부 시스템과 동기화할 때 사용한다.
- [ ] effect cleanup이 필요한 작업을 구분한다.
- [ ] 폼의 로딩, 검증 실패, 제출 실패 상태를 표현한다.

