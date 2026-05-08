# 현재 도구 운영 방식: Notion + GitHub Projects

현재 사용하는 도구는 Notion과 GitHub Projects입니다. 두 도구의 역할을 분리해야 같은 내용을 두 번 관리하지 않습니다.

## 역할 분리

| 도구 | 주요 역할 | 넣을 내용 | 넣지 않을 내용 |
| --- | --- | --- | --- |
| Notion | 지식, 계획, 설계, 회고 | 학습 노트, 설계 메모, 회고, 참고 자료 | 매일 바뀌는 개발 상태 |
| GitHub Issues | 실행 가능한 작업 | bug, feature, task, docs 작업 | 장기 지식 문서 |
| GitHub Projects | 작업 상태판 | 우선순위, 상태, 담당, 마감 | 긴 설명 문서 전체 |
| Git commit | 변경 이력 | 실제 코드와 문서 변경 기록 | 논의 과정 전체 |
| Pull Request | 리뷰와 검증 | 변경 목적, 테스트, 관련 issue | 개인 학습 메모 전체 |

## Notion에 둘 것

Notion은 생각을 길게 정리하는 공간입니다.

추천 페이지:

- 학습 로드맵
- 프로젝트 아이디어
- 요구사항 초안
- 설계 메모
- API(Application Programming Interface) 초안
- 장애 회고
- 책 요약
- 기술 비교
- 월간 회고

Notion 문서 예시:

```text
제목: 게시글 검색 기능 설계

목표:
- 사용자가 제목과 내용으로 게시글을 검색한다.

결정:
- 첫 버전은 PostgreSQL ilike로 구현한다.
- 데이터가 커지면 full text search를 검토한다.

GitHub 연결:
- Feature issue: #24511
- Search API task: #24512
```

## GitHub Issues에 둘 것

GitHub Issue는 실행 가능한 작업으로 만듭니다.

좋은 issue:

- 제목만 보고 목적을 알 수 있습니다.
- 완료 기준이 있습니다.
- 관련 Notion 문서가 있으면 링크합니다.
- label로 작업 종류를 표시합니다.

Issue 제목 예:

```text
feat: 게시글 검색 API 추가
fix: 로그인 만료 응답 코드 수정
docs: PostgreSQL 인덱스 문서 보강
spike: React Query 도입 검토
```

## GitHub Projects 필드

추천 필드:

| 필드 | 값 |
| --- | --- |
| Status | Backlog, Ready, In Progress, In Review, Done, Blocked |
| Type | Epic, Feature, Task, Bug, Docs, Spike, Chore |
| Priority | P0, P1, P2, P3 |
| Area | Spring, React, TypeScript, PostgreSQL, Architecture, Workflow |
| Size | S, M, L |
| Target | 이번 주, 이번 달, 나중 |

## Status 정의

| Status | 의미 |
| --- | --- |
| Backlog | 언젠가 할 수 있지만 아직 준비되지 않음 |
| Ready | 바로 시작 가능 |
| In Progress | 작업 중 |
| In Review | 리뷰 또는 검증 중 |
| Done | 완료 기준 충족 |
| Blocked | 외부 의존성이나 결정 대기 |

## Priority 정의

| Priority | 의미 |
| --- | --- |
| P0 | 지금 막지 않으면 전체 진행이 멈춤 |
| P1 | 이번 주 안에 처리해야 함 |
| P2 | 중요하지만 일정 조정 가능 |
| P3 | 여유가 있을 때 처리 |

## Notion과 GitHub 연결 규칙

Notion에서 GitHub로:

```text
GitHub Issue:
- #24511 feat: 게시글 검색 API 추가
- #24512 test: 게시글 검색 통합 테스트 추가
```

GitHub에서 Notion으로:

```text
Reference:
- Notion: 게시글 검색 기능 설계
```

중요한 원칙:

- 실행 상태는 GitHub Projects가 진실의 원천입니다.
- 긴 배경과 회고는 Notion이 진실의 원천입니다.
- 같은 내용을 양쪽에 길게 복사하지 않습니다.

## GitHub Labels

추천 label:

- `type: feat`
- `type: fix`
- `type: docs`
- `type: test`
- `type: refactor`
- `type: chore`
- `area: spring`
- `area: react`
- `area: typescript`
- `area: postgres`
- `area: workflow`
- `priority: p0`
- `priority: p1`
- `priority: p2`
- `priority: p3`
- `status: blocked`

## 운영 루틴

매일:

- GitHub Projects에서 In Progress를 확인합니다.
- 오늘 끝낼 작업을 1개에서 3개만 고릅니다.
- Blocked 항목이 있으면 이유를 남깁니다.

매주:

- Done 항목을 회고합니다.
- Backlog를 정리합니다.
- Notion에 배운 점을 남깁니다.
- 너무 큰 issue를 작게 나눕니다.

매월:

- Notion 로드맵을 업데이트합니다.
- GitHub Projects의 오래된 Backlog를 삭제하거나 재정의합니다.
- 완료된 작업 중 포트폴리오에 남길 내용을 고릅니다.

## 체크리스트

- [ ] 실행 상태는 GitHub Projects에서 관리한다.
- [ ] 긴 지식과 회고는 Notion에 남긴다.
- [ ] GitHub Issue는 완료 가능한 크기로 만든다.
- [ ] Notion과 GitHub를 링크하되 같은 내용을 중복 작성하지 않는다.
- [ ] 매주 Backlog를 정리한다.

