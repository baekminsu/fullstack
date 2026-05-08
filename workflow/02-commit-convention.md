# Commit Convention

커밋 메시지는 변경 이력을 검색하고 이해하기 위한 최소 문서입니다. 이 저장소는 Conventional Commits 스타일을 기반으로 하되, 기존에 사용하던 작업 번호 연결 규칙을 유지합니다.

## 기본 형식

```text
<type>: <subject> (#<issue-or-task-number>)
```

번호가 없으면 뒤의 `(#번호)`를 생략합니다.

```text
docs: 작업 관리 workflow 문서 추가
```

번호가 있으면 반드시 붙입니다.

```text
feat: 아마노 주차 API 관제 연결 (#24321)
```

## Type 목록

| Type | 의미 | 예시 |
| --- | --- | --- |
| feat | 새로운 기능 추가 | `feat: 게시글 검색 API 추가 (#120)` |
| fix | 버그 수정 | `fix: 로그인 만료 시 500 응답 수정 (#121)` |
| docs | 문서 수정 | `docs: Spring 학습 문서 보강 (#122)` |
| style | 코드 스타일 변경 | `style: Java import 순서 정리 (#123)` |
| design | 사용자 UI 디자인 변경 | `design: 게시글 카드 간격 조정 (#124)` |
| test | 테스트 코드 추가 또는 수정 | `test: 주문 생성 테스트 추가 (#125)` |
| refactor | Production Code 리팩토링 | `refactor: 주문 생성 책임 분리 (#126)` |
| build | 빌드 파일 수정 | `build: Gradle 의존성 추가 (#127)` |
| ci | CI(Continuous Integration) 설정 파일 수정 | `ci: GitHub Actions 테스트 job 추가 (#128)` |
| perf | 성능 개선 | `perf: 게시글 목록 쿼리 인덱스 적용 (#129)` |
| chore | 자잘한 수정이나 관리 작업 | `chore: 패키지 버전 정리 (#130)` |
| rename | 파일 또는 폴더명 변경 | `rename: userdocs 폴더명을 memberdocs로 변경 (#131)` |
| remove | 파일 삭제 | `remove: 사용하지 않는 예제 파일 삭제 (#132)` |

기존에 오타로 적혔던 값은 최종 규칙에서 `refactor`와 `Task`로 통일합니다.

## Subject 규칙

Subject는 한 줄로 씁니다.

좋은 subject:

- 무엇을 바꿨는지 명확합니다.
- 너무 넓은 표현을 피합니다.
- 작업 번호 없이도 대략적인 변경이 보입니다.

나쁜 예:

```text
fix: 수정
docs: 문서
chore: 작업함
```

좋은 예:

```text
fix: 토큰 만료 시 인증 실패 응답으로 변환
docs: PostgreSQL 트랜잭션 학습 문서 추가
refactor: 회원가입 유스케이스 검증 책임 분리
```

## Issue 또는 Task 번호

GitHub Issue, GitHub Projects item, Notion task, 이전 Azure DevOps work item 번호가 있으면 subject 뒤에 붙입니다.

```text
feat: 게시글 검색 API 추가 (#24321)
fix: 결제 실패 시 주문 상태 롤백 (#24322)
docs: TypeScript 타입 가드 예제 추가 (#24323)
```

번호가 여러 개면 핵심 번호 하나만 subject에 붙이고, 자세한 연결은 pull request 본문에 적습니다.

```text
feat: 주문 취소 API 추가 (#24400)
```

pull request 본문:

```text
Related: #24400, #24401, #24402
```

## 커밋 크기

좋은 커밋:

- 하나의 의도를 가집니다.
- 되돌렸을 때 영향 범위가 예측됩니다.
- 리뷰어가 diff를 빠르게 이해할 수 있습니다.

나쁜 커밋:

- 기능 추가, 리팩토링, 포맷팅, 문서 수정이 한 번에 섞입니다.
- subject가 실제 변경과 다릅니다.
- 테스트 실패 상태로 남습니다.

## Type 선택 기준

헷갈릴 때는 아래 기준을 사용합니다.

- 사용자에게 새 동작이 생기면 `feat`
- 기대와 다른 동작을 고치면 `fix`
- 코드 동작 변화 없이 구조를 개선하면 `refactor`
- 테스트 코드만 바꾸면 `test`
- 문서만 바꾸면 `docs`
- CSS(Cascading Style Sheets)나 화면 배치처럼 UI(User Interface) 디자인만 바꾸면 `design`
- 포맷, 세미콜론, import 정리처럼 동작 없는 코드 스타일만 바꾸면 `style`
- 빌드 도구, 패키지 설정은 `build`
- GitHub Actions 같은 자동화 설정은 `ci`
- 속도, 메모리, 쿼리 성능 개선은 `perf`
- 기타 관리 작업은 `chore`

## 예시

```text
feat: 아마노 주차 API 관제 연결 (#24321)
fix: 회원가입 이메일 중복 응답 코드 수정 (#24322)
docs: Google Java Style Guide 적용 문서 추가 (#24323)
style: Java import 순서 정리 (#24324)
design: 로그인 화면 입력 간격 조정 (#24325)
test: 게시글 검색 통합 테스트 추가 (#24326)
refactor: 주문 생성 유스케이스 책임 분리 (#24327)
build: Spring Boot validation 의존성 추가 (#24328)
ci: pull request 테스트 workflow 추가 (#24329)
perf: 게시글 목록 keyset pagination 적용 (#24330)
chore: 개발 환경 변수 예시 정리 (#24331)
rename: workflow 문서 파일명 정리 (#24332)
remove: 사용하지 않는 샘플 SQL 삭제 (#24333)
```

## 체크리스트

- [ ] type이 변경 성격과 맞다.
- [ ] subject가 구체적이다.
- [ ] issue 또는 task 번호가 있으면 `(#번호)`를 붙였다.
- [ ] 번호가 없으면 억지로 만들지 않았다.
- [ ] 한 커밋에 서로 다른 의도를 섞지 않았다.
