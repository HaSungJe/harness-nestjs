# 도메인 생성 명령

`<domain> 도메인 생성` 명령을 받으면 이 문서를 따른다.

## 절차

1. 프로젝트 `CLAUDE.md` 의 아키텍처 섹션을 읽는다. 구체 경로·디렉터리 구조·모듈 import 규약은 모두 거기서 확인.
   - `CLAUDE.md` "Architecture" 섹션
   - 링크된 `docs/architecture.md` 의 "New Domain Minimum File Structure" / "Adding a New Domain" / "Key Patterns"
2. 위 규약대로 해당 도메인의 디렉터리·파일 스캐폴드를 생성한다.
   - 최소 파일 구성(module / symbols / entities 폴더 등)은 프로젝트 규약을 그대로 따름
   - 모듈 클래스에 붙이는 metadata 데코레이터, 기본 import 모듈(TypeORM·Auth 등)도 `docs/architecture.md` 규약대로
3. `app.module.ts` 에 새 도메인 모듈을 등록한다.
   - 등록 방법·Symbol 토큰 바인딩·의존 모듈 주입은 CLAUDE.md / `docs/architecture.md` 규약대로

## 하네스의 책임 범위

이 문서는 **"도메인 생성 명령이 들어오면 CLAUDE.md 의 아키텍처 규약대로 스캐폴드를 만든다"** 까지만 정의한다. 실제 코드 내용(어떤 모듈을 import 할지, `@SetMetadata` 에 무엇을 넣을지, Entity 파일을 어떻게 준비할지 등)은 프로젝트 규약이 소유.

## 검증

스캐폴드 생성 후 아래를 확인:
- [ ] 디렉터리 경로가 CLAUDE.md / `docs/architecture.md` 의 디렉터리 구조와 일치
- [ ] 최소 파일 구성이 `docs/architecture.md` "New Domain Minimum File Structure" 와 일치
- [ ] `app.module.ts` 에 새 도메인 모듈 import 추가됨
