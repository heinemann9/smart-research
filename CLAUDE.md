# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 개요

문서 전용 저장소로, 리서치 및 RAG 시스템 구축을 위한 워크플로우 가이드를 포함합니다. 세 가지 도구를 조합합니다:

- **Claude Code** — 오케스트레이터 및 분석가
- **notebooklm-mcp-cli** — Google NotebookLM용 MCP 서버 & CLI (Gemini 기반 RAG)
- **Obsidian** — 지식 그래프 저장소 (마크다운, 위키링크, Dataview 대시보드)

주요 파일: `docs/research-workflow-guide.md`, `docs/project-plan.md`

## 프로젝트 환경

- `notebooklm-mcp-cli` v0.4.4 — 프로젝트 로컬 설치 (`.venv/`), 실행 시 `uv run nlm ...` 사용
- NotebookLM 인증: `sinankng@gmail.com` (default 프로필)
- Obsidian vault: `~/obsidian-vault/research/`
- 산출물 원본: `output/` 폴더

## 핵심 워크플로우

1. **리서치 모드**: 소스 수집 → AI 분석 → 산출물 생성 → Obsidian 저장
2. **RAG 모드**: 프로젝트 CLAUDE.md에 NotebookLM 노트북 등록 → 코딩 중 MCP `notebook_query`로 자동 질의

## 규칙

- 모든 문서는 한국어로 작성. 편집 시 한국어 유지.
- `nlm` CLI 도구(`notebooklm-mcp-cli`)와 Claude Code MCP 서버 연동을 기준으로 작성됨.
- Obsidian vault 구조: `~/obsidian-vault/research/` 하위에 `inbox/`, `projects/`, `sources/`, `concepts/`, `artifacts/`, `cache/`, `templates/`, `dashboards/` 디렉토리 사용.

## nlm CLI 주의사항 (검증 결과)

- **Rate Limiting**: 연속 질의 시 `INVALID_ARGUMENT` 에러 발생 가능. 질의 간 딜레이 필요.
- `nlm data-table create <notebook_id> "<description>"` — description은 positional argument (플래그 아님)
- `nlm download <type> <notebook_id> --id <artifact_id> -o <path>` — artifact ID는 `--id` 옵션으로 지정
- `nlm quiz create --difficulty` — 정수형 값 사용 (문자열 `hard`/`medium` 아님)

## Agent 워크플로우 규칙

### 검증 후 완료 보고
- 구현 완료를 보고하기 전에 반드시 관련 테스트를 실행하고 통과를 확인한다.
- C++ 변경: `cmake --build` + `ctest -R <관련테스트>`
- 프론트 변경: `npm run lint` + `npx vitest run <관련파일>`
- "빌드 성공"만으로는 완료가 아니다. 테스트까지 통과해야 완료다.

### 계획 우선
- 3개 이상의 파일을 수정하거나, 새로운 모듈/인터페이스를 추가하는 작업은 구현 전에 계획을 먼저 수립한다.
- 계획에는 수정 대상 파일, 변경 내용 요약, 영향 범위를 포함한다.
- 구현 중 계획과 달라지면 재계획 후 진행한다.

### 자율적 버그 수정
- 빌드 실패나 테스트 실패 발생 시, 사용자에게 질문하기 전에 직접 원인을 추적하고 수정을 시도한다.
- 에러 메시지 → 관련 소스 → 수정 → 재빌드/재테스트 순서로 진행한다.
- 3회 시도 후에도 해결되지 않으면 사용자에게 상황을 보고한다.

### 자기 개선 루프
- 사용자에게 교정받은 규칙(커밋 메시지 형식, 코드 스타일, 작업 방식 등)은 즉시 이후 작업에 반영한다.
- 반복적으로 교정받는 패턴이 발생하면 CLAUDE.md 또는 AGENTS.md에 명시적 규칙으로 추가를 제안한다.

### 우아한 구현 추구
- 비자명한 로직(상태 머신 전이, 프로토콜 변환, 비동기 흐름 등)을 구현한 후, 더 간결하거나 명확한 구조가 없는지 한 번 검토한다.
- 불필요한 복잡도가 발견되면 즉시 리팩토링한다. 단, 동작하는 코드를 과도하게 추상화하지는 않는다.

## 커밋 규칙

- 커밋 메시지에 `Co-Authored-By` 라인을 포함하지 않는다.
- 커밋 메시지는 **한글**로 작성한다.
- 커밋 메시지 형식: `<type>(#<이슈번호>): <제목>`
  - 이슈번호가 없는 경우 `#0`으로 표기한다.
  - type: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `ci` 등
  - 예: `feat(#12): WebSocket 정적 파일 서빙 추가`
  - 예: `fix(#0): heartbeat 타임아웃 로직 수정`
- **본문(body)**에 세부 변경 내역을 포함한다.
  - 본문 문체는 **명사형 완료 형태**로 작성한다 (예: `~처리`, `~추가`, `~수정`, `~제거`).
  - 항목이 여러 개일 경우 `-` 불릿 리스트를 사용한다.
  - 예시:
    ```
    fix(#0): 실서버 연동을 위한 안정성 수정

    - Write Queue: 단일 슬롯에서 deque 기반 다중 슬롯으로 변경하여 데이터 손실 방지
    - RemoteViewClient: URL 파서 trailing slash/path 처리 추가
    - BridgeSession: reportJoinOk/closeSession 비동기화 처리
    - BridgeServer: maxSessions config 값 전파
    ```
