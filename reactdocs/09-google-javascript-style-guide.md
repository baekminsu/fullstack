# Google JavaScript Style Guide 적용 가이드

기준일: 2026-05-09  
공식 출처:

- [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)
- [Google HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.html)
- [Google Style Guides 목록](https://google.github.io/styleguide/)

Google의 공개 Style Guides 목록에는 별도 React 전용 스타일 가이드가 없습니다. 따라서 React 학습 코드에는 Google JavaScript Style Guide와 Google HTML/CSS Style Guide를 기본으로 적용하고, React 컴포넌트 규칙은 이 저장소의 로컬 규칙으로 보완합니다.

## 핵심 원칙

React 코드는 JavaScript, HTML(HyperText Markup Language), CSS(Cascading Style Sheets), JSX(JavaScript XML)가 섞여 보입니다. 스타일 기준도 이 네 영역을 함께 봐야 합니다.

우선순위:

1. JavaScript 문법과 모듈 규칙은 Google JavaScript Style Guide를 따릅니다.
2. HTML/CSS 작성은 Google HTML/CSS Style Guide를 따릅니다.
3. React 컴포넌트 구조는 이 저장소의 로컬 규칙을 따릅니다.
4. 자동 포매터와 린터로 반복 규칙을 강제합니다.

## JavaScript 모듈

Google JavaScript Style Guide는 named export를 사용하고 default export를 피하라고 설명합니다.

```javascript
// Good
export function formatDate(date) {
  return date.toISOString().slice(0, 10);
}

// Avoid
export default function formatDate(date) {
  return date.toISOString().slice(0, 10);
}
```

React 프로젝트 적용:

- 새 공용 함수, hook, component는 named export를 기본으로 둡니다.
- 기존 프로젝트가 page component에 default export를 쓰는 구조라면 해당 영역은 지역 스타일을 따릅니다.
- 같은 파일을 여러 번 import하지 않습니다.
- import alias는 충돌 해결 목적일 때만 사용합니다.

## 변수와 함수

기본 규칙:

- 재할당이 없으면 `const`를 사용합니다.
- 재할당이 필요할 때만 `let`을 사용합니다.
- `var`는 사용하지 않습니다.
- 이름은 설명적으로 짓고, 프로젝트 내부자만 아는 약어를 피합니다.

```javascript
const activeUsers = users.filter((user) => user.active);

let retryCount = 0;
retryCount += 1;
```

나쁜 이름:

```javascript
const usrCnt = users.length;
const tmp = calculateTotal(orderItems);
```

좋은 이름:

```javascript
const userCount = users.length;
const orderTotalPrice = calculateTotal(orderItems);
```

## 객체와 배열

React state를 다룰 때는 원본 객체와 배열을 직접 바꾸지 않습니다.

```javascript
const nextUser = {
  ...user,
  nickname: "new-name",
};

const nextItems = items.filter((item) => item.id !== removedItemId);
```

객체 literal은 의미 있는 순서로 정리합니다.

```javascript
const request = {
  email,
  password,
  nickname,
};
```

## 조건문과 중괄호

조건문은 읽기 쉽게 유지합니다.

```javascript
if (isLoading) {
  return <Loading />;
}

if (error) {
  return <ErrorPanel message={error.message} />;
}
```

복잡한 조건은 이름 있는 변수나 함수로 분리합니다.

```javascript
const canSubmit = form.email && form.password && !submitting;
```

## JSX와 컴포넌트

Google JavaScript Style Guide는 React 전용 규칙을 제공하지 않습니다. 이 저장소에서는 아래 규칙을 로컬 기준으로 사용합니다.

컴포넌트 이름:

- React component는 `UpperCamelCase`를 사용합니다.
- hook은 `use`로 시작합니다.
- event handler는 `handle` 또는 의미 있는 동사로 시작합니다.

```jsx
export function UserProfileCard({ user, onSelect }) {
  function handleClick() {
    onSelect(user.id);
  }

  return (
    <button type="button" onClick={handleClick}>
      {user.nickname}
    </button>
  );
}
```

props:

- boolean props를 많이 늘리지 않습니다.
- 컴포넌트 책임이 커지면 props를 줄이는 방향으로 분리합니다.
- 서버 데이터 요청과 순수 표시 컴포넌트를 구분합니다.

## HTML 규칙

Google HTML/CSS Style Guide는 HTML element, attribute, CSS selector, property를 소문자로 작성하는 규칙을 둡니다.

```html
<!-- Good -->
<button type="button">save</button>

<!-- Bad -->
<BUTTON TYPE="button">save</BUTTON>
```

React JSX에서는 HTML 속성 일부가 JavaScript 이름으로 바뀝니다.

```jsx
<label htmlFor="email">이메일</label>
<input id="email" className="form-input" />
```

JSX의 `htmlFor`, `className`은 React 문법이므로 예외입니다.

## CSS 규칙

Google HTML/CSS Style Guide 기준:

- 가능하면 HTTPS(HyperText Transfer Protocol Secure) 리소스를 사용합니다.
- CSS selector와 property는 소문자를 사용합니다.
- 불필요한 타입 선택자와 과도한 중첩을 피합니다.
- shorthand property는 의미가 명확할 때 사용합니다.

```css
.user-card {
  display: flex;
  gap: 8px;
  color: #1f2937;
}
```

React 프로젝트에서는 CSS Module, styled-components, Tailwind CSS 등 도구를 쓰더라도 이름과 구조의 일관성을 유지합니다.

## 주석과 문서화

주석은 코드가 말하지 못하는 이유를 설명합니다.

```javascript
// 서버 검증 메시지는 필드별로 내려오므로 기존 클라이언트 검증 결과를 덮어쓴다.
setFieldErrors(serverErrors);
```

나쁜 주석:

```javascript
// users를 map으로 돌린다.
users.map((user) => user.email);
```

## 자동화 도구

추천:

- ESLint: 코드 품질과 React hooks 규칙 검사
- Prettier: 포맷팅 통일
- TypeScript: 타입 안정성
- Testing Library: 사용자 행동 기반 테스트

스타일 규칙은 PR(Pull Request) 리뷰보다 도구에서 먼저 잡히게 합니다.

## 체크리스트

- [ ] 새 모듈은 named export를 기본으로 한다.
- [ ] `const`를 기본으로 쓰고 `var`는 쓰지 않는다.
- [ ] 이름은 의미를 드러내고 불명확한 약어를 피한다.
- [ ] React state는 불변 방식으로 업데이트한다.
- [ ] JSX에서 로딩, 에러, 빈 상태를 명확히 분리한다.
- [ ] HTML과 CSS는 소문자 중심 규칙을 따른다.
- [ ] 주석은 구현 설명보다 이유와 제약을 설명한다.
- [ ] ESLint와 Prettier로 스타일을 자동화한다.

