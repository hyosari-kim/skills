---
name: hyosari-arena
description: 같은 작업에 후보 N개를 병렬로 띄우고, 기반 하나를 고른 뒤 진 후보들의 가장 좋은 부분을 접붙인다. 한 번의 시도로는 사소하지 않은 산출물의 모양이 잘못 굳을 때, "arena this", "arena에 던져" 같은 요청에 쓴다. /hstack:hyosari-arena 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Hyosari arena

무엇이든 띄우기 전에 단계마다 한 항목씩(Frame, Fan out, Cross-judge, Pick, Graft, Verify) 할 일 목록을 연다.

## Phase A: Frame

모든 후보가 같은 프롬프트를 받는다. 그래서 프롬프트가 계약이다.

1. 각 후보가 만드는 산출물을 적는다.
2. 루브릭을 정한다. *이 작업*에서 성공이 어떤 모습인지를 채점할 수 있는 구체적인 기준 3~6개다. Phase D에서 고르는 쪽이 쓰는 도구다. 후보는 작업만 본다.
3. 러너를 고른다. 기본은 `model: "fable", effort: "high"`, `model: "opus", effort: "high"`, `model: "sonnet", effort: "high"` 세 자리다. 원문의 세 번째 자리는 다른 회사 모델이었다. Claude Code의 `model`은 모두 Claude라서 이 자리는 회사가 다른 시각을 주지 못한다. 후보는 `subagent_type: "hstack:hyosari-agent"`로 띄운다. `Agent` 도구가 `model`이나 `effort` 값을 거부하면 그 자리를 스키마가 받는 가장 가까운 값으로 바꾸거나 그 자리를 뺀다. 러너는 최소 둘을 남기고, 바꾼 것을 말한다. 디자인 방향이 여럿이면 더 띄운다. 판단보다 생성량이 관건인 작업이면 같은 모델을 반복해도 된다. 서브에이전트마다 추론 수준(`effort`)은 **hyosari-mode** 스킬의 Subagents 섹션을 따라 고른다.
4. 후보마다 자기 출력 위치를 준다. 가능하면 `isolation: "worktree"`로 띄워 각자 git worktree를 갖게 하고, 그럴 수 없으면 `/tmp/arena-<slug>/candidate-<n>/`을 준다. **hyosari-principle-separate-before-serializing-shared-state** 원칙 스킬을 따른다.

## Phase B: Fan out

N개 서브에이전트를 `Agent` 호출로 한 메시지에 모두 띄운다. 각자에게 작업, 공유 근거 자료의 경로, 자기 출력 경로를 주고, 산출물과 짧은 근거를 만들라고 지시한다. 근거에는 검토한 대안과 버린 것을 적게 한다.

후보 하나가 아무것도 내지 않으면 N-1개로 진행하고 빠진 것을 적는다.

## Phase C: Cross-judge

Phase B 후보가 모두 끝나면 읽기 전용 판정자 하나를 `subagent_type: "Explore"`로 띄운다. 브리프에 "파일을 고치지 않는다"를 쓰고, 끝난 뒤 `git status`로 확인한다. 판정자 후보는 `model: "opus", effort: "high"`, `model: "sonnet", effort: "high"` 순서다. 원문은 판정자를 부모와 다른 회사 모델로 정했고 Fable도 Claude로 쳤다. Claude Code에서는 다른 회사 모델을 쓸 수 없으므로 부모와 다른 모델인 첫 항목을 쓴다. 이 대체는 회사가 다른 판정이 아니라는 한계를 결과에 적는다. 부모 모델을 모르면 첫 항목을 쓰고, 부모와 다른지 확인하지 못했다고 말한다. `Agent` 도구가 값을 거부하면 다음 항목을 쓰거나 판정자를 건너뛰고, 그렇게 했다고 말한다. Phase D에서 내가 읽는 동안 판정자는 후보를 경로 라벨로 부르며 루브릭 기준마다 점수를 매기고 기반을 추천한다.

## Phase D: Pick a base

고르기 전에 후보를 모두 처음부터 끝까지 읽는다. 전체 인상이 아니라 루브릭 기준 하나씩 각 후보를 채점하고, Phase C에서 판정자를 돌렸으면 그 결과와 비교한다. 기반이 같으면 선택이 확인된다. 다르면 둘 중 하나가 치우쳤거나 루브릭이 모호했다. 결정하기 전에 두 근거를 모두 읽는다.

나중에 유지보수하는 사람이 불변식을 깨지 않고 가장 쉽게 확장할 수 있는 기반을 고른다. 둘이 비슷하면 Smallest Change(**hyosari-principle-smallest-change**)에 따라 경계가 깔끔하거나 API가 작은 쪽을 고른다.

## Phase E: Graft

진 후보를 하나씩 한 번 더 훑으며 기반에 옮길 만한 것을 찾는다. 보통 후보당 한두 가지이고, 대부분을 옮기지는 않는다. 접붙일 것은 **hyosari-principle-redesign-from-first-principles** 원칙 스킬에 따라 손으로 녹여 넣는다. 그래야 결과가 하나의 멘탈 모델 아래에서 일관된다. 기계적으로 붙여 넣지 않는다.

후보들이 한 모양으로 모이면 강한 합의다. 그렇다고 적고 합의된 모양을 낸다. 접붙일 것은 없다. 크게 갈라지면 Phase A가 덜 정해졌던 것이다. 갈라진 것을 평균 내지 말고 다시 프레이밍해서 다시 돌린다.

## Phase F: Verify

합친 산출물은 다른 출력처럼 **hyosari-principle-prove-it-works** 원칙 스킬에 따라 검증한다. 검증에서 arena가 놓친 문제가 나오면 Phase A가 틀렸거나(다시 프레이밍하고 다시 돌린다), 어느 후보는 잡았는데 내가 접붙이기를 놓친 것이다(Phase E로 돌아간다). 덮어 두지 않는다.

## Outputs

합친 산출물 하나와 그 옆에 짧은 종합 노트를 낸다. 노트에는 기반과 고른 이유, 판정자의 결론이나 Phase C에서 판정자를 건너뛰었다는 사실, 접붙인 것마다 출처 후보, 버린 것과 이유, 합의나 빠진 후보, 검증 결과를 적는다.
