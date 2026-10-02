# skills

개인 Claude Code 에이전트와 스킬 모음.

| 경로 | 설명 |
|---|---|
| `agents/doc-reviewer.md` | spec·plan 등 구현용 문서가 빈틈 없이 작성됐는지 검토하는 에이전트 |
| `skills/doc-review` | `/doc-review`로 doc-reviewer 에이전트를 부르는 스킬 |
| `skills/ooui-designer` | 객체 중심(OOUI)으로 화면 구조를 설계·검토하는 스킬 |

## 설치

`~/.claude`에 심볼릭 링크로 연결한다.

```bash
git clone https://github.com/hyosari-kim/skills.git ~/code/skills
mkdir -p ~/.claude/agents ~/.claude/skills
ln -s ~/code/skills/agents/doc-reviewer.md ~/.claude/agents/doc-reviewer.md
ln -s ~/code/skills/skills/doc-review ~/.claude/skills/doc-review
ln -s ~/code/skills/skills/ooui-designer ~/.claude/skills/ooui-designer
```
