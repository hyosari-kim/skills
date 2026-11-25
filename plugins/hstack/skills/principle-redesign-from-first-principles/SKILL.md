---
name: principle-redesign-from-first-principles
description: 새 요구사항을 기존 설계에 들일 때 쓰는 원칙. /hstack:principle-redesign-from-first-principles 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Redesign From First Principles

변경을 들일 때 기존 설계에 덧대지 않는다. 그 요구사항이 처음부터 기본 가정이었던 것처럼 다시 설계한다.

- 영향받는 파일을 모두 읽고 현재 설계를 이해한다.
- 묻는다. "이 새 요구사항을 안고 처음부터 쓴다면 무엇을 만들까?"
- 변경을 모든 참조로 퍼뜨린다. 타입, 문서, 예시, 근거 설명 부분.
- 재설계 전체를 생각하고, 전달은 조금씩 한다.

이것이 기존 설계에 변경을 들일 때 선택지의 가치를 지키는 방법이다.
