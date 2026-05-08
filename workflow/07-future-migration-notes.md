# Future Migration Notes

현재는 Notion과 GitHub Projects를 사용합니다. 하지만 나중에 Azure DevOps, Jira, Linear 같은 도구로 옮기더라도 작업 체계 전체를 다시 만들 필요는 없게 준비합니다.

## 핵심 전략

도구에 묶이는 것:

- 화면 이름
- 상태 필드 이름
- 자동화 방식
- 권한 모델
- 리포트 기능

도구에 묶이면 안 되는 것:

- 작업 단위 정의
- commit convention
- branch convention
- 완료 기준
- issue와 pull request 템플릿의 핵심 질문
- Notion에 남기는 결정과 회고 방식

## 공통 개념 매핑

| 공통 개념 | GitHub Projects | Notion | Azure DevOps | Jira | Linear |
| --- | --- | --- | --- | --- | --- |
| Epic | Issue 또는 project item | Project page | Epic | Epic | Project 또는 Initiative |
| Feature | Issue | Feature page | Feature | Story | Issue |
| Task | Issue 또는 checklist | Task row | Task | Task | Sub-issue |
| Bug | Issue + bug label | Bug row | Bug | Bug | Issue + bug label |
| Docs | Issue + docs label | Docs page | Task | Task | Issue |
| Spike | Issue + spike label | Research page | Task | Spike | Issue |
| Status | Project status | Status property | State | Status | Status |
| Priority | Priority field | Priority property | Priority | Priority | Priority |

## 상태 매핑

| 공통 상태 | GitHub Projects | Azure DevOps | Jira | Linear |
| --- | --- | --- | --- | --- |
| Backlog | Backlog | New | Backlog | Backlog |
| Ready | Ready | Approved | Selected for Development | Ready |
| In Progress | In Progress | Active | In Progress | In Progress |
| In Review | In Review | Resolved 또는 Review | In Review | In Review |
| Done | Done | Closed | Done | Done |
| Blocked | Blocked | Blocked tag | Blocked flag | Blocked |

도구마다 정확한 상태 이름은 달라도 흐름은 유지합니다.

## Commit 규칙 유지

도구를 바꿔도 commit은 유지됩니다.

```text
feat: 게시글 검색 API 추가 (#24511)
fix: 로그인 만료 응답 코드 수정 (#24512)
docs: 작업 관리 문서 보강 (#24513)
```

Azure DevOps로 돌아가면 번호 형식이 다를 수 있습니다.

```text
feat: 게시글 검색 API 추가 (#24511)
feat: 게시글 검색 API 추가 (AB#24511)
```

팀이나 도구 자동 연결 규칙에 맞춰 하나만 선택합니다. 중요한 것은 작업 번호가 변경 이력에 남는 것입니다.

## Notion 유지 여부

도구를 옮겨도 Notion은 계속 지식 저장소로 유지할 수 있습니다.

유지하면 좋은 것:

- 학습 노트
- 설계 결정
- 장애 회고
- 책 요약
- 프로젝트 회고

이관할 수 있는 것:

- 실행 task
- bug 상태
- sprint 계획
- 담당자와 마감일

실행 상태는 새 작업관리 도구로 옮기고, 장기 지식은 Notion에 남기는 방식이 가장 비용이 낮습니다.

## 이전 준비 체크리스트

도구를 바꾸기 전에 확인합니다.

- [ ] 작업 단위 정의가 문서화되어 있다.
- [ ] 현재 GitHub Projects 필드 목록을 export할 수 있다.
- [ ] Notion과 GitHub Issue 연결 방식이 정리되어 있다.
- [ ] commit convention이 도구와 독립적이다.
- [ ] 완료 기준이 특정 도구 UI에 묶여 있지 않다.
- [ ] 새 도구의 상태와 현재 상태를 매핑했다.
- [ ] migration 후 2주 동안 중복 관리 기간을 둔다.

## migration 시 피할 것

- 모든 과거 데이터를 완벽하게 옮기려 하지 않습니다.
- 오래된 Done 작업까지 새 도구에서 다시 관리하지 않습니다.
- 도구 기능에 맞추려고 작업 단위 정의를 자주 바꾸지 않습니다.
- Notion과 새 도구에 같은 내용을 길게 중복 작성하지 않습니다.

## 권장 이전 방식

1. 새 도구에 공통 상태를 먼저 만듭니다.
2. 현재 진행 중인 작업만 옮깁니다.
3. 다음 달 작업부터 새 도구에서 시작합니다.
4. 완료된 과거 작업은 GitHub commit과 pull request 링크로 보존합니다.
5. Notion에는 migration 결정과 매핑표만 남깁니다.

## 체크리스트

- [ ] 미래 도구 전환은 가능하게 하되, 현재 운영은 Notion과 GitHub Projects에 맞춘다.
- [ ] 도구 이름보다 작업 단위와 완료 기준을 우선한다.
- [ ] 실행 상태와 장기 지식의 저장 위치를 분리한다.
- [ ] migration은 현재 진행 중인 작업부터 작게 시작한다.

