### Worktree and simulator cleanup

1. 스냅샷을 남기고 감사한다. `df -h /`를 기록하고, `scripts/worktree-audit.sh [-b <pr-base-ref>] [repo-path]`를 돌린다(**principle-build-the-lever**). `-b`가 없으면 `MERGED`를 `origin/HEAD`에 대고 확인하므로, PR이 다른 브랜치를 향하면 `-b origin/<branch>`를 넘긴다. 경로는 손으로 치지 않고 `git worktree list`에서 읽는다. 손으로 친 `myrepo-worktrees/x`는 `.claude/worktrees/x`에 있는 것을 놓치기 때문이다(**principle-encode-lessons-in-structure**). 각 worktree를 크기, 나이, 머지 상태, 커밋하지 않은 작업, PR 상태, 그것을 건드린 가장 최근 대화(`~/.claude/projects/<slug>/*.jsonl`)로 분류하고 버킷을 제안한다. 대화 기록 스캔은 느리니 백그라운드로 돌린다.
2. 버킷은 조언이지 허락이 아니다. 고정되거나 진행 중인 대화가 진짜 산출물이다(**principle-prove-it-works**). 그 집합을 사용자나 사이드바에서 받아 모든 후보와 대조한다. 이 도구가 사용자가 고정해 둔 worktree를 `safe`로 표시한 적이 있으니, 고정된 집합이 이긴다.
3. 지우기 전에 쓰임을 확인한다. 모든 `verify-recent-chat` 행과 의심스러운 것마다 서브에이전트를 나눠 대화 기록을 읽게 하고, 그 대화가 고정됐거나 진행 중인지, 어떤 worktree를 건드리는지 보고하게 한다(**principle-guard-the-context-window**, 대화 기록은 덩치가 크다). 고정된 대화는 백그라운드 서브에이전트로 arena와 재현용 트리를 형제 worktree에 띄우는데, 그 이름이 사이드바에 안 보여도 쓰는 중이다. 각 서브에이전트의 effort는 SKILL.md의 Subagents 섹션에 따라 일에 맞게 고른다.
4. 되돌릴 수 없는 손실 앞에서 멈춘다. `wip:N`은 추적되는 파일의 커밋하지 않은 수정 N개다. diff를 보여주고 먼저 결정을 받는다. 깨끗한 worktree를 지우는 것은 브랜치에서 되살릴 수 있지만 커밋하지 않은 작업은 사라진다. `scratch:N`은 추적되지 않는 버릴 파일이라 지워도 되지만, 파일 이름은 밝힌다. SKILL.md의 Autonomy에 따라, 깨끗하고 머지됐고 쓰지 않는 것은 진행한다. `wip`와 쓰는 중인 것은 멈춘다.
5. 확인된 집합을 정리한다. 경로마다 `git worktree remove --force <path>`. 무시된 빌드 산출물 때문에 디렉터리가 남으면 `rm -rf`로 지운 뒤 `git worktree prune`. 브랜치 ref는 남으므로 커밋은 잃지 않는다. `df -h /`와 다시 나열해서 확인한다.
6. 시뮬레이터와 다른 공간 회수. 시뮬레이터가 보통 그다음으로 크다. `xcrun simctl --set testing delete all`(XCTestDevices 복제본), `xcrun simctl delete unavailable`, 그리고 `xcrun simctl runtime list` 후 오래된 런타임은 `runtime delete <id>`. 더 필요하면 Xcode `DerivedData`와 `iOS DeviceSupport`, 패키지 캐시(pnpm, uv, brew, yarn). 사용자가 남기라고 하지 않은 캐시만 지운다.

코드 리뷰로 실수를 잡을 수 없이 사용자 상태를 지우는 유일한 플레이북이라, 위의 관문들이 리뷰다.

**Reply:** 전후 `df -h /`와 회수한 공간, 정리한 worktree, 남겨둔 것마다 한 줄 이유(어떤 대화가 쓰는 중인지, 또는 커밋하지 않은 작업).
