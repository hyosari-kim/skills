---
name: hyosari-principle-separate-before-serializing-shared-state
description: 동시에 움직이는 주체들이 같은 파일, 브랜치, 키, 상태 객체에 쓸 수 있을 때 쓰는 원칙. 먼저 공유를 없애고, 쓰는 쪽이 하나여야 하는 것이 진짜 불변식일 때만 구조적으로 직렬화한다. /hstack:hyosari-principle-separate-before-serializing-shared-state 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Separate Before Serializing Shared State

동시에 움직이는 주체들이 같은 파일, 브랜치, 키, 객체에 쓸 수 있으면 먼저 묻는다. 정말 공유 객체 하나가 필요한가, 아니면 서로 독립인 사실을 각자 내놓는 것인가. 기본은 주체마다 자기 파일, 키, 브랜치, 상태 디렉터리를 주고, 상태를 읽는 곳에서만 합치는 것이다. 두 워커가 `state.json` 하나에 각자 자기 필드를 쓰는 것도 여전히 공유 변경이다.

공유 쓰기 대상이 하나여야 하는 것이 진짜 불변식일 때만 접근을 구조적으로 직렬화한다(lockfile, 쓰는 쪽 하나, 순차 단계, compare-and-swap). 지시문은 동시성 제어가 아니다. "락이 필요하다"는 먼저 점검할 냄새다.
