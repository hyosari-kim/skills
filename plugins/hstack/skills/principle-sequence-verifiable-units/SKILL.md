---
name: principle-sequence-verifiable-units
description: 여러 단계로 된 작업(일괄 정리, 마이그레이션, 비슷한 수정의 연속)과 커밋·PR을 쌓는 방식에 쓰는 원칙. 작업을 저마다 확인할 수 있는 상태로 끝나는 작은 단위로 나누고, 다음으로 가기 전에 하나씩 확인하고, 그 순서 자체가 리뷰어에게 증명이 되게 전달한다. /hstack:principle-sequence-verifiable-units 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Sequence work into verifiable units

작업을 저마다 확인할 수 있는 상태로 끝나는 작은 단위로 늘어놓고, 지금 단위가 통과하기 전에는 다음으로 가지 않는다.

일괄 정리, 마이그레이션, 비슷한 수정의 연속에서 각 단위는 하나의 괄호다. 잘 되는 상태, 변경 하나, 확인 실행, 진행. 도구가 수정을 해도 단위마다 확인을 돌린다.

작업을 증명하는 순서로 전달한다. 실패하는 테스트가 먼저, 그다음 수정. 다듬기 전에 덜어내기. 처치 전에 기준선. 커밋 하나하나가 혼자 서야 한다.
