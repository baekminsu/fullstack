# 모듈, tsconfig, 도구 설정

TypeScript 프로젝트는 언어 문법만큼 설정이 중요합니다. `tsconfig.json`, 모듈 해석, 빌드 도구, 린터 설정이 팀의 코드 품질을 좌우합니다.

## 모듈 export와 import

named export:

```typescript
export function formatDate(date: Date): string {
  return date.toISOString().slice(0, 10);
}
```

import:

```typescript
import { formatDate } from "./formatDate";
```

default export:

```typescript
export default function UserPage() {
  return null;
}
```

팀에서는 페이지 컴포넌트는 default, 유틸과 타입은 named처럼 기준을 정할 수 있습니다.

## type-only import

타입만 가져올 때 사용합니다.

```typescript
import type { User } from "./types";
```

런타임 번들에 필요 없는 import임을 명확히 합니다.

## tsconfig 기본

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "strict": true,
    "jsx": "react-jsx",
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true
  },
  "include": ["src"]
}
```

중요 옵션:

- `strict`: 엄격한 타입 검사
- `jsx`: JSX(JavaScript XML) 변환 방식
- `moduleResolution`: import 경로 해석 방식
- `noUncheckedIndexedAccess`: 배열과 객체 인덱스 접근의 undefined 가능성 반영
- `exactOptionalPropertyTypes`: 선택 속성을 더 정확히 검사

## path alias

깊은 상대 경로를 줄입니다.

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

사용:

```typescript
import { Button } from "@/components/Button";
```

빌드 도구도 같은 alias를 알아야 합니다.

## ESLint와 Prettier

ESLint는 코드 문제를 찾고, Prettier는 포맷을 통일합니다.

검사 대상:

- 사용하지 않는 변수
- hooks 규칙 위반
- 부정확한 dependency array
- 위험한 any
- import 순서

포맷과 품질 규칙을 섞으면 팀 논쟁이 늘어납니다. Prettier는 스타일, ESLint는 문제 탐지로 역할을 나눕니다.

## 빌드와 타입 체크

개발 서버가 실행된다고 타입 체크가 모두 통과했다는 뜻은 아닙니다.

일반 스크립트:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc -b && vite build",
    "typecheck": "tsc --noEmit",
    "lint": "eslint ."
  }
}
```

CI(Continuous Integration)에서는 `typecheck`, `lint`, `test`, `build`를 분리해 실패 원인을 명확히 합니다.

## 환경 변수 타입

환경 변수는 문자열입니다.

```typescript
const apiBaseUrl = import.meta.env.VITE_API_BASE_URL;
```

필수 환경 변수가 없으면 애플리케이션 시작 시점에 실패시키는 것이 좋습니다.

```typescript
function requireEnv(name: string, value: string | undefined): string {
  if (!value) {
    throw new Error(`Missing env: ${name}`);
  }
  return value;
}
```

## 체크리스트

- [ ] type-only import를 사용할 수 있다.
- [ ] strict 기반 tsconfig를 설명할 수 있다.
- [ ] path alias를 빌드 도구와 함께 설정해야 함을 안다.
- [ ] typecheck와 build의 차이를 이해한다.
- [ ] 환경 변수를 런타임에서 검증한다.

