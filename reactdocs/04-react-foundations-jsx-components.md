# React 기본, JSX, 컴포넌트

React는 UI(User Interface)를 컴포넌트 단위로 구성하는 라이브러리입니다. 핵심은 화면을 직접 수정하는 것이 아니라 상태를 바꾸면 UI가 그 상태에 맞게 다시 계산되도록 만드는 것입니다.

## React 애플리케이션 시작 구조

일반적인 진입점:

```jsx
import { createRoot } from "react-dom/client";
import App from "./App";

createRoot(document.getElementById("root")).render(<App />);
```

`App`은 루트 컴포넌트입니다. 실제 애플리케이션은 여러 컴포넌트가 트리 구조로 조립됩니다.

## JSX

JSX(JavaScript XML)는 JavaScript 안에서 UI 구조를 표현하는 문법입니다.

```jsx
function Greeting() {
  return <h1>Hello</h1>;
}
```

JSX는 HTML과 비슷하지만 JavaScript 표현식과 결합됩니다.

```jsx
function UserName({ name }) {
  return <span>{name}</span>;
}
```

## 컴포넌트

컴포넌트는 props를 받아 UI를 반환하는 함수입니다.

```jsx
function UserCard({ user }) {
  return (
    <article>
      <h2>{user.nickname}</h2>
      <p>{user.email}</p>
    </article>
  );
}
```

좋은 컴포넌트는 다음을 만족합니다.

- 입력 props가 명확합니다.
- 내부 상태가 최소입니다.
- 한 화면 책임을 너무 많이 갖지 않습니다.
- 로딩, 에러, 빈 상태를 표현할 위치가 명확합니다.

## props

props는 부모가 자식에게 전달하는 읽기 전용 입력입니다.

```jsx
function Button({ label, onClick, disabled }) {
  return (
    <button onClick={onClick} disabled={disabled}>
      {label}
    </button>
  );
}
```

자식이 props를 직접 바꾸면 안 됩니다. 변경이 필요하면 부모에게 이벤트로 알립니다.

## 조건부 렌더링

```jsx
if (isLoading) {
  return <p>불러오는 중</p>;
}

if (error) {
  return <p>문제가 발생했습니다</p>;
}

return <UserList users={users} />;
```

삼항 연산자:

```jsx
{isAdmin ? <AdminMenu /> : <UserMenu />}
```

조건이 복잡하면 JSX 안에서 억지로 처리하지 말고 변수나 컴포넌트로 분리합니다.

## 목록 렌더링과 key

```jsx
function UserList({ users }) {
  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.email}</li>
      ))}
    </ul>
  );
}
```

`key`는 React가 목록 항목의 정체성을 구분하는 값입니다. 인덱스를 key로 쓰면 정렬, 삭제, 삽입에서 UI 상태가 꼬일 수 있습니다.

## children

컴포넌트 사이에 넣은 내용을 props로 받을 수 있습니다.

```jsx
function Panel({ title, children }) {
  return (
    <section>
      <h2>{title}</h2>
      {children}
    </section>
  );
}
```

사용:

```jsx
<Panel title="공지">
  <p>서비스 점검 안내</p>
</Panel>
```

## 컴포넌트 분리 기준

분리하면 좋은 경우:

- 같은 UI 패턴이 반복됩니다.
- 한 컴포넌트가 너무 많은 상태를 가집니다.
- 조건부 렌더링이 길어집니다.
- 테스트하고 싶은 로직이 섞여 있습니다.
- 도메인 의미가 이름으로 드러나야 합니다.

분리하지 않아도 되는 경우:

- 한 번만 쓰이며 매우 짧습니다.
- 분리 이름이 오히려 의미를 흐립니다.
- props 전달만 복잡해지고 책임은 그대로입니다.

## 체크리스트

- [ ] JSX(JavaScript XML) 안에서 JavaScript 표현식을 사용할 수 있다.
- [ ] props를 읽기 전용 입력으로 다룬다.
- [ ] 목록 렌더링에 안정적인 key를 사용한다.
- [ ] 조건부 렌더링이 복잡하면 컴포넌트로 분리한다.
- [ ] 컴포넌트 이름이 UI 역할이나 도메인 의미를 드러낸다.

