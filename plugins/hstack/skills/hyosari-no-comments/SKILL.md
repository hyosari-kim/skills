---
name: hyosari-no-comments
description: Comment Sicko 에이전트를 띄워 범위 안의 주석을 지우게 하고, 받아들인 지적을 고치고, 주석이 주장하는 제약을 코드로 옮길 방법을 제안한다. /hstack:hyosari-no-comments 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Hyosari no comments

Comment Sicko를 띄운다. 받아들인 지적대로 행동한다.

Comment Sicko의 새 시각을 따른다.

## 범위

호출한 쪽이 준 파일이나 diff를 쓴다. 없으면 기본 브랜치(기본값 `main`) 대비 현재 diff를 작업 트리까지 포함해 쓴다.

## 절차

1. `Agent`를 `subagent_type: "hstack:hyosari-comment-sicko"`로 띄운다. 범위를 넘긴다. 서브에이전트마다 일에 맞는 추론 수준을 **hyosari-mode**의 Subagents 섹션에 따라 고른다. 그 에이전트의 규칙을 다시 적지 않는다.
2. 보고와 diff를 검사한다. 애플리케이션 코드 수정, 범위 이탈, 예외로 보호되는 삭제, 사유를 잘못 적은 `MUST KILL`, 일부러 남긴 코드를 죄인 취급한 지적은 거부한다. 우리 코드의 놀라운 동작에 붙은 재구성 지적은 계속 처리 대상이다. 그 주석은 되살리지 않는다. 남기기(keep)는 우리가 바꿀 수 없는 것에 관한 주석이라는 증거가 있을 때만 산다. 범위 안의 lint와 TypeScript 억제 주석 중 놓친 것을 감사한다. 정확성이나 안전을 위한 억제는 처리 대상 `MUST KILL`로 남긴다. 삭제는 정확한 예외와 범위 안의 증거가 있을 때만 되살린다. 근거가 얇은 `IMPORTANT`나 `do not remove`의 삭제나 남기기를 받아들이기 전에 그 심볼을 추적하거나 **hyosari-why**를 돌린다. 삭제가 모호하면 되살리지 않는다. 남기기가 반박되거나 여전히 모호하면 지운다. 거부한 보고 하나는 되돌리고 실패를 짚어 한 번 다시 돌린다. 두 번째도 거부하면 열린 문제로 보고하고 **hyosari-no-comments**를 실패로 끝낸다.
3. 받아들인 단순한 지적은 직접 고친다. 죽은 경로를 지우거나, 매개변수를 빼거나, 실제 API를 쓴다. 고치는 데 형태 설계가 필요하면 받아들인 지적 전체와 주변 코드에 대해 **hyosari-architect**를 한 번 돌린다. 스케치에서 멈춘다. architect는 형태를 잡는다. 구현은 4단계가 한다.
4. 범위 안에서 가장 작은 근본 원인 수정을 구현한다. 이름이 나온 우회 코드는 모두 없앤다. 근본 원인이 범위 밖이면 범위 안의 가장 작은 수정을 넣고 나머지는 열린 문제로 보고한다. **hyosari-principle-fix-root-causes**와 **hyosari-principle-redesign-from-first-principles**는 의도만 안내한다. 둘 다 울타리를 넓히거나 울타리 밖의 사례를 고칠 권한을 주지 않는다. 증상을 막는 가드를 덧붙이지 않는다.
5. 제약 주석은 `do not remove`, `do not change wording`, `talk to X before changing` 같은 말을 한다. 우리가 바꿀 수 없는 것에 관한 남기기는 그대로 둔다. 범위 안에서 가장 싼 타입, 런타임 검사, 테스트, CI lint를 제안한다. 대화 중이면 승인을 기다린다. 무인 실행과 eval에서는 호출한 쪽의 사전 승인이 있어야 한다. 승인되면 코드로 옮긴 뒤 주석을 지운다. 아니면 지우고, 그 제약을 열린 문제로 보고하고, 범위 밖 작업을 스케치한다.
6. 삭제 수, 되살린 주석, 재실행, architect 스케치, 수정, 코드화 제안, 코드화한 것, 강제되지 않는 제약, 그 밖의 열린 작업을 보고한다.
