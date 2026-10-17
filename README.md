# my-subagents

pi-subagents 서브에이전트 정의 파일(`*.md`) 원본 저장소. 여기서 편집하고, `agents/` 폴더째 글로벌 사용자 에이전트 폴더에 symlink로 연결해 사용한다.

## 에이전트 목록

| 이름 | 역할 |
|------|------|
| `worker` | 일반 구현 작업 + oracle 승인 핸드오프 |
| `oracle` | 설계·계획·트레이드오프 판단 |
| `researcher` | 코드베이스 조사, 문맥 수집 |
| `reviewer` | 코드 리뷰 |
| `scout` | 빠른 코드베이스 정찰 |
| `evidence-auditor` | 검증 감사 |
| `delegate` | 작업 위임·조율 |

## 설치

```bash
ln -s "$(pwd)/agents" ~/.pi/agent/agents/my-subagents
```

디스커버리가 `~/.pi/agent/agents/**/*.md` 를 재귀로 읽으므로, 폴더 symlink 1개면 모든 에이전트가 로드된다.

## 신규 에이전트 추가

`agents/` 에 `<name>.md` 생성. frontmatter `name`, `description` 필수.

```yaml
---
name: my-agent
description: 무슨 일을 하는 에이전트
model: some-model-id
---
```

symlink는 이미 연결돼 있으므로 파일 추가·수정 즉시 반영된다.

## 주의

- 런타임 이름 = frontmatter `name`. **파일명이 아님.**
- `name`/`description` 빠지면 에러 없이 조용히 무시된다.
- `worker`~`evidence-auditor` 는 내장 에이전트를 복사 + `model:` 핀 한 것. pi-subagents 업그레이드 시 수동 재동기화 필요.
- 자세한 규칙은 [AGENTS.md](./AGENTS.md) 참고.
