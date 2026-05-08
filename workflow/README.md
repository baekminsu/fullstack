# Workflow

이 폴더는 Notion, GitHub Projects, Git commit, branch, pull request, issue, task, 문서 템플릿을 하나의 작업 흐름으로 정리합니다.

현재 사용하는 도구는 Notion과 GitHub Projects입니다. 다만 작업 단위와 완료 기준은 도구에 종속되지 않게 정의합니다. 나중에 Azure DevOps, Jira, Linear 같은 도구로 옮기더라도 작업 분류, 커밋 규칙, 완료 기준은 최대한 유지할 수 있어야 합니다.

## 읽는 순서

1. [01-work-item-taxonomy.md](./01-work-item-taxonomy.md): 작업 단위 정의
2. [02-commit-convention.md](./02-commit-convention.md): 커밋 메시지 규칙
3. [03-branch-pr-convention.md](./03-branch-pr-convention.md): 브랜치와 pull request 규칙
4. [04-current-tools-notion-github.md](./04-current-tools-notion-github.md): 현재 Notion과 GitHub Projects 운영 방식
5. [05-templates.md](./05-templates.md): issue, task, bug, docs, spike 템플릿
6. [06-definition-of-done.md](./06-definition-of-done.md): 완료 기준
7. [07-future-migration-notes.md](./07-future-migration-notes.md): 미래 도구 전환 매핑

## 기본 원칙

- Notion은 생각, 지식, 설계, 회고를 쌓는 공간입니다.
- GitHub Projects는 실제 실행 상태와 issue 추적 공간입니다.
- Git commit은 변경 이력을 작고 명확하게 남기는 기록입니다.
- Issue와 task 번호가 있으면 commit, branch, pull request에 연결합니다.
- 번호가 없으면 억지로 만들지 않고, 나중에 연결 가능한 문맥을 남깁니다.

## 추천 흐름

1. Notion에서 문제, 요구사항, 참고 자료를 정리합니다.
2. GitHub Issue로 실행 가능한 작업을 만듭니다.
3. GitHub Projects에서 상태, 우선순위, 담당 범위를 관리합니다.
4. branch를 만들고 작업합니다.
5. commit은 작업 단위로 작게 남깁니다.
6. pull request에서 변경 목적, 검증 결과, 관련 issue를 연결합니다.
7. 완료 후 Notion에 회고나 지식으로 남길 내용을 정리합니다.

