---
name: hyosari-principle-subtract-before-you-add
description: 기능 추가, 리팩터링, 재작성의 순서를 정할 때 쓰는 원칙. 죽은 코드, 중복 검증기, 빈 참조를 먼저 지우고 더 단순해진 바탕 위에 짓는다. /hstack:hyosari-principle-subtract-before-you-add 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Subtract Before You Add

시스템을 발전시킬 때 복잡도를 먼저 덜고 그다음 짓는다. 기본은 빼기다.

단순화를 꾸준한 투자로 삼는다. 처음 봤을 때보다 같거나 더 작은 겉면 뒤에서 설계를 조금 더 단순하고 더 유능하게 남긴다.

- 짓기 전에 지우는 순서로 둔다.
- 다듬기 전에 자른다. 품질에 투자하기 전에 최소한으로 줄인다.
- 추측한 경계 사례가 아니라 실제로 관찰된 사용에 맞춰 설계한다.
- 명세가 요구하는 것을 넘어서는 추측성 검증기, 파서, 방어 코드를 두지 않는다.
- 프롬프트를 단순하게 한다. 중복 지시와 지나친 템플릿을 지운다.
- 참조에 새로운 내용이 없으면 빈 껍데기로 남기지 말고 지운다.
