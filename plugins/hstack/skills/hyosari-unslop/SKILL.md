---
name: hyosari-unslop
description: 글에서 AI가 쓴 티가 나는 패턴을 찾아 지운다. 문서, PR 본문, 커밋 메시지, 답변을 다듬을 때 쓴다. /hstack:hyosari-unslop 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Hyosari unslop

글에서 AI 패턴을 지운다. 아래 패턴을 훑은 뒤 다시 쓴다. 뜻은 지키고 의도한 어조에 맞춘다.

규칙 번호는 다른 스킬이 인용하는 고정 id다. 지운 규칙은 번호를 비워 둔다.

3. 사실을 더하지 않는 꼬리 "-ing" 구절을 쓰지 않는다("highlighting...", "ensuring...").
5. 막연한 출처("experts believe")를 쓰지 않는다. 출처를 밝히거나 지운다.
7. AI 어휘를 쓰지 않는다. additionally, crucial, delve, enhance, foster, landscape, pivotal, showcase, testament, underscore, vibrant.
8. "serves as", "stands as", "boasts", "features" 대신 "is"나 "has"라고 쓴다.
9. "not just X, but Y"를 쓰지 않는다. 요점을 그대로 말한다.
10. 생각을 억지로 셋씩 묶지 않는다.
11. 한 대상에는 이름 하나만 쓴다. 그 이름을 반복한다.
12. X와 Y가 실제 척도 위에 있지 않으면 "from X to Y"를 쓰지 않는다.
13. em dash, en dash, 대시 대용 하이픈을 쓰지 않고, 그 자리에 괄호도 쓰지 않는다. 마침표나 쉼표를 쓴다.
14. 콜론은 목록이나 예시 앞에만 둔다. 문장 중간의 연결어로 쓰지 않는다.
15. 고유명사나 약어마다 굵게 하지 않는다.
16. 그 줄을 되풀이하는 굵은 라벨과 콜론을 쓰지 않는다("**Performance:** Performance improved..."). 마침표로 끝나고 새 정보를 더하는 굵은 도입구는 괜찮다.
17. 제목은 sentence case로 쓴다.
18. 장식용 이모지를 쓰지 않는다.
19. 곧은 따옴표를 쓴다.
20. 챗봇 문구("I hope this helps!", "Let me know if...")를 쓰지 않는다.
22. 칭찬이나 동의로 시작하지 않는다. 바로 답한다.
23. 군더더기를 지운다. "In order to"는 "to"로 바꾼다. "it is important to note that"은 지운다.
24. 얼버무림은 많아야 하나만 쓴다. "may".
25. 막연한 결론을 쓰지 않는다. 구체적인 계획이나 사실을 쓴다.
26. 추상적인 은유 명사를 쓰지 않는다. substrate, wedge, vector, nexus, primitive, harness, surface, scaffolding, paradigm, ratchet, north star, flywheel. 구체적인 단어를 쓴다.
27. 어떤 느낌인지가 아니라 무엇을 하는지 쓴다. 메커니즘이나 숫자를 댄다. 다른 프로젝트 문서에 그대로 들어가도 되는 문장은 지운다.
28. 한 문장에 생각 하나만 담는다. 독자가 다시 읽어야 하는 문장은 나눈다.
29. 능동태로 쓴다. 행위자를 모르거나 중요하지 않을 때가 아니면 행위자를 밝힌다.
30. 부사를 지우거나 더 강한 동사를 쓴다. 숫자를 댄다.
31. 쉬운 단어를 쓴다. "utilize"나 "leverage"가 아니라 "use".
32. 문자 그대로의 표현이 있으면 은유, 경구, 의인화한 코드, 비유적 동사를 쓰지 않는다.
33. 관사와 동사를 갖춘 온전한 문장으로 쓴다. 화살표와 약어는 풀어 쓴다.
