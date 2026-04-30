# Changelog

## [0.3.0] - 2026-04-30

### Added
- 워크플로 슬래시 커맨드 7종 — 기존 한국어 키워드와 **동등한 진입점**으로 동작 (가산, 키워드 방식도 그대로 유지). 슬래시 이름은 `.harness/docs/<folder>/` 와 **1:1 매칭**
  - `/domain-create <domain>` — `<domain> 도메인 생성`
  - `/feature-plan <featureName>` — `<featureName> 기능 생성`
  - `/feature-implement <featureName>` — `<featureName> 작업 시작`
  - `/feature-modify-plan <featureName>` — `<featureName> 기능 수정`
  - `/feature-modify-implement <featureName>` — `<featureName> 수정 시작`
  - `/git-commit` — `작업내용 커밋해줘`
  - `/git-push` — `작업내용 푸쉬해줘`
- 각 슬래시 파일은 `.harness/docs/<command>/index.md` 로 위임하는 얇은 진입점 (콘텐츠 중복 0)

### Changed
- **폴더 rename — 슬래시 이름과 매칭**
  - `.harness/docs/commit/` → `.harness/docs/git-commit/`
  - `.harness/docs/push/` → `.harness/docs/git-push/`
- **`.harness/docs/feature-modify/` 분할 — 단계별 폴더로 분리**
  - `feature-modify-plan/` (변경요청 단계 ① ~ ④, `change-file.md` 동행)
  - `feature-modify-implement/` (구현·테스트·report 추기 단계 ⑤ ~ ⑧)
- `.harness/docs/feature-plan/index.md` — 내부 `../push/` 참조를 `../git-push/` 로 갱신
- `.harness/docs/routing.md` — 한국어 키워드 / 슬래시 / 상세 문서 3컬럼 표로 재구성. **4개 섹션** 으로 분할 (도메인 생성 / 기능 생성 / 기능 수정 / 배포)
- `.harness/samples/starter/CLAUDE.sample.md` "명령어 라우팅" 블록 — 슬래시 진입점도 함께 안내
- [README.md](README.md) — 워크플로 표·전제 조건 표에 슬래시 병기, 설치 항목 표의 커맨드 행 갱신
- [README.md](README.md) — `samples/starter/` 설명에서 "BullMQ 기준" 등 특정 스택 언급 제거. 샘플은 *형식·톤 참고용 예시* 라는 점을 명확히. `/harness-init` 설명도 동일 정리

### Fixed
- 일관성 감사 후속 정리:
  - `bin/cli.js` `--version` 이 하드코딩 `0.1.0` 이던 것을 `package.json` 에서 동적 읽기로 변경 (앞으로 버전 bump 시 한 곳만 갱신)
  - `bin/cli.js` `init` 명령 Next steps — `/harness-init` 슬래시를 1순위 권장, manual copy 는 대안으로 격하. 마지막 단계 예시도 슬래시(`/feature-plan`) 와 한국어 키워드 병기
  - `harness-config.json` 의 `step_5_fail_retry_limit` 키를 `self_heal_retry_limit` 으로 rename. 옛 키는 워크플로 단계 번호와 의미가 어긋났음 (실제 self-heal 은 ⑦단계). 동작상은 dead config (실제 retry 값은 `hooks/run-tests.sh` 의 `MAX_RETRY=10` 하드코딩) 라 영향 없음
  - `samples/starter/CLAUDE.sample.md` 상단에 HTML 주석으로 **샘플의 성격** 명시 — "한 프로젝트의 실제 컨벤션 예시이며 플러그인이 강요하는 스택이 아님. `/harness-init` 으로 본인 스택에 맞춰 재생성 권장. 단 '## 플러그인 설정' 블록은 그대로 복사 필수"
  - [README.md](README.md) "5단계" 표현이 ①~⑨ 9행 표와 충돌하던 것을 "5국면 (① ~ ⑨ 세부 단계)" 로 정리
  - [README.md](README.md) 정보 동선 재구성 — **`## 명령 일람`** 섹션을 인트로 직후에 신설 (한국어 키워드 / 슬래시 / 설명 한눈 표). 워크플로 표·전제 조건 표·설치 표에 흩어져 있던 슬래시 매핑 정보를 명령 일람으로 흡수, 각 표는 키워드만 남겨 가볍게. uninstall 표의 `harness-init.md 제거` 행도 **플러그인 소유 슬래시 8종 제거** 로 정확화
  - `skeleton/.harness/README.md` — 디렉터리 트리·라우팅 흐름이 옛 구조 (`commit/`·`push/`·`feature-modify/` + 키워드 단일 진입점) 를 보여주던 것을 새 구조 (7폴더 1:1 매칭 + 키워드/슬래시 양쪽 진입점) 로 재작성. 워크플로 샘플 파일명도 실제(`workflow-*.md`, `workflow-step*.jpg`) 로 정정
  - `package-lock.json` 의 자기 버전 두 곳 0.1.0 → 0.3.0
- **메인테이너 가이드 정합성 (CONTRIBUTING.md)**:
  - "새 명령 추가" 절차 — 옛 2-column routing.md 형식 + 한국어 키워드 단일 등록만 안내하던 것을 **5단계 절차** 로 재작성. 슬래시 파일 생성, 3-column routing.md, CLAUDE.sample.md 의 키워드/슬래시 양쪽 bullet, README 워크플로 표 갱신을 모두 포함
  - **네이밍 규칙** 명문화 — 슬래시 이름 ↔ docs 폴더명 1:1 매칭, `-plan`/`-implement` 접미어, `git-` 접두어
  - "명령 제거 / 이름 변경" 체크리스트 — 4항목 → 7항목으로 확장 (슬래시 파일, CLAUDE.sample.md 슬래시 bullet, README, CHANGELOG 추가)
  - "skeleton merge 규칙 > 새 슬래시 커맨드 추가 시" — 워크플로 명령은 위 5단계 절차로 위임, 워크플로 외 보조 슬래시(예: `/harness-init`) 만 슬래시 파일 + README 갱신으로 충분함을 명확히 구분
- **BullMQ 잔재 정리** (사용자 차원 점검 후속):
  - `.claude/commands/harness-init.md` ④ 섹션의 "BullMQ 를 안 쓰면 큐 섹션 생략" → "큐 솔루션 미사용이면 큐 섹션 생략, 인증 방식이 다르면 Auth 섹션을 실제 방식으로 치환" 으로 일반화
  - `templates/work.md` 사전 구현 항목 HTML comment 예시의 "BullMQ Queue 인프라" → "큐 인프라 (CLAUDE.md 큐 관련 섹션 기준)" 으로 일반화. README 본문의 같은 예시와 톤 일치

### Notes
- CLI 자체는 매니페스트 무수정 — `bin/cli.js` 의 `walk(skeleton/.claude/commands)` 가 새 슬래시 7개를 자동 발견. 폴더 rename 도 `walk(skeleton/.harness)` 이 자동 처리
- 기존 0.2.x 사용자 `update` 시: 새 슬래시 자동 추가, `commit/`·`push/` 폴더는 `.harness/` 전량 삭제 후 재복사 정책에 따라 새 이름으로 재배치, `feature-modify/` 폴더는 분할 결과물로 교체

## [0.2.2] - 2026-04-24

### Changed
- `커밋` / `푸쉬` 명령에 **매번 사용자 확인** 단계 필수화. 이전 세션에서 승인받은 이력이 있어도 매 커밋·푸쉬마다 재확인.
- commit: 커밋 메시지 + staged 파일 목록을 사용자에게 보여주고 승인 후에만 `git commit` 실행
- push: 푸쉬될 커밋 목록 + 대상 브랜치 + 작업 디렉터리(worktree/본체) 를 보여주고 승인 후에만 `git push` 실행. `--follow-tags` / force-push 필요 시 별도 승인
- 워크플로 샘플 파일명 정리 (`workflow-{request,work,report}.md.md` → `workflow-{request,work,report}.md`) + 템플릿 3종 참조 경로 갱신

## [0.2.1] - 2026-04-24

### Fixed
- `작업 시작` 시 통합 브랜치(main/master/develop) 에 있으면 `feature/<featureName>` 브랜치 생성 여부를 사용자에게 확인. 이전엔 브랜치 규칙이 없어 main 에 바로 작업이 쌓이는 문제. 이미 non-integration 브랜치면 그대로 진행.

## [0.2.0] - 2026-04-24

### Added
- `/harness-init` 커스텀 슬래시 커맨드 — 하네스 샘플을 템플릿 삼아 루트 `CLAUDE.md` + `docs/` 자동 생성
- `skeleton/.claude/commands/` 배포 경로 신설
- CLI: `copyClaudeCommands()` / `wipePluginOwnedCommands()` — 플러그인 소유 커맨드 설치·갱신·제거 (파일명 매칭으로 사용자 추가 커맨드 보존)
- `init` / `update` / `uninstall` 각 단계 카운터에 `.claude/commands/` 처리 추가

### Changed
- [README.md](README.md) 설치 항목 표에 커스텀 커맨드 행 추가 + "첫 사용" 섹션 신설
- [CONTRIBUTING.md](CONTRIBUTING.md) 에 `skeleton/.claude/commands/` merge 규칙 + 새 커맨드 추가 절차 문서화

## [0.1.0] - 2026-04-24

### Added
- `init` / `update` / `uninstall` CLI 명령 (설치·갱신·제거 전체 라이프사이클)
- `.harness/` 워크플로 스캐폴드 — docs (명령별 실행 규칙) · templates · validators · hooks
- PostToolUse 훅 3개 전부 wiring — `on-request-written.sh` · `on-work-written.sh` · `run-tests.sh`
- husky pre-commit 회귀 테스트 블록
- 작업시작·커밋·푸쉬 커맨드에 **전제 조건** 명시 (파일 존재 기반 state)
- `.harness/output/work/<domain>/<featureName>-work.md` 존재 + request 결정사항 전부 답변 + validator 통과 — 3중 검증
- work.md 에 **사전 구현 필요 항목** 선택 섹션 — CLAUDE.md 규칙 적용에 필요한 인프라가 src/ 에 없을 때 별도 작업 유도
- `domain-create` 에 Architecture fallback — CLAUDE.md 가 없거나 부실할 때 최소 `src/api/<domain>/` 스캐폴드 생성
- 자가 수복 루프 — 테스트 실패 시 최대 10회 재시도
- `.harness/samples/` — 참고용 샘플
  - `samples/starter/` — 프로젝트 규칙 스타터 (`CLAUDE.sample.md` + `docs/`, NestJS 11 + TypeORM + BullMQ 기준)
  - `samples/workflow/` — 실제 워크플로 산출물 + 단계별 스크린샷
- `harness-config.json` — 프로젝트별 조정 포인트 (test 경로 · 필수 섹션 · 참조 docs)
- `update` 시 `harness-config.json` 딥 머지로 사용자 값 보존 + 새 필드만 추가
- `uninstall` `--purge` 옵션 — 기본은 사용자 산출물(`output/` · `memory/`) 보존
- 샘플 `CLAUDE.sample.md` 에 "## 플러그인 설정" (명령어 라우팅 + 메모리 시스템) 섹션 포함
- 문서 분리 — 사용자용 [README.md](README.md) / 메인테이너용 [CONTRIBUTING.md](CONTRIBUTING.md)
