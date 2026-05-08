# TypeScript 프로젝트 커리큘럼

TypeScript 실력은 타입 오류를 없애는 능력이 아니라 데이터 구조를 정확히 모델링하고 변경에 강한 API(Application Programming Interface) 계약을 만드는 능력입니다.

## 1단계: JavaScript 코드에 타입 붙이기

주제:

- Todo 필터링 함수
- 가격 계산 함수
- 사용자 검색 함수

필수 적용:

- 기본 타입
- 함수 매개변수와 반환 타입
- 배열 타입
- 객체 타입

완료 기준:

- `any` 없이 작성합니다.
- 입력과 출력 타입을 보고 함수 목적을 이해할 수 있습니다.
- 잘못된 타입의 테스트 데이터가 컴파일에서 막힙니다.

## 2단계: API(Application Programming Interface) 타입 모델링

주제:

- 게시글 API 응답
- 페이지 응답
- 에러 응답

필수 적용:

- type alias
- interface
- generic
- union type

완료 기준:

- 목록 응답과 단건 응답을 재사용 타입으로 표현합니다.
- 검증 실패와 인증 실패를 구분합니다.
- 서버 응답 타입과 화면 상태 타입을 분리합니다.

## 3단계: React 폼 타입

주제:

- 회원가입 폼
- 게시글 작성 폼
- 검색 필터 폼

필수 적용:

- props 타입
- event 타입
- form value 타입
- form error 타입
- discriminated union

완료 기준:

- 입력값과 에러 메시지의 key가 어긋나지 않습니다.
- 제출 중, 성공, 실패 상태가 타입으로 구분됩니다.
- 컴포넌트 props가 명확합니다.

## 4단계: 도메인 상태 모델링

주제:

- 주문 상태
- 이슈 상태
- 권한 상태

필수 적용:

- literal type
- `as const`
- exhaustiveness check
- mapped type

완료 기준:

- 상태가 추가되면 처리 누락이 컴파일 오류로 드러납니다.
- 권한별 화면 표시 규칙이 타입과 함께 관리됩니다.
- 문자열 오타가 컴파일 단계에서 잡힙니다.

## 5단계: 품질 설정

주제:

- 기존 React 프로젝트 TypeScript 엄격화

필수 적용:

- `strict`
- `noUncheckedIndexedAccess`
- `exactOptionalPropertyTypes`
- ESLint
- `typecheck` 스크립트

완료 기준:

- CI(Continuous Integration)에서 타입 체크가 실행됩니다.
- `any` 사용 위치와 이유가 기록됩니다.
- 타입 오류를 없애기 위해 무의미한 type assertion을 남발하지 않습니다.
