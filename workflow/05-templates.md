# Templates

이 문서는 Notion, GitHub Issue, Pull Request에 사용할 수 있는 템플릿을 모읍니다. 그대로 복사해서 쓰기보다 작업 크기에 맞게 줄여 사용합니다.

## Feature Issue Template

```markdown
# feat: [기능 이름]

## Purpose

- 왜 이 기능이 필요한가:
- 사용자 또는 시스템이 얻는 가치:

## Scope

- 포함:
- 제외:

## Requirements

- [ ] 
- [ ] 
- [ ] 

## Acceptance Criteria

- [ ] 사용자가 어떤 행동을 하면 어떤 결과가 나온다.
- [ ] 실패 케이스가 정의되어 있다.
- [ ] 필요한 테스트가 추가되어 있다.

## Technical Notes

- API(Application Programming Interface):
- DB(Database):
- UI(User Interface):
- 보안:
- 성능:

## Related

- Notion:
- Parent:
- Depends on:
```

## Task Issue Template

```markdown
# task: [작업 이름]

## Goal

-

## Steps

- [ ] 
- [ ] 
- [ ] 

## Done When

- [ ] 결과물이 확인된다.
- [ ] 테스트 또는 수동 검증 결과가 있다.
- [ ] 관련 문서가 필요하면 업데이트했다.

## Related

- Feature:
- Notion:
```

## Bug Issue Template

```markdown
# fix: [버그 이름]

## Current Behavior

-

## Expected Behavior

-

## Reproduction

1. 
2. 
3. 

## Impact

- 사용자 영향:
- 데이터 영향:
- 운영 영향:

## Suspected Cause

-

## Fix Plan

- [ ] 재현 테스트 또는 재현 절차 작성
- [ ] 원인 확인
- [ ] 수정
- [ ] 회귀 테스트

## Verification

- [ ] 

## Related

-
```

## Docs Issue Template

```markdown
# docs: [문서 이름]

## Purpose

-

## Audience

- 읽는 사람:
- 필요한 배경지식:

## Content

- [ ] 
- [ ] 
- [ ] 

## Done When

- [ ] 문서가 현재 코드나 운영 방식과 맞다.
- [ ] 약어는 첫 언급에서 풀네임을 병기했다.
- [ ] 관련 문서와 링크가 연결되어 있다.

## Related

-
```

## Spike Issue Template

```markdown
# spike: [조사 주제]

## Question

-

## Context

-

## Options

### Option A

- 장점:
- 단점:

### Option B

- 장점:
- 단점:

## Decision

-

## Next Action

- [ ] 

## Timebox

- 시작:
- 종료:
```

## Refactor Issue Template

```markdown
# refactor: [리팩토링 이름]

## Problem

-

## Goal

- 외부 동작은 유지한다.
- 내부 구조를 개선한다.

## Scope

- 포함:
- 제외:

## Safety Checks

- [ ] 기존 테스트 확인
- [ ] 부족한 테스트 추가
- [ ] 리팩토링 전후 동작 비교

## Done When

- [ ] 외부 동작이 바뀌지 않았다.
- [ ] 테스트가 통과한다.
- [ ] 코드 책임이 더 명확해졌다.
```

## Pull Request Template

```markdown
## Summary

- 변경 목적:
- 핵심 변경:

## Related

- Closes #
- Related #
- Notion:

## Changes

- [ ] 
- [ ] 
- [ ] 

## Verification

- [ ] 테스트:
- [ ] 수동 확인:
- [ ] 문서 확인:

## Risk

- 영향 범위:
- 롤백 방법:
- 주의할 점:

## Screenshots

UI 변경이 있으면 추가한다.
```

## Notion Project Page Template

```markdown
# [프로젝트 이름]

## Goal

-

## Why Now

-

## Success Criteria

- [ ] 
- [ ] 
- [ ] 

## Scope

### In

-

### Out

-

## Milestones

| Milestone | Goal | GitHub |
| --- | --- | --- |
| 1 |  |  |

## Decisions

| Date | Decision | Reason |
| --- | --- | --- |
|  |  |  |

## Links

- GitHub Project:
- Issues:
- Pull Requests:

## Retrospective

- 잘 된 점:
- 어려웠던 점:
- 다음에 바꿀 점:
```

## Notion Learning Note Template

```markdown
# [학습 주제]

## Definition

-

## Why It Matters

-

## Example

-

## Mistakes

-

## Project Application

-

## Links

-
```

## 체크리스트

- [ ] 템플릿은 작업 크기에 맞게 줄여 사용한다.
- [ ] 빈 섹션을 그대로 남기지 않는다.
- [ ] issue에는 완료 기준을 반드시 넣는다.
- [ ] pull request에는 검증 결과를 반드시 넣는다.
- [ ] Notion에는 결정 이유와 회고를 남긴다.

