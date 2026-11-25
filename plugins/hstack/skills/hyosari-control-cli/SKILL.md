---
name: hyosari-control-cli
description: 외부 서비스 없이 로컬 하네스를 만들거나 고쳐 대화형 CLI나 TUI를 조작하고 살피고 프로파일한다. CLI UX 확인, 시작 시간 회귀, 메모리 누수, 멈춤, 프롬프트 흐름, 터미널 데모에 쓴다. /hstack:hyosari-control-cli 로 호출하거나 hyosari-mode가 가리킬 때 쓴다.
disable-model-invocation: true
---

# Hyosari control CLI

대화형 CLI를 손으로 찔러 보지 말고 다시 돌릴 수 있는 로컬 하네스로 다룬다. 저장소에 테스트나 데모 하네스가 있으면 먼저 그것을 쓴다. 없으면 표준 로컬 도구로 임시 하네스를 꾸린다. 기본값은 tmux다.

## 어디에 쓰나

- 결정적인 입력으로 CLI나 TUI 버그를 재현한다.
- 키보드 흐름, 프롬프트, 인터럽트, 크기 조절 동작, 터미널 레이아웃을 검증한다.
- 버그 수정의 전후 기록(transcript)을 남긴다.
- 시작 시간, 느린 작업, 멈춤, 메모리 증가를 프로파일한다.
- 설명보다 출력으로 보여 주는 게 쉬울 때 짧은 터미널 데모를 녹화한다.

## 하네스 루프

0. CLI를 어떻게 빌드하고 띄우는지 모르면 먼저 Claude Code `run` 스킬(Skill 도구)로 띄우는 방법을 찾는다.
1. 검사할 명령과 가장 작은 재현 작업 공간을 정한다.
2. 기존 로컬 하네스를 찾는다. package 스크립트, e2e 테스트, 데모 녹화기, expect 스크립트, PTY 헬퍼.
3. 하네스가 없으면 결정적인 환경 변수를 준 격리된 tmux 세션(`tmux new-session -d`)에서 CLI를 띄운다.
4. 조작하기 전에 현재 화면을 `tmux capture-pane -p`로 남긴다.
5. 한 번에 행동 하나만 `tmux send-keys`로 보낸다. 텍스트, Enter, 화살표, Escape, Ctrl-C, 크기 조절.
6. 다음 행동 전에 구체적인 화면 패턴이나 프롬프트가 나타나기를 기다린다.
7. 기록과 프로파일 산출물을 저장한다.
8. 세션을 깨끗하게 종료한다.

## 하네스 선택지

- 저장소 고유 하네스. 앱의 시작 방법, 환경, 프롬프트를 알고 있으니 체크인된 스크립트를 먼저 쓴다.
- `tmux`(기본값). 관리되는 세션, `capture-pane`, `send-keys`, attach와 detach.
- PTY 탐침. tmux가 없으면 짧은 Python, Node, Expect 스크립트를 쓴다.
- 런타임 인스펙터. CPU 프로파일, heap snapshot, 실시간 평가에는 Node나 Bun 인스펙터를 쓴다.
- 터미널 녹화기. 사용자가 데모를 요청하면 저장소의 데모 도구나 asciinema 호환 도구를 쓴다.

## 최소 tmux 하네스

```bash
SESSION="cli-harness-$(date +%s)"
tmux new-session -d -s "$SESSION" -- <command-under-test>
tmux capture-pane -pt "$SESSION"
tmux send-keys -t "$SESSION" "help" Enter
tmux capture-pane -pt "$SESSION"
tmux kill-session -t "$SESSION"
```

Node CLI라면 이렇게 띄운다.

```bash
NODE_OPTIONS="--inspect=127.0.0.1:0" tmux new-session -d -s "$SESSION" -- <node-cli-command>
```

터미널 출력에서 인스펙터 URL을 찾고, 프로파일이 필요하면 Chrome DevTools 호환 도구를 쓴다.

## 최소 PTY 하네스

tmux도 데모 하네스도 없는 저장소에서 결정적인 대기가 필요하면 PTY 스크립트를 쓴다. 사용자가 재사용할 테스트로 추가하라고 하지 않으면 임시로 둔다.

```python
import os
import pty
import select
import subprocess
import time

master_fd, slave_fd = pty.openpty()
proc = subprocess.Popen(
    ["<command>", "<arg>"],
    stdin=slave_fd,
    stdout=slave_fd,
    stderr=slave_fd,
    close_fds=True,
)
os.close(slave_fd)

deadline = time.time() + 30
buffer = b""
while time.time() < deadline:
    ready, _, _ = select.select([master_fd], [], [], 0.25)
    if not ready:
        continue
    chunk = os.read(master_fd, 4096)
    buffer += chunk
    if b"<ready text>" in buffer:
        os.write(master_fd, b"help\n")
        break

print(buffer.decode(errors="replace"))
proc.terminate()
os.close(master_fd)
```

CLI가 더 풍부한 터미널 제어를 요구하면 `pty.fork()`나 기존 PTY 라이브러리를 쓴다.

## 프로파일 방법

- 시작 시간 회귀. 같은 기계, 같은 환경, 같은 명령에서 기준과 변경의 시작 시간을 잰다.
- 느린 작업. CPU 프로파일을 시작하고, 작업을 하고, 프로파일을 멈추고, self-time 상위 함수를 비교한다.
- 메모리 누수. 가능하면 GC를 강제하고, heap snapshot을 찍고, 작업을 반복하고, 다시 GC를 강제하고, snapshot을 한 번 더 찍는다.
- 멈춤. 인터럽트하기 전에 화면, 살아 있는 핸들과 자원, 스택이나 CPU 샘플을 남긴다.

## 지킨다

- sleep보다 결정적인 대기를 쓴다. sleep을 꼭 써야 하면 이유를 적는다.
- 조작 중인 세션에 자격 증명이나 파괴적인 명령을 보내지 않는다.
- 저장소에 테스트나 데모 하네스가 이미 있는 게 아니면 하네스는 `/tmp`에 둔다.
- 다른 저장소의 경로를 하드코딩하지 않는다. 현재 저장소의 스크립트와 런타임에 맞춰 명령을 고친다.
- 사용자가 남기라고 하지 않으면 tmux 세션, 임시 디렉터리, 인스펙터 프로세스, 데모 산출물을 정리한다.
