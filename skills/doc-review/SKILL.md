---
name: doc-review
description: spec·plan 등 구현용 문서를 doc-reviewer 에이전트로 검토한다. 사용자가 /doc-review 로 부를 때만 쓴다.
disable-model-invocation: true
argument-hint: "[문서 경로...]"
---

# 문서 리뷰 (doc-reviewer)

## 1. 대상 정하기

- 인자로 경로(파일·폴더, 여러 개 가능)를 받으면 그것.
- 없으면 이 대화에서 작성·수정한 spec·plan 문서의 경로를 모두.
- 어느 문서인지 불분명하면 부르기 전에 묻는다.

## 2. 부르기

- Agent 도구로 `subagent_type: "doc-reviewer"` 를 부른다.
- prompt 에는 **문서 경로만** 넣는다. 대화 맥락·결정 배경·요약은 넣지 않는다.

## 3. 결과 전달

- 지적 중 코드로 사실 여부를 가릴 수 있는 것은 직접 확인하고, 틀린 지적은 그렇다고 표시한다.
- 구현 분기점과 spec↔plan 어긋남은 **결정하거나 문서에 반영하지 않는다.** 선택지와 추천을 붙여 사용자에게 묻는다.
- 문서를 고치지 않는다. 반영은 사용자가 정한 뒤에 한다.
