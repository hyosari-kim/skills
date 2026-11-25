---
name: hyosari-principle-boundary-discipline
description: 검증, 오류 처리, 프레임워크 어댑터를 연결할 때 쓰는 원칙. 방어 코드는 시스템 경계(CLI, 설정, 네트워크, 외부 API)에 모으고, 내부에서는 타입을 믿고 비즈니스 로직을 순수 함수에 둔다. /hstack:hyosari-principle-boundary-discipline 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Boundary Discipline

검증, 타입 좁히기, 오류 처리는 데이터가 시스템 경계를 넘는 곳(CLI 인자, 설정 파일, 외부 API, 네트워크)에서 한다. 안쪽에서는 타입을 믿는다. 다시 검증하지 않고 불필요한 nil 검사도 하지 않는다. 원시 데이터는 경계에서 도메인 타입으로 파싱하고, 전송 계층과 프레임워크 타입은 공개 인터페이스에 드러내지 않는다. 비즈니스 로직은 프레임워크 의존이 없는 순수 함수에 둔다. 그것을 부르는 바깥 껍데기는 얇게 둔다.

시험은 둘이다. 이 데이터가 지금 경계를 넘고 있는가? 아니면 그 검증은 중복이다. 이것을 껍데기가 부르기만 하는 순수 함수로 만들 수 있는가? 그렇다면 뽑아낸다.
