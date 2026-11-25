---
name: principle-make-operations-idempotent
description: 크래시, 재시작, 재시도 속에서 도는 명령, 생명주기 단계, 처리 루프를 설계할 때 쓰는 원칙. 이전 실행이 어디까지 했든 같은 최종 상태로 수렴하게 한다. /hstack:principle-make-operations-idempotent 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Make Operations Idempotent

상태를 바꾸는 모든 작업은 몇 번을 돌든, 이전 실행이 어디서 멈췄든 올바른 상태로 수렴하게 설계한다.

각 단계는 이전 실행이 하려던 것이 아니라 지금 있는 상태에서 끌어낸다. 현재 상태를 읽고, 목표와 비교하고, 차이만 적용한다.

실행이 남기는 것(락, 임시 파일, 반쯤 쓴 레코드, 가져간 작업)은 다음 실행이 낡은 것으로 알아볼 수 있어야 한다. 그 근거는 이전 실행이 어디서 멈췄어도 성립해야 한다. 임시 경로에 쓰고 rename으로 제자리에 옮긴다.

시험: 이것이 두 번 돌면 어떻게 되는가? 이전 실행이 가능한 모든 지점에서 크래시했다면? 답이 남겨진 것에 따라 달라지면 조정(reconciliation) 단계를 더한다.
