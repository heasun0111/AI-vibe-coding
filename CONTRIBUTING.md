# Git 브랜치·PR 정책

AI(Claude Code)를 활용한 "바이브 코딩"으로 GitHub 저장소를 운영할 때 일반적으로 권장되는 정책이다.
사람의 실시간 리뷰가 상대적으로 적을 수 있는 만큼, **CI(자동 검증)를 병합의 필수 관문으로 삼는 것**이
핵심이다.

## 브랜치 전략

- **`main`**: 보호된 브랜치. 항상 배포/제출 가능한 상태를 유지한다. **직접 커밋·푸시하지 않는다** —
  반드시 Pull Request(PR)를 통해서만 병합한다.
- 작업 브랜치는 목적을 접두어로 구분해 만든다:
  - `feature/<설명>` — 신규 기능/산출물 추가
  - `fix/<설명>` — 버그 수정
  - `docs/<설명>` — 문서·스킬·`CLAUDE.md` 등 문서성 변경
  - `refactor/<설명>` — 동작 변경 없는 리팩터링
  - `test/<설명>` — 테스트만 추가/수정
  - `chore/<설명>` — 빌드/CI/설정 등 그 외
  - 예: `feature/swe6-verification-spec`, `fix/tdd-branch-coverage-script`

## 커밋 메시지 — Conventional Commits

`<type>(<scope>): <설명>` 형식을 따른다.

- `type`: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `style`, `perf` 중 하나.
- 예: `feat(swe5): add integration test coverage gate`, `docs(claude): clarify skill boundaries`.
- Claude Code 등 AI가 만든 커밋은 기존 관례대로 `Co-Authored-By` 트레일러를 유지한다.

## Pull Request 규칙

- 모든 변경은 PR로 진행한다. `.github/pull_request_template.md` 체크리스트를 채운다.
- PR 제목도 Conventional Commits 형식을 따른다(`ci.yml`의 `pr-title` 잡이 자동 검사한다).
- 리뷰: 협업 시 최소 1인 승인, 솔로 운영 시에도 PR 템플릿의 개발 생명주기 체크리스트로 자체 검토를
  대신하지 않는다 — 병합 전 반드시 체크리스트를 실제로 확인한다.
- **CI(워크플로 `CI`의 `Lint, quality gates, and tests` 잡)가 통과해야만 병합할 수 있다** — 아래
  "GitHub 저장소 설정"을 켜야 실제로 강제된다.
- 병합은 Squash merge를 기본으로 해 `main`의 이력을 선형으로 유지한다.
- 병합 후 작업 브랜치는 삭제한다.

## CI/CT — `.github/workflows/ci.yml`

모든 PR과 `main` 병합에서 자동 실행된다.

- Python 3.14 환경에서 단위 테스트(`unittest`) 실행과 **Branch 커버리지 100%** 검증
  (`CLAUDE.md`의 "단위 테스트 지침", `tdd` 스킬 5·6절과 동일 기준).
- 순환복잡도 10 이하(`xenon`), 중복 코드 8라인 이상 금지(`pylint duplicate-code`), 기본 린트
  (`flake8`).
- PR 제목이 Conventional Commits 형식을 따르는지 검사.
- 아직 `.py` 소스가 없으면(현재처럼 초기 단계) 해당 단계를 건너뛰고 경고만 남긴다 — `coding`/`tdd`
  서브에이전트가 실제 구현을 시작하면 게이트가 자동으로 활성화된다.

`CLAUDE.md`의 구현/테스트 지침이 개정되면 이 워크플로와 본 문서도 함께 갱신한다.

## 필요한 GitHub 저장소 설정 (저장소 관리자가 직접 적용)

브랜치 보호 규칙은 GitHub 저장소 설정(서버 쪽 상태)이라 이 저장소의 파일만으로는 켤 수 없다. 저장소
소유자가 **Settings → Branches → Branch protection rules → `main`**에서 아래를 켜야 한다.

- [ ] **Require a pull request before merging** — `main` 직접 push 차단
- [ ] **Require approvals** — 최소 1(협업 시). 솔로 운영이면 0으로 두되 PR 체크리스트로 대체
- [ ] **Require status checks to pass before merging** — `ci.yml`을 최소 한 번 PR에서 실행한 뒤,
      검색 목록에 나타나는 `Lint, quality gates, and tests`(필요하면 `Conventional PR title`도)를
      필수 체크로 지정. 처음 실행되기 전에는 목록에 나타나지 않으니, 이 워크플로가 포함된 PR을 한 번
      연 뒤 설정한다.
- [ ] **Require branches to be up to date before merging**
- [ ] **Do not allow bypassing the above settings** (관리자 포함 여부는 팀 정책에 따라 결정)
- [ ] **Restrict force pushes** / **Restrict deletions** — `main` 이력 보호

`gh` CLI가 설치·인증되어 있다면(저장소 관리자 권한 필요) 아래 명령으로 동일하게 적용할 수 있다
(`contexts`의 정확한 문자열은 위와 같이 실제 실행 이력에서 확인 후 맞춘다):

```bash
gh api -X PUT repos/heasun0111/AI-vibe-coding/branches/main/protection \
  -F required_status_checks='{"strict":true,"contexts":["Lint, quality gates, and tests"]}' \
  -F enforce_admins=true \
  -F required_pull_request_reviews='{"required_approving_review_count":1}' \
  -F restrictions=null
```
