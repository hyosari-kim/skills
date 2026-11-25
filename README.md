# skills

개인 Claude Code 에이전트와 스킬 모음.

| 경로 | 설명 |
|---|---|
| `agents/doc-reviewer.md` | spec·plan 등 구현용 문서가 빈틈 없이 작성됐는지 검토하는 에이전트 |
| `skills/doc-review` | `/doc-review`로 doc-reviewer 에이전트를 부르는 스킬 |
| `skills/ooui-designer` | 객체 중심(OOUI)으로 화면 구조를 설계·검토하는 스킬 |
| `plugins/hstack` | 작업 방식 플러그인. 아래 표 참고 |

## hstack 플러그인

`/hstack:hyosari-mode`로 부르는 작업 방식 라우터와 그 하위 스킬, 에이전트다. cursor/plugins의 pstack(poteto-mode, MIT, `plugins/hstack/LICENSE-pstack`)을 Claude Code용으로 옮겼다. 모든 스킬은 `/hstack:<이름>`으로 부른다.

| 경로 (`plugins/hstack/` 아래) | 설명 |
|---|---|
| `skills/hyosari-mode` | 작업 방식 라우터. 작업을 플레이북 18개(조사, 진단, 버그 수정, 성능, 기능, 리팩터링, 프로토타입, 장시간 실행, PR 등)에 맞추고 아래 하위 스킬을 경로로 읽어 쓴다 |
| `agents/hyosari-agent.md` | 플레이북 단계에서 띄우는 서브에이전트(`hstack:hyosari-agent`). 작업 전에 hyosari-mode SKILL.md를 읽는다 |
| `agents/hyosari-comment-sicko.md` | hyosari-no-comments가 띄우는 주석 제거 에이전트(`hstack:hyosari-comment-sicko`) |
| `skills/pr-reply` | `/hstack:pr-reply <PR 번호>`로 미해결 리뷰 코멘트에 답글을 다는 스킬. hyosari-mode가 경로로 읽는다 |
| `skills/hyosari-architect` | 함수·모듈 경계를 넘는 코드 전에 호출하는 쪽, 타입, 시그니처, 모듈 경계를 정하는 스킬 |
| `skills/hyosari-tdd` | 싼 로컬 테스트 경로가 있을 때 실패하는 테스트를 먼저 쓰고 고치는 스킬 |
| `skills/hyosari-how` | 지금 코드가 어떻게 동작하는지, 어디에 있어야 하는지를 파일·줄 근거로 설명하는 스킬 |
| `skills/hyosari-why` | 왜 그렇게 만들었는지, 언제부터 깨졌는지를 커밋·PR·이슈 기록으로 답하는 스킬 |
| `skills/hyosari-interrogate` | 구현 설명 없이 리뷰어가 변경을 깨보고, 실행으로 증명된 지적만 남기는 스킬 |
| `skills/hyosari-explain-the-number` | 측정한 숫자를 보고하거나 그 숫자로 결정하기 전에 무엇을 쟀는지 점검하는 스킬 |
| `skills/hyosari-swarm` | 병렬 워커 N개를 펼치고 거둬서 보고서 하나로 내는 스킬 |
| `skills/hyosari-arena` | 같은 작업에 후보 N개를 돌리고 바탕 하나에 좋은 부분을 이식하는 스킬 |
| `skills/hyosari-figure-it-out` | 맞는 플레이북이 없는 큰 작업에 감사 가능한 플레이북을 설계하는 스킬 |
| `skills/hyosari-in-your-own-words` | 사용자의 목표와 문제를 내 말로 다시 말하는 스킬 |
| `skills/hyosari-control-ui` | 웹·IDE·Electron UI를 Playwright·브라우저로 움직이며 확인하는 스킬 |
| `skills/hyosari-control-cli` | 대화형 CLI·TUI를 tmux 등으로 움직이며 확인하는 스킬 |
| `skills/hyosari-no-comments` | 리뷰 전에 comment-sicko로 주석을 걷어내는 스킬 |
| `skills/hyosari-unslop` | 글에서 AI 티를 걷어내는 스킬 |
| `skills/hyosari-technical-writing` | 문서·PR 설명·커밋 메시지 작성 기준 |
| `skills/hyosari-show-me-your-work` | 장시간·무인 작업의 결정 기록(TSV)을 남기는 스킬 |
| `skills/hyosari-principle-*` | 원칙 25개. pstack 원칙 24개(smallest-change, model-first는 기존 이름)와 고유 1개(follow-the-codebase) |

hstack 스킬은 모두 슬래시로만 불리고, `hyosari-mode`가 하위 스킬을 형제 폴더(`../<이름>`) 경로로 읽는다. 그래서 스킬을 따로 떼어 설치하지 않는다.

## 설치

저장소를 받는다.

```bash
git clone https://github.com/hyosari-kim/skills.git ~/code/skills
```

hstack은 저장소를 로컬 마켓플레이스로 등록해 설치한다. Claude Code 안에서 실행한다.

```
/plugin marketplace add ~/code/skills
/plugin install hstack@hyosari-skills
```

나머지 스킬과 에이전트는 `~/.claude`에 심볼릭 링크로 연결한다.

```bash
mkdir -p ~/.claude/agents ~/.claude/skills
ln -s ~/code/skills/agents/doc-reviewer.md ~/.claude/agents/doc-reviewer.md
ln -s ~/code/skills/skills/doc-review ~/.claude/skills/doc-review
ln -s ~/code/skills/skills/ooui-designer ~/.claude/skills/ooui-designer
```

저장소를 고친 뒤에는 `/plugin marketplace update hyosari-skills`로 플러그인을 다시 받는다.
