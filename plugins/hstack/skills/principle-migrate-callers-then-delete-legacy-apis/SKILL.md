---
name: principle-migrate-callers-then-delete-legacy-apis
description: 옛 호출부가 남아 있는 채로 새 내부 API를 들일 때 쓰는 원칙. 호환 계층을 남기지 말고 같은 흐름 안에서 호출부를 옮기고 옛 API를 지운다. /hstack:principle-migrate-callers-then-delete-legacy-apis 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Migrate Callers Then Delete Legacy APIs

- 테스트가 새 계약을 확인하도록 고치고, 리팩터링 전 구현 세부만 지키는 테스트는 지운다.

**적용되는 경우:**
- 하위 호환에 기대는 외부 사용자가 없다.
- 프로젝트가 조율된 호환성 깨짐을 감당할 수 있다.
- 새 API가 단순화나 리팩터링 작업의 일부다.
