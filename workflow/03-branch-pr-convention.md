# Branch와 Pull Request Convention

branch와 pull request는 작업을 리뷰 가능한 단위로 나누는 장치입니다. commit이 변경 이력이라면, pull request는 변경의 목적과 검증 결과를 설명하는 문서입니다.

## Branch 이름

기본 형식:

```text
<type>/<issue-number>-<short-description>
```

예시:

```text
feat/24321-amano-parking-control-api
fix/24322-login-expired-response
docs/24323-spring-study-roadmap
refactor/24324-order-usecase
```

issue 번호가 없으면:

```text
docs/workflow-convention
chore/update-readme-links
```

## Branch type

commit type과 맞춥니다.

- `feat`
- `fix`
- `docs`
- `style`
- `design`
- `test`
- `refactor`
- `build`
- `ci`
- `perf`
- `chore`
- `rename`
- `remove`

## Branch 설명 규칙

좋은 이름:

- 짧습니다.
- 영어 소문자와 hyphen을 사용합니다.
- issue 번호가 있으면 포함합니다.
- 변경 목적이 보입니다.

나쁜 이름:

```text
work
test
minsu
fix-bug
new
```

좋은 이름:

```text
fix/24510-refresh-token-expiration
feat/24511-post-search
docs/workflow-templates
```

## Pull Request 제목

pull request 제목은 commit subject와 같은 형식을 사용합니다.

```text
feat: 게시글 검색 API 추가 (#24511)
```

issue 번호가 없으면:

```text
docs: workflow 규칙 문서 추가
```

## Pull Request 본문

기본 구조:

```markdown
## Summary

- 변경 목적:
- 핵심 변경:

## Related

- Closes #
- Related #

## Verification

- [ ] 테스트 실행:
- [ ] 수동 확인:
- [ ] 문서 확인:

## Notes

-
```

## Pull Request 크기

좋은 pull request:

- 한 가지 목적을 가집니다.
- 리뷰어가 15분에서 30분 안에 핵심을 이해할 수 있습니다.
- 테스트와 검증 방법이 적혀 있습니다.
- 관련 issue와 연결됩니다.

나쁜 pull request:

- 여러 feature가 섞입니다.
- 리팩토링과 기능 변경이 분리되지 않습니다.
- 검증 결과가 없습니다.
- 제목만 보고 무엇을 했는지 알 수 없습니다.

## Merge 기준

main branch에 들어가기 전 확인합니다.

- 테스트가 통과했습니다.
- lint 또는 formatter가 통과했습니다.
- 문서가 필요한 변경이면 문서를 업데이트했습니다.
- DB(Database) migration이 있으면 롤백 또는 복구 관점을 확인했습니다.
- 관련 issue가 연결되어 있습니다.
- 리뷰 피드백을 반영했습니다.

## main branch

main branch는 항상 실행 가능한 상태를 목표로 합니다.

권장:

- 직접 push보다 pull request merge를 기본으로 합니다.
- 작업 branch에서 작게 commit합니다.
- main 최신 변경을 주기적으로 반영합니다.
- 배포 가능한 단위로 merge합니다.

## 체크리스트

- [ ] branch 이름에 type과 issue 번호를 넣었다.
- [ ] issue 번호가 없으면 짧은 설명을 넣었다.
- [ ] pull request 제목이 commit convention과 맞다.
- [ ] pull request 본문에 Summary, Related, Verification을 적었다.
- [ ] main branch는 깨진 상태로 두지 않는다.

