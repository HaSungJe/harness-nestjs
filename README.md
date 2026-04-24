# harness-nestjs

> Drop-in harness for NestJS — structures AI-assisted feature development via request/work specs.

`npx harness-nestjs init` 한 줄로 기존 NestJS 프로젝트에:

- `.harness/` 워크플로 구조 (request → work → 구현 → 테스트 → 리포트)
- `.claude/settings.json` 훅 등록 (Claude Code 사용 시)
- `.husky/pre-commit` 회귀 테스트 블록
- `.gitignore` 필수 엔트리

를 한 번에 주입합니다.

---

## 설치

```bash
npx harness-nestjs init
```

끝. 별도의 `npm install` 단계 불필요 — init 이 자기 자신을 devDependency 로 추가 후 필요한 파일들을 프로젝트에 배치합니다.

## 요구사항

- Node.js >= 18
- 기존 NestJS 프로젝트 (`package.json` 이 있는 폴더에서 실행)
- (선택) Claude Code — `.claude/settings.json` 훅을 활용하려면

## 무엇이 설치되는가

| 항목 | 경로 | 역할 |
|---|---|---|
| 하네스 워크플로 | `.harness/` | request/work 스펙 · validator · hook 스크립트 · 템플릿 |
| Claude Code 훅 | `.claude/settings.json` | work/request 파일 저장 시 validator 자동 실행 |
| Husky pre-commit | `.husky/pre-commit` | `src/` 또는 `*.spec.ts` 변경 시 `npm test` (회귀 보장) |
| .gitignore | `.gitignore` | 하네스 세션 로컬 상태 파일 (`.retry-count` 등) 제외 |

기존 파일이 있으면 **덮어쓰지 않고 merge/append** (중복 체크). `--force` 로 강제 가능.

## 옵션

```bash
npx harness-nestjs init             # 기본 동작
npx harness-nestjs init --dry-run   # 실제 변경 없이 예정 작업만 출력
npx harness-nestjs init --force     # 기존 파일 덮어쓰기 (주의)
```

## 워크플로 개요

설치 후 프로젝트 루트의 `.harness/README.md` 를 참조. 핵심 명령:

| 명령 | 역할 |
|---|---|
| `user 도메인 생성` | 새 도메인 폴더 · module 파일 생성 |
| `xxx 기능 생성` | request 스펙 초안 → 사용자 보완 → work 스펙 작성 |
| `xxx 작업 시작` | work 스펙대로 구현 + spec · 테스트 생성 · 자가수복 · 리포트 |
| `작업내용 커밋해줘` | staged 변경에서 feature_goal 추출해 커밋 |
| `작업내용 푸쉬해줘` | 대상 브랜치 질의 후 push |

## 제거

```bash
# 수동 제거
rm -rf .harness/
# .claude/settings.json 의 harness 항목 제거 (사용자 기존 설정은 보존)
# .husky/pre-commit 의 # === harness-block-start === ~ === harness-block-end === 블록 제거
# .gitignore 의 .harness/.retry-count, .harness/.current-spec 라인 제거
```

자동 제거 명령은 추후 지원 예정.

## License

MIT — 자세한 내용은 [LICENSE](./LICENSE).

## Contributing

이슈·PR 환영: [github.com/HaSungJe/harness-nestjs](https://github.com/HaSungJe/harness-nestjs)
