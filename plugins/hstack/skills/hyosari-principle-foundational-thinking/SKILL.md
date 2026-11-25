---
name: hyosari-principle-foundational-thinking
description: 로직을 쓰기 전에 쓰는 원칙. 핵심 타입과 자료구조를 고를 때, 뼈대 작업과 기능 작업의 순서를 정할 때, 동시에 움직이는 주체가 무엇을 공유하는지 물을 때. 자료구조를 맞게 잡아 이후 코드가 뻔해지게 한다. /hstack:hyosari-principle-foundational-thinking 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Foundational Thinking

로직을 쓰기 전에 데이터 형태를 맞게 잡는다. 핵심 타입을 일찍 정하고, 모든 접근 패턴을 따라가 보고, 주된 경로에 맞는 구조를 고른다. 모든 줄이 아니라 구조를 DRY하게 만든다.

주체들 사이에 상태를 공유하기 전에, 다른 주체가 그것을 동시에 바꾸면 무슨 일이 생기는지 묻는다. 답이 "아무 일도 없다"가 아니면 분리한다.

뼈대부터 세운다. 이후 모든 단계를 돕는 것(CI, 테스트, 공유 타입)을 죽은 코드를 지운 뒤 맨 먼저 한다. 커밋은 작게, 목적 하나로 유지한다.
