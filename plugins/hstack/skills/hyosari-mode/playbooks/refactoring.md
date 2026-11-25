### Refactoring

**계약을 소유한다. 구조는 바뀌고 동작은 바뀌지 않는다.** 동작을 추가하는 Feature, 동작을 바로잡는 Bug fix와 다르다.

정리하다 버그나 빠진 기능을 발견하면 따로 떼어낸다. 고정된 계약 위에서 구조 변경을 먼저 낸다. 재설계가 필요하면 그렇다고 이름 붙이고 Feature로 보낸다. 크거나 여러 곳을 가로지르는 구조 작업은 **hyosari-figure-it-out**이 맡는다. 이 플레이북은 작거나 중간 크기의 변경용이다.

1. 동작을 먼저 고정한다. 영향받는 부분을 따라 읽어 계약을 파악하고, 구조를 건드리기 전에 현재 동작을 잡는 특성화 테스트, 스냅샷, 동등성 스크립트 중 하나를 쓴다. 커버리지가 없으면 고정부터 만든다. 타입 체크와 린트는 고정이 아니다.
2. 목표 구조를 적는다. 오늘 새로 만든다면 모듈, 타입, 호출 관계가 어떤 모양이어야 하는지다. 재구성은 분기나 불가능한 상태를 지워야 한다. 간접층을 더하면 안 된다. 구조의 기준은 **hyosari-principle-model-first**다. 함수나 모듈 경계를 넘으면 **hyosari-architect**를 쓴다. 코드에 빠진 구조가 무엇인지 이름 붙인다. 형태가 이미 분명하고 국소적이면 단순한 코드를 그대로 둔다. 목표 구조는 **hyosari-principle-foundational-thinking**과 **hyosari-principle-redesign-from-first-principles**를 따른다. **hyosari-architect**는 옮기기 전에 쓴다.
3. 더하기 전에 뺀다(**hyosari-principle-subtract-before-you-add**, **hyosari-principle-smallest-change**). 죽은 코드, 호출처가 하나뿐인 래퍼, 중복 검증, 고아 참조를 먼저 지운다. 목표 구조에 닿는 가장 작은 변경만 낸다. "도움이 될 수도 있는" 추측성 정리는 되돌린다.
4. 동작을 보존하는 작은 단계로 움직이고, 매 단계 고정이 통과해야 한다. API를 바꾸면 호출처를 전부 옮기고 옛 API를 같은 묶음에서 지운다. 호환용 shim이나 새것과 옛것의 병렬 경로를 두지 않는다. 이름 변경은 문자열, 문서, 역참조에서 누락이 없는지 실제 파일로 확인한다. 이름 변경은 이런 곳에서 조용히 빠지기 쉽다. API 이전은 **hyosari-principle-migrate-callers-then-delete-legacy-apis**를 따른다. 기계적인 수정은 범위(파일 경로, 옮기는 이름, 유지할 동작)를 정해 서브에이전트에 맡긴다. 모델은 hyosari-mode의 Subagents 섹션에 refactoring 역할 기본값이 있으면 그것을, 없으면 `model: "opus", effort: "xhigh"`를 쓰고, 추론 수준은 일에 맞게 고른다.
5. 동작이 안 바뀌었음을 실제 산출물로 증명한다(**hyosari-principle-prove-it-works**). "컴파일된다"는 증거가 아니다. 큰 변경은 이전과 이후의 출력을 diff하는 동등성 확인을 돌린다. 동등성 확인은 이전과 이후 출력을 diff하는 스크립트, 기록한 기준선을 새 코드에 재생하기, **hyosari-control-ui**나 **hyosari-control-cli**로 같은 곳에서 돌리는 스모크 실행 중 하나다.
6. 읽는 부담이 어디서 줄었는지 확인한다. 어디서도 줄지 않았으면 되돌린다. 성공의 척도는 읽는 부담의 감소다(**hyosari-principle-minimize-reader-load**).
7. 작고 순서 있는 커밋으로 정리한다(**hyosari-principle-sequence-verifiable-units**). 삭제 커밋, 재구성, 후속 정리 순서다. 각 조각은 다음으로 넘어가기 전에 통과 상태여야 한다. **Opening a PR**을 실행한다.

**Reply:** 바뀐 구조, 고정에 쓴 것, 동작 불변 증명, 읽는 부담의 변화, 출하한 것과 되돌린 것. 새 동작은 없다.
