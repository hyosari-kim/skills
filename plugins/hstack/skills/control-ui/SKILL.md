---
name: control-ui
description: 로컬 브라우저나 CDP 하네스를 만들거나 고쳐 웹, IDE, Electron UI를 조작하고 살핀다. 로컬 UI 검증, 스크린샷, 접근성 스냅샷, 성능 프로파일, 시각 비교, UI 버그 재현에 쓴다. /hstack:control-ui 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Control UI

로컬 브라우저 자동화로 UI 동작을 증거와 함께 검증한다. 저장소에 Playwright, 브라우저, Electron 하네스가 있으면 먼저 그것을 쓴다. 없으면 앱의 dev 서버나 Chromium 디버그 포트를 중심으로 임시 로컬 하네스를 꾸린다.

## 어디에 쓰나

- 실제 브라우저의 포커스, 키보드 입력, 스크롤, 크기 조절, 렌더링에 달린 UI 버그를 재현한다.
- 스크린샷과 스냅샷으로 시각 변경이나 접근성 변경을 검증한다.
- 내보내기 전에 로컬 웹, IDE, Electron 동작을 확인한다.
- 콘솔 로그, 네트워크 로그, CPU 프로파일, trace, heap snapshot을 남긴다.
- 작업 검증에 쓸 전후 증거를 만든다.

## 도구 고르기

위에서부터 쓸 수 있는 첫 도구를 고른다.

앱을 어떻게 띄우는지 모르면 먼저 Claude Code `run` 스킬(Skill 도구)로 띄우는 방법을 찾는다.

1. 저장소의 기존 하네스. Playwright 테스트, Cypress spec, Storybook, 브라우저 스크립트, Electron 실행 스크립트, 스냅샷 도구. 앱의 시작 방법, 환경, 선택자를 이미 알고 있다.
2. Playwright MCP(`mcp__plugin_playwright_playwright__*`). `browser_navigate`, `browser_snapshot`(접근성 트리), `browser_click`, `browser_type`, `browser_take_screenshot`, `browser_console_messages`, `browser_network_requests`. 도구가 지연 로드 상태면 `ToolSearch`로 한 번에 불러온다.
3. Claude 데스크톱 앱 내장 브라우저(`mcp__Claude_Browser__*`). 로컬 dev 서버는 `preview_start`로 띄우고 연다. 페이지 구조는 `read_page`나 `find`로 읽고, 조작은 `computer`, `form_input`, 로그는 `read_console_messages`, `read_network_requests`로 본다. 쓰기 전에 **anthropic-skills:built-in-browser** 스킬을 읽는다.
4. 임시 Playwright 스크립트. 아래 "일반 웹 하네스"처럼 `/tmp`에 두고 한 번 돌린다.

Electron이나 Chromium 앱의 디버그 포트에 붙어야 하면 아래 "일반 CDP 하네스"를 쓴다.

## 준비 절차

1. 저장소에 적힌 dev 명령으로 앱을 로컬에서 띄운다.
2. 기존 로컬 하네스를 찾는다. Playwright 테스트, Cypress spec, Storybook, 브라우저 스크립트, Electron 실행 스크립트, 스냅샷 도구.
3. 웹 앱이면 기존 브라우저 도구로 로컬 URL에 붙는다.
4. Electron이나 Chromium이면 지원될 때 원격 디버깅 포트를 켠다.
5. 탭 순서만이 아니라 안정적인 앱 표식으로 올바른 페이지를 고른다.
6. 좌표보다 접근성 role, label, 안정적인 `data-*` 선택자를 쓴다.

## 일반 웹 하네스

가능하면 저장소에 설치된 브라우저 도구를 쓴다. 저장소에 이미 Playwright가 있으면 일회용 최소 탐침은 이렇다.

```javascript
import { chromium } from "playwright";

const browser = await chromium.launch();
const page = await browser.newPage({ viewport: { width: 1280, height: 800 } });
await page.goto("http://127.0.0.1:<port>");
await page.getByRole("button", { name: /submit/i }).click();
await page.screenshot({ path: "/tmp/ui-harness-after.png", fullPage: true });
await browser.close();
```

사용자가 요청하지 않으면 이 탐침 하나 때문에 Playwright를 프로젝트 의존성으로 추가하지 않는다. 기존 dev 의존성이나 환경에 이미 있는 외부 브라우저 도구를 쓴다.

## 일반 CDP 하네스

Electron이나 `--remote-debugging-port=<port>`로 띄운 Chromium 앱은 CDP로 붙는다.

```javascript
import { chromium } from "playwright";

const browser = await chromium.connectOverCDP("http://127.0.0.1:<debug-port>");
const pages = browser.contexts().flatMap((context) => context.pages());
let page;
for (const candidate of pages) {
  if (await candidate.locator("<app-root-selector>").count()) {
    page = candidate;
    break;
  }
}

if (!page) {
  console.log(await Promise.all(pages.map(async (p) => ({
    title: await p.title(),
    url: p.url(),
  }))));
  throw new Error("No matching app page found");
}

await page.screenshot({ path: "/tmp/ui-harness-cdp.png", fullPage: true });
await browser.close();
```

`<app-root-selector>`는 현재 저장소의 안정적인 표식으로 바꾼다. 앱 루트 노드, landmark, 제품 고유의 `data-*` 속성 같은 것이다.

## 조작 루프

1. 행동하기 전에 페이지 스냅샷이나 스크린샷을 남긴다.
2. 가장 최근 페이지 구조에서 대상을 고른다.
3. 구조적 행동을 정확히 하나만 한다. 클릭, 입력, 키 입력, 드래그, 스크롤, 이동, 크기 조절.
4. 새 스냅샷이나 스크린샷을 남긴다.
5. 기대한 상태 변화를 확인한다.
6. 사용자가 증거를 요청했으면 전후 비교용 산출물을 저장한다.

## CDP 기능

상위 브라우저 API로 부족할 때만 raw CDP를 쓴다.

- 성능. CPU 프로파일, trace, paint flashing, FPS 미터, layout shift 점검.
- 메모리. 누수 조사를 위한 heap snapshot과 강제 GC.
- 네트워크. 요청 차단, 속도 제한, 캐시 끄기, 요청과 응답 로그.
- 렌더링. viewport 변경, color scheme 에뮬레이션, reduced motion, 접근성 점검.
- 디버깅. 콘솔 스트리밍, 예외 포착, DOM 스냅샷.

## 페이지 고르기

디버그 포트 하나를 여러 앱 창이나 탭이 함께 쓰면 이렇게 한다.

- 검사 대상 화면의 긍정 표식(앱 루트 선택자 같은)을 쓴다.
- 필요하면 부정 표식으로 엉뚱한 화면을 피한다.
- 맞는 페이지가 없으면 추측하지 말고 페이지 제목과 URL 목록을 출력한다.

## 지킨다

- 이동이나 구조 변경 뒤에 낡은 요소 참조에 기대지 않는다.
- 클릭 직전에 새 스크린샷을 찍은 게 아니면 좌표 클릭을 피한다.
- 테스트 데이터는 로컬에 두고 버릴 수 있게 한다.
- 사용자가 명시적으로 동의하지 않으면 개인정보에 민감한 작업 공간의 스크린샷이나 heap snapshot을 저장하지 않는다.
- 다른 저장소의 선택자, 포트, 스크립트 경로를 하드코딩하지 않는다. 현재 저장소의 로컬 앱 표식을 찾는다.
- 끝나면 dev 서버, 디버그 세션, 임시 프로필을 정리한다.
