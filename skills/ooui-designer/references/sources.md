# 출처와 참고 자료

이 스킬은 아래 자료의 내용을 에이전트용 규칙으로 재구성해 요약한 것이다. 원문을 옮기지 않았으므로, 원문 표현이 필요하면 해당 자료를 직접 확인하도록 안내한다.

## 원전
- ソシオメディア株式会社, 上野学, 藤井幸多, 『オブジェクト指向UIデザイン──使いやすいソフトウェアの原理』, 技術評論社 (WEB+DB PRESS plus), 2020. 개정신판(改訂新版) 2026년 7월 출간. 개정신판은 본문 변경 없이 전반부 도판을 컬러화하고 후기를 추가한 판이다.
  - 본문은 공개되어 있지 않아 이 스킬은 목차(출판사 페이지)와 저자들의 공개 글을 근거로 한다. 목차: https://gihyo.jp/book/2026/978-4-297-15716-6
  - 3레이어(모델·인터랙션·프레젠테이션) 설계 단계, 레이아웃 패턴 카탈로그의 원천. 후반부에 18개의 실습 과제(ワークアウト)가 있다
- 藤井幸多, 「OOUIデザインのトレーニング：アクションのオブジェクト化」, The Art of User Interface Design, 2021. https://atochotto.com/654
  - 책 공저자의 글. 액션 이름에서 숨은 속성·객체 꺼내기, 추상적 액션명의 변환, 액션 뒤에 숨은 객체, 동사의 명사화 (`modeling.md` AO1~AO5의 원천)
- 藤井幸多, 「OOUIデザインのトレーニング：ゲームの服屋」, 2021. https://atochotto.com/699
  - 다른 뷰가 역할을 흡수한 싱글의 생략, 작업 대상 객체의 싱글만 두기, 다른 객체의 컬렉션 + 싱글 병치, 태스크 선택(사기/팔기)을 두 객체의 컬렉션으로 바꾸기, 다중 선택 패턴, 연속 작업 (`views-navigation.md` O4·O5·C4·C6·N6, `layout-patterns.md` 다중 선택 패턴의 원천)
- 藤井幸多, 「OOUIデザインのトレーニング：モデルとプレゼンテーションの関係」, 2021. https://atochotto.com/1590
  - 관련 객체를 속성으로 표시, 생성 액션은 컬렉션에만, 객체 이름을 루트 라벨·컬렉션 제목·돌아가기 라벨에 공통 사용 (C5, R8)
- 上野学, 「OOUI – オブジェクトベースのUIモデリング」, Sociomedia, 2016. https://www.sociomedia.co.jp/7279
  - 객체 판정 기준, 컬렉션/싱글, 태스크 지향 예외 조건, 태스크 → 객체 전환 사례, IBM CUA·OVID 등 역사. 메일함 싱글의 격하, 기업 싱글과 사람 컬렉션의 일체화, 페이지 싱글의 두 면(설정/내용) (O2·C1·M2)
- 上野学, 「OOUI の目当て」, Sociomedia, 2019. https://www.sociomedia.co.jp/8740
  - 명사 → 동사 구문, 모드리스, 되돌리기의 의미, 컬렉션 형식이 앱의 성격을 정한다는 관점. 계약의 참조용/편집용 싱글 분리와 편집용 싱글의 생성 화면 재사용, 마스터-디테일 (M1·C3)
- Sociomedia OOUI 트레이닝 소개. https://www.sociomedia.co.jp/9556

## 해설·실무 자료
- teamLab, 「複雑なUI設計への銀の弾丸 オブジェクト指向UIデザイン」, 2025. https://speakerdeck.com/teamlab/object-oriented-ui-design
  - 루트 네비게이션, 뷰 연결, 컬렉션 형식, 필터, 싱글 표시, 생성·수정·삭제 패턴 요약
- k-sato, 「オブジェクト指向UIデザイン読書メモ」, Zenn. https://zenn.dev/k_sato/articles/4743447a49bd78
- i3DESIGN, 「オブジェクトベースのUI(OOUI)」. https://www.i3design.jp/in-pocket/8790
- nijibox, 「OOUI(オブジェクト指向UI)とは？」. https://blog.nijibox.jp/article/ooui/
  - 인스턴스가 부모당 몇 개뿐인 객체의 싱글 생략 사례 (O3)
- miki horio, 「OOUI実践編-ビューとナビゲーションの検討」, note. https://note.com/leony/n/na16e133aa2ac
  - 책 내용 정리. 기계적으로 컬렉션·싱글을 두고 다중도로 호출 관계를 잇는 절차, 루트 네비게이션 라벨 규칙 (도출 절차, R3·R7)
- osanai, 「オブジェクト指向UIデザインのトレーニング ─ バンドルカード篇」, note. https://note.com/katsunori_osanai/n/nf79489f2f64d
- Goodpatch, 「UIデザイナーのスキルとOOUI観点の構造設計」. https://goodpatch.com/blog/uidesigner-skill-ooui-structure
- freee, 「2年目デザイナーが始めるOOUI」. https://developers.freee.co.jp/entry/start-userstory-ooui
  - 유저 스토리에서 "신청"을 객체로, "승인"을 그 액션으로 정리한 사례 (AO6의 참고)

## 관련 개념
- OOUX / ORCA (Sophia Prater): 영어권의 객체 지향 UX 방법론. 객체, 관계, 액션(CTA), 속성 순으로 모델링한다. 이 스킬의 Step 1과 대응한다.
- IBM Common User Access (1989), Dave Collins 『Designing Object-Oriented User Interfaces』 (1995), IBM 『Designing for the User with OVID』 (1998): OOUI의 역사적 원류.

## 이 스킬이 덧붙인 부분
원전의 개념 위에 다음을 추가했다: 규칙 ID 체계(P·T·V·O·C·M·N·R·AO), 객체 역할 구분표, 기본값 표, 모바일 관용구 대응표, 삭제의 되돌리기 패턴, 산출물 템플릿, 구현 매핑. AO6(행위가 기록으로 남으면 객체로)은 여러 사례를 일반화해 이 스킬에서 규칙으로 정리한 것이다. 뷰의 생략·결합(O·C·M)은 원전들에 흩어진 사례를 분류해 이름을 붙인 것이고, 책의 칼럼 「シングルビューとコレクションビューの省略」 본문은 확인하지 못했다.
