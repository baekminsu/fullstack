# JavaScript 기본

JavaScript는 브라우저와 Node.js에서 실행되는 동적 타입 언어입니다. React 개발자는 문법뿐 아니라 값의 비교, 참조, 실행 컨텍스트, 비동기 흐름을 이해해야 합니다.

## 실행 환경

JavaScript는 주로 두 환경에서 실행됩니다.

- Browser: 화면, DOM(Document Object Model), 이벤트, Web API(Application Programming Interface)를 다룹니다.
- Node.js: 서버, 빌드 도구, 테스트 도구, CLI(Command Line Interface)를 실행합니다.

React 개발에서는 브라우저 런타임과 Node.js 기반 개발 도구를 함께 씁니다.

## 변수 선언

`let`은 재할당 가능한 변수입니다.

```javascript
let count = 0;
count = 1;
```

`const`는 재할당할 수 없는 변수입니다.

```javascript
const name = "minsu";
```

객체를 `const`로 선언해도 내부 속성은 바뀔 수 있습니다.

```javascript
const user = { name: "kim" };
user.name = "lee";
```

React에서는 값을 직접 바꾸기보다 새 객체를 만들어 상태를 업데이트합니다.

## var를 피하는 이유

`var`는 함수 스코프이고 호이스팅 동작이 직관적이지 않습니다.

```javascript
console.log(name); // undefined
var name = "kim";
```

현대 JavaScript에서는 기본적으로 `const`를 쓰고, 재할당이 필요할 때만 `let`을 씁니다.

## 기본 타입

원시 타입:

- `string`
- `number`
- `boolean`
- `undefined`
- `null`
- `symbol`
- `bigint`

참조 타입:

- object
- array
- function

## undefined와 null

`undefined`는 값이 아직 할당되지 않았다는 의미입니다.

```javascript
let name;
console.log(name); // undefined
```

`null`은 의도적으로 값이 없음을 표현합니다.

```javascript
const selectedUser = null;
```

API 응답에서는 필드가 없을 수도 있고 null일 수도 있습니다. React 화면에서는 두 경우를 구분해서 처리할 때가 많습니다.

## number

JavaScript의 일반 숫자는 정수와 실수를 모두 `number`로 다룹니다.

```javascript
const price = 10000;
const rate = 0.1;
```

부동소수점 오차:

```javascript
console.log(0.1 + 0.2); // 0.30000000000000004
```

금액 계산은 서버에서 정수 단위로 처리하거나, 프론트에서는 표시 용도로만 다루는 편이 안전합니다.

## 비교 연산자

`===`는 타입 변환 없이 비교합니다.

```javascript
console.log(1 === "1"); // false
```

`==`는 암묵적 타입 변환을 수행합니다.

```javascript
console.log(1 == "1"); // true
```

실무에서는 예측 가능한 `===`를 사용합니다.

## Truthy와 Falsy

Falsy 값:

- `false`
- `0`
- `""`
- `null`
- `undefined`
- `NaN`

주의:

```javascript
const count = 0;
if (!count) {
  console.log("count가 없음처럼 처리됨");
}
```

0이 의미 있는 값이면 `count == null`처럼 null과 undefined만 검사하는 편이 낫습니다.

## 템플릿 리터럴

```javascript
const message = `안녕하세요, ${name}님`;
```

여러 줄 문자열에도 사용할 수 있습니다.

```javascript
const html = `
  <div>
    <h1>${title}</h1>
  </div>
`;
```

React에서는 JSX(JavaScript XML)를 사용하므로 HTML 문자열을 직접 만드는 경우는 적습니다.

## 모듈

내보내기:

```javascript
export function add(a, b) {
  return a + b;
}
```

가져오기:

```javascript
import { add } from "./math";
```

기본 내보내기:

```javascript
export default function Button() {
  return null;
}
```

팀에서는 named export와 default export 중 하나의 기준을 정하면 import 이름 혼란이 줄어듭니다.

## 체크리스트

- [ ] Browser와 Node.js 실행 환경의 차이를 설명할 수 있다.
- [ ] `const`, `let`, `var`의 차이를 알고 `var`를 피한다.
- [ ] `undefined`와 `null`을 구분한다.
- [ ] `==`보다 `===`를 사용한다.
- [ ] 0, 빈 문자열, null을 같은 의미로 처리하지 않는다.

