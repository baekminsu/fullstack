# 성능, 테스트, 접근성

React 앱의 품질은 렌더링이 빠른지, 깨진 기능을 테스트가 잡는지, 키보드와 스크린리더 사용자도 사용할 수 있는지로 판단해야 합니다.

## 성능을 보기 전 원칙

측정 없이 최적화하지 않습니다. 먼저 사용자가 느끼는 문제를 확인합니다.

확인할 것:

- 초기 로딩이 느린가?
- 특정 입력에서 렌더링이 버벅이는가?
- 목록이 너무 큰가?
- 이미지나 번들 크기가 큰가?
- API(Application Programming Interface) 응답이 느린가?

## 불필요한 렌더링

부모가 렌더링되면 자식도 기본적으로 다시 렌더링됩니다.

`memo`는 props가 같으면 렌더링을 건너뛸 수 있습니다.

```jsx
const UserRow = memo(function UserRow({ user, onSelect }) {
  return <button onClick={() => onSelect(user.id)}>{user.email}</button>;
});
```

하지만 props로 매번 새 함수나 객체를 넘기면 효과가 줄어듭니다.

## useMemo와 useCallback

`useMemo`는 계산 결과를 기억합니다.

```jsx
const filteredUsers = useMemo(() => {
  return users.filter((user) => user.name.includes(keyword));
}, [users, keyword]);
```

`useCallback`은 함수 참조를 기억합니다.

```jsx
const handleSelect = useCallback((userId) => {
  setSelectedUserId(userId);
}, []);
```

남용하면 코드가 복잡해집니다. 실제 렌더링 비용이 있는 곳에 적용합니다.

## 코드 분할

페이지 단위로 코드를 늦게 불러올 수 있습니다.

```jsx
const AdminPage = lazy(() => import("./pages/AdminPage"));
```

관리자 페이지처럼 일부 사용자만 접근하는 화면에 유용합니다.

## 테스트 기준

테스트는 구현 세부사항보다 사용자 행동을 검증합니다.

좋은 테스트:

- 사용자가 보는 텍스트를 찾습니다.
- 사용자가 클릭하거나 입력하는 방식으로 조작합니다.
- 결과 화면이나 API 호출을 검증합니다.

나쁜 테스트:

- 내부 state 이름을 검증합니다.
- CSS(Cascading Style Sheets) 클래스만 검증합니다.
- 구현 변경에 너무 쉽게 깨집니다.

## 컴포넌트 테스트 예시

```jsx
test("검색어를 입력하면 목록이 필터링된다", async () => {
  render(<UserSearch users={users} />);

  await userEvent.type(screen.getByRole("textbox", { name: "검색" }), "kim");

  expect(screen.getByText("kim@example.com")).toBeInTheDocument();
});
```

role과 accessible name으로 요소를 찾으면 접근성도 함께 좋아집니다.

## 접근성 기본

A11y(Accessibility)는 장애가 있는 사용자도 서비스를 사용할 수 있게 만드는 품질입니다.

기본 원칙:

- 버튼은 `button` 요소를 사용합니다.
- 입력에는 label을 연결합니다.
- 이미지에는 의미 있는 alt를 제공합니다.
- 키보드만으로 조작할 수 있어야 합니다.
- 에러 메시지는 스크린리더가 알 수 있어야 합니다.

## label

```jsx
<label htmlFor="email">이메일</label>
<input id="email" type="email" />
```

## 에러 메시지

```jsx
<input
  id="email"
  aria-invalid={Boolean(error)}
  aria-describedby={error ? "email-error" : undefined}
/>
{error && (
  <p id="email-error" role="alert">
    {error}
  </p>
)}
```

## 체크리스트

- [ ] 측정 후 성능 최적화를 한다.
- [ ] `memo`, `useMemo`, `useCallback`을 목적 없이 남용하지 않는다.
- [ ] 테스트는 사용자 행동 중심으로 작성한다.
- [ ] 입력과 label을 연결한다.
- [ ] 키보드만으로 주요 흐름을 사용할 수 있다.
