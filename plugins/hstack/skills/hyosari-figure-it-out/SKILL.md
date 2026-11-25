---
name: hyosari-figure-it-out
description: 맞는 플레이북이 없을 때 감사할 수 있는 플레이북을 직접 설계한다. 큰 마이그레이션, 여러 부분에 걸친 야심찬 변경, 사람이 자리를 비웠다가 돌아와 리뷰할 작업에 쓴다. 작업에 맞게 엄밀함을 조절하고, 가설 루프를 돌리고, 결정을 기록한다. "figure it out" 같은 요청에도 쓴다. /hstack:hyosari-figure-it-out 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Hyosari figure it out

작업이 어떤 플레이북에도 맞지 않으면 플레이북을 설계한다. 코드보다 먼저 내야 할 산출물은 워크플로 자체다. 작업에 맞게 엄밀함을 조절하고, 과학적 방법을 따르고, 사람이 자리를 비웠다가 돌아와 감사할 수 있는 결정 기록을 남기는 단계의 순서다.

## Start

할 일 목록을 연다. 첫 항목은 **hyosari-mode** 스킬의 Principles 섹션을 읽는 것이다. 그다음 아래 단계를 항목으로 넣는다.

## Phase A: Frame

먼저 근거를 모으고, 그다음 정한다. 아래를 말할 수 있기 전에는 실행을 시작하지 않는다.

- 반증할 수 있는 조건으로 쓴 완료의 정의(**hyosari-principle-prove-it-works** 원칙 스킬).
- 수치로 적은 범위. 대략의 단위 수와 공수, 근거를 모으며 드러난 막힘.
- 엄밀함 수준. 높은 쪽으로 기운다. 되돌릴 수 없는 결정과 영향 범위가 큰 일은 더 높인다. 되돌릴 수 있고 위험이 낮은 단계는 낮춘다. 엄밀함은 "더 열심히"가 아니라 관문(gate)과 산출물이다.

긴 실행을 시작하기 전에 프레이밍과 트레이드오프를 보여 준다. 되돌릴 수 있는 일은 그대로 진행한다(**hyosari-principle-never-block-on-the-human** 원칙 스킬). 다만 몇 시간짜리 실행이면 체크포인트를 한 번 둔다.

## Phase B: Design the workflow

따로 착지시킬 수 있는 원자 단위로 나눈다. 가장 위험한 미지수부터 순서를 정한다. 기능보다 뼈대와 검증이 먼저다(**hyosari-principle-foundational-thinking** 원칙 스킬).

- 작업보다 먼저 검증 장치를 만든다. 기준값은 바꾸기 전 상태에서 잡아서, 확인이 "이전 값 대 새 값"으로 읽히게 한다.
- 되돌릴 수 없는 설계 결정에는 **hyosari-architect** 스킬을 돌린다(그 안에서 **hyosari-arena**를 돌린다). 모양이 이미 구체적인 기계적 작업에는 건너뛴다. 정해진 설계에 arena를 또 돌리는 것은 과잉 설계다(**hyosari-principle-smallest-change** 원칙 스킬).
- 무엇을 병렬로 펼칠지 정한다. 이음매를 따라서만 병렬로 나누고, 워커마다 자기 worktree나 브랜치를 준다(**hyosari-principle-separate-before-serializing-shared-state** 원칙 스킬). 지나치게 펼치지 않는다. 서브에이전트는 `Agent` 도구로 `subagent_type: "hstack:hyosari-agent"`를 써서 띄우고, 추론 수준(`effort`)은 일에 맞게 **hyosari-mode** 스킬의 Subagents 섹션을 따라 고른다.
- 설계한 단계 목록을 적어 둔다. 사람이 리뷰하는 것이 그 목록이다.

그다음 설계를 실행한다. 설계의 단계를 할 일 목록의 Phase C와 Phase D 항목 사이에 구체적인 항목으로 넣는다. 각 단계는 Phase C의 루프 규율로 돌리고, Phase D의 기록은 끝에 몰아서 쓰지 말고 단계가 착지할 때마다 한 행씩 엮어 넣는다.

## Phase C: Run the loop

단위 하나하나가 실험이다. 가설을 적고, 가장 작은 변경을 하고, 실제 산출물에서 완료 조건에 대해 측정하고, 나아졌으면 남기고 아니면 되돌린다. **hyosari-principle-sequence-verifiable-units** 원칙 스킬을 따라, 확인을 끝에 몰지 말고 각 단위를 검증한 뒤에 다음 단위를 시작한다.

- 자기 보고가 아니라 산출물을 직접 보고 검증한다. 너무 쉽게 통과하면 시스템보다 관찰 방법을 먼저 의심한다.
- 위임한 일에는 판정자를 붙인다. 워커가 관문을 속이면 되돌리고 계약을 단단히 한다. 관문 자체가 틀렸으면 우회하지 말고 별도 변경으로 관문을 고친다.
- 판정은 VERIFIED, NOT VERIFIED, INCONCLUSIVE 중 하나다. INCONCLUSIVE는 통과가 아니다. 부정적인 결과를 숨기지 않는다.

## Phase D: Keep the audit trail

**hyosari-show-me-your-work** 스킬로 실행을 기록한다. figure-it-out의 작업은 대개 충분히 야심차서, 리뷰하는 사람이 PR에서 읽을 수 있게 기록을 커밋한다. 기록과 diff가 함께 있어야 사람이 돌아와서 작업을 믿을 수 있다.

## Phase E: Verify and hand back

검증 장치만이 아니라 실제 제품에서 전체를 Phase A의 완료 조건에 대해 확인한다. 반복되는 교정은 관문, lint 규칙, 확인, 스크립트로 굳힌다(**hyosari-principle-encode-lessons-in-structure** 원칙 스킬).

**Reply:** 설계한 플레이북, 엄밀함 수준과 이유, 결정 기록 경로, 완료 조건에 대해 검증된 것, 아직 열린 것.
