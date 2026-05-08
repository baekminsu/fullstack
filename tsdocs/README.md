# TypeScript 상세 학습 문서

TypeScript는 JavaScript에 정적 타입 시스템을 더한 언어입니다. 목적은 타입을 많이 쓰는 것이 아니라 런타임 오류를 개발 단계에서 줄이고, 도메인 데이터 구조와 API(Application Programming Interface) 계약을 명확히 만드는 것입니다.

## 읽는 순서

1. [01-typescript-start.md](./01-typescript-start.md): TypeScript의 목적, 설치 개념, 기본 타입
2. [02-type-system-functions-objects.md](./02-type-system-functions-objects.md): 함수, 객체, 배열, 타입 별칭, 인터페이스
3. [03-unions-generics-narrowing.md](./03-unions-generics-narrowing.md): 유니언, 리터럴 타입, 제네릭, 타입 좁히기
4. [04-modules-tsconfig-tooling.md](./04-modules-tsconfig-tooling.md): 모듈, tsconfig, 빌드 도구, 린팅
5. [05-react-with-typescript.md](./05-react-with-typescript.md): React 컴포넌트와 hooks 타입
6. [06-advanced-patterns.md](./06-advanced-patterns.md): mapped type, conditional type, utility type
7. [07-ts-project-curriculum.md](./07-ts-project-curriculum.md): 단계별 프로젝트

## 학습 원칙

- `any`를 편하게 쓰기보다 타입을 모르는 이유를 먼저 찾습니다.
- API 응답 타입과 화면 상태 타입을 분리합니다.
- 타입은 런타임 검증을 대체하지 않습니다.
