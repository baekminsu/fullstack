# 브라우저, DOM(Document Object Model), 이벤트, Web API(Application Programming Interface)

React는 브라우저 위에서 동작합니다. React가 DOM(Document Object Model) 조작을 추상화해주더라도 브라우저 이벤트, 네트워크, 저장소, 렌더링 흐름을 이해해야 문제를 해결할 수 있습니다.

## DOM(Document Object Model)

DOM은 HTML 문서를 JavaScript가 다룰 수 있는 객체 트리로 표현한 것입니다.

```html
<main>
  <h1>Hello</h1>
  <button>Click</button>
</main>
```

브라우저는 이를 노드 트리로 만들고, JavaScript는 DOM API(Application Programming Interface)로 접근합니다.

```javascript
const button = document.querySelector("button");
button.textContent = "Save";
```

React에서는 직접 DOM을 자주 조작하지 않지만, focus 제어, 외부 라이브러리 연동, 스크롤 측정에서는 DOM 접근이 필요할 수 있습니다.

## 이벤트

이벤트는 사용자의 행동이나 브라우저 상태 변화를 나타냅니다.

```javascript
button.addEventListener("click", () => {
  console.log("clicked");
});
```

React에서는 JSX(JavaScript XML)에 이벤트 핸들러를 전달합니다.

```jsx
<button onClick={handleClick}>저장</button>
```

## 이벤트 버블링

자식에서 발생한 이벤트가 부모 방향으로 전파되는 현상입니다.

```jsx
<div onClick={() => console.log("parent")}>
  <button onClick={() => console.log("child")}>Click</button>
</div>
```

필요하면 전파를 막을 수 있습니다.

```jsx
function handleClick(event) {
  event.stopPropagation();
}
```

남용하면 접근성과 예측 가능성이 떨어집니다.

## form 기본 동작

HTML form은 제출 시 페이지를 새로고침할 수 있습니다.

```jsx
function SignupForm() {
  function handleSubmit(event) {
    event.preventDefault();
    // API 요청
  }

  return <form onSubmit={handleSubmit}>...</form>;
}
```

React 폼에서는 `preventDefault`를 통해 직접 제출 흐름을 제어합니다.

## fetch

```javascript
const response = await fetch("/api/posts", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    title: "hello",
    content: "content",
  }),
});
```

응답 처리:

```javascript
if (response.status === 204) {
  return null;
}

const data = await response.json();
```

항상 JSON(JavaScript Object Notation) 본문이 있다고 가정하면 204 응답에서 오류가 납니다.

## localStorage와 sessionStorage

`localStorage`는 브라우저에 오래 저장됩니다.

```javascript
localStorage.setItem("theme", "dark");
const theme = localStorage.getItem("theme");
```

`sessionStorage`는 탭 세션 동안 유지됩니다.

인증 토큰 저장 위치는 보안 요구사항에 따라 신중히 결정합니다. XSS(Cross Site Scripting) 위험이 있으면 localStorage의 토큰이 탈취될 수 있습니다.

## URLSearchParams

검색 조건을 URL(Uniform Resource Locator)에 담을 때 사용합니다.

```javascript
const params = new URLSearchParams();
params.set("page", "1");
params.set("keyword", "spring");

const url = `/api/posts?${params.toString()}`;
```

검색, 정렬, 페이지 번호는 URL에 두면 새로고침과 공유가 쉬워집니다.

## 브라우저 렌더링 기초

대략적인 흐름:

1. HTML을 파싱해 DOM을 만듭니다.
2. CSS(Cascading Style Sheets)를 파싱해 CSSOM(CSS Object Model)을 만듭니다.
3. 렌더 트리를 만듭니다.
4. 레이아웃을 계산합니다.
5. 페인트합니다.
6. 합성합니다.

JavaScript가 DOM을 자주 읽고 쓰면 레이아웃 계산이 반복될 수 있습니다. React에서도 불필요한 렌더링과 큰 DOM 트리는 성능 문제를 만들 수 있습니다.

## 체크리스트

- [ ] DOM(Document Object Model)을 객체 트리로 설명할 수 있다.
- [ ] 이벤트 버블링과 `stopPropagation`의 영향을 이해한다.
- [ ] form 제출에서 `preventDefault`가 필요한 이유를 안다.
- [ ] fetch 응답의 상태 코드와 본문 처리를 분리한다.
- [ ] URLSearchParams로 검색 조건을 URL에 보관할 수 있다.
