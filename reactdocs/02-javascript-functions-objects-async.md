# 함수, 객체, 배열, 비동기

React 코드는 결국 JavaScript 함수와 객체의 조합입니다. 참조 동일성, 불변 업데이트, 비동기 흐름을 모르면 렌더링 버그와 상태 버그가 자주 발생합니다.

## 함수 선언과 함수 표현식

함수 선언:

```javascript
function add(a, b) {
  return a + b;
}
```

함수 표현식:

```javascript
const add = function (a, b) {
  return a + b;
};
```

화살표 함수:

```javascript
const add = (a, b) => a + b;
```

React 컴포넌트와 이벤트 핸들러에서는 화살표 함수를 자주 사용합니다.

## 매개변수 기본값

```javascript
function createUser(name, role = "USER") {
  return { name, role };
}
```

기본값은 undefined일 때만 적용됩니다. null을 넘기면 null 그대로 들어갑니다.

## 구조 분해 할당

객체:

```javascript
const user = { id: 1, email: "a@example.com" };
const { id, email } = user;
```

배열:

```javascript
const [first, second] = ["a", "b"];
```

React props에서 자주 사용합니다.

```javascript
function UserCard({ user, onSelect }) {
  return <button onClick={() => onSelect(user.id)}>{user.email}</button>;
}
```

## 전개 연산자

배열 복사:

```javascript
const nextItems = [...items, newItem];
```

객체 복사:

```javascript
const nextUser = { ...user, nickname: "new" };
```

중첩 객체는 얕게 복사됩니다.

```javascript
const next = { ...user };
next.profile.name = "changed"; // 원본 profile도 영향받을 수 있음
```

깊은 중첩 상태가 많다면 상태 구조를 평평하게 만들거나, 서버 상태 관리 도구를 검토합니다.

## 배열 메서드

`map`:

```javascript
const names = users.map((user) => user.name);
```

`filter`:

```javascript
const activeUsers = users.filter((user) => user.active);
```

`find`:

```javascript
const selected = users.find((user) => user.id === selectedId);
```

`reduce`:

```javascript
const total = orderItems.reduce((sum, item) => sum + item.price, 0);
```

React 렌더링에서는 원본 배열을 변경하는 `push`, `sort`, `splice`를 조심합니다. 새 배열을 만들어 업데이트합니다.

## 객체 비교

객체는 값이 같아 보여도 참조가 다르면 다릅니다.

```javascript
console.log({ id: 1 } === { id: 1 }); // false
```

React의 memoization과 dependency array는 참조 동일성에 영향을 받습니다.

## this

일반 함수의 `this`는 호출 방식에 따라 달라집니다.

```javascript
const user = {
  name: "kim",
  printName() {
    console.log(this.name);
  },
};
```

화살표 함수는 자신만의 `this`를 갖지 않습니다. 현대 React 함수 컴포넌트에서는 class component보다 hooks를 사용하므로 `this`를 직접 다루는 일이 줄었습니다.

## Promise

비동기 작업의 성공과 실패를 표현합니다.

```javascript
fetch("/api/users")
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((error) => console.error(error));
```

## async와 await

```javascript
async function loadUsers() {
  try {
    const response = await fetch("/api/users");
    if (!response.ok) {
      throw new Error("사용자 목록 조회 실패");
    }
    return await response.json();
  } catch (error) {
    console.error(error);
    throw error;
  }
}
```

`await`는 Promise가 끝날 때까지 현재 async 함수의 진행을 멈춥니다. 브라우저 전체가 멈추는 것은 아닙니다.

## 에러 처리

fetch는 HTTP(HyperText Transfer Protocol) 상태 코드가 404나 500이어도 네트워크 요청 자체가 성공하면 reject하지 않습니다.

```javascript
const response = await fetch("/api/users/1");
if (!response.ok) {
  throw new Error(`HTTP error ${response.status}`);
}
```

서버 API(Application Programming Interface) 연동 코드는 성공 응답, 검증 실패, 인증 실패, 네트워크 실패를 구분해야 합니다.

## 체크리스트

- [ ] 함수 선언, 함수 표현식, 화살표 함수의 차이를 설명할 수 있다.
- [ ] 객체와 배열을 불변 방식으로 업데이트한다.
- [ ] 참조 동일성이 React 렌더링에 영향을 주는 이유를 안다.
- [ ] Promise, async, await로 API 요청을 작성할 수 있다.
- [ ] fetch의 HTTP 에러 처리 방식을 이해한다.
