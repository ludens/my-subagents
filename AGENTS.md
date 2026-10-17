# AGENTS.md

이 레포는 pi-subagents 서브에이전트 정의 파일(`*.md`)의 원본 저장소. `agents/` 폴더에 두고, 그 폴더째 글로벌 사용자 에이전트 폴더에 `ln -s`로 연결해 쓴다.

## Symlink 대상 (글로벌 폴더)

- 사용자 범위 로드 경로: `~/.pi/agent/agents/` — `**/*.md` 재귀 디스커버리.
- 폴더 symlink 1개로 연결: `~/.pi/agent/agents/my-subagents -> <repo>/agents`
- `~/.agents/` 도 함께 스캔됨. 둘 다 존재하면 둘 다 로드됨.
- `PI_CODING_AGENT_DIR` 환경변수 설정 시 `$PI_CODING_AGENT_DIR/agents/` 로 대체.

## 신규 에이전트 추가 절차

1. 이 레포 `agents/` 에 `<name>.md` 생성 — frontmatter `name`, `description` 필수.
2. 폴더 symlink 이미 연결돼 있으므로 추가 작업 없음. 파일 추가·수정 즉시 반영.

## Landmines

- `name`/`description` frontmatter 빠지면 에이전트가 **에러 없이 조용히 무시**된다. 파일만 만들고 빠뜨리지 말 것.
- 런타임 이름 = frontmatter `name`. **파일명이 아님.** 파일명 바꿔도 에이전트 이름 안 바뀐다.
- `*.chain.md` 는 에이전트로 로드되지 않음.
- 디스커버리는 symlink(파일·디렉토리 모두)를 따라감. 깨진 symlink는 조용히 무시.
- symlink 타겟은 **절대경로**. 상대경로면 링크 위치(`~/.pi/agent/agents/`) 기준으로 풀려서 엉뚱한 곳 가리킴.
- 이름 충돌 우선순위: 프로젝트 > 사용자 > 패키지 > 내장. `~/.agents/` 에 같은 `name` 에이전트 있으면 중복 로드 주의.
- `.pi`, `node_modules`, `.git`, `sync-backups` 이름의 하위 디렉토리는 디스커버리에서 제외됨. 에이전트 파일은 이런 폴더 안에 두지 말 것. (폴더 symlink 자체는 `my-subagents` 라 문제 없음.)

## Override 파일

- `worker.md`~`evidence-auditor.md` 는 내장 에이전트를 **verbatim 복사 + `model:` 핀** 한 것. pi-subagents 업그레이드로 내장 프롬프트가 바뀌어도 이 파일들은 자동으로 안 따라감. 수동 재동기화 필요. (파일은 `agents/` 하위에 있음.)
- `model:` 값은 frontmatter `name` 기준으로만 매칭됨. 파일명이 아니라 name.

## 참고

- 전체 frontmatter 레퍼런스·모델·도구 설정: https://pi.dev/packages/pi-subagents (docs/agents.md, docs/models.md)
- `git init` 안 된 상태. 필요 시 먼저 init.
