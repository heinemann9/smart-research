# Smart Research 프로젝트 구축 일정

**Created:** 2026-03-10
**기준 문서:** `research-workflow-guide.md`

Claude Code + notebooklm-mcp-cli + Obsidian 리서치 & RAG 시스템 구축을 위한 단계별 실행 계획.

---

## 전체 요약

| Phase | 내용 | 예상 기간 |
|-------|------|----------|
| **1** | 인프라 설정 | 1일 | ✅ |
| **2** | 리서치 워크플로우 검증 | 2~3일 | ✅ |
| **3** | Obsidian 연동 패턴 구축 | 2일 | ⬜ |
| **4** | RAG 시스템 구축 | 2~3일 | ⬜ |
| **5** | 운영 안정화 | 1~2일 | ⬜ |
| | **합계** | **8~11일** | |

> Phase 2와 3은 병행 가능. 병행 시 **6~8일**로 단축 가능.

---

## Phase 1: 인프라 설정 (1일)

**목표**: 3개 도구 설치 및 연동 확인

| 단계 | 작업 | 완료 기준 | 상태 |
|------|------|----------|------|
| 1-1 | `notebooklm-mcp-cli` 프로젝트 로컬 설치 (`uv add`) | `uv run nlm --version` 정상 출력 | ✅ v0.4.4 |
| 1-2 | NotebookLM 인증 (`uv run nlm login`) | `uv run nlm login --check` 통과 | ✅ sinankng@gmail.com |
| 1-3 | Claude Code MCP 서버 연결 (`uv run nlm setup add claude-code`) | `uv run nlm doctor` 전체 통과 | ✅ 재시작 시 활성화 |
| 1-4 | Obsidian vault 디렉토리 생성 (`~/obsidian-vault/research/` 하위 8개 폴더) | Obsidian 앱에서 vault 열기 성공 | ✅ 생성 완료 |
| 1-5 | Obsidian 필수 플러그인 설치 (Dataview, Templater, Spaced Repetition) | 플러그인 활성화 확인 | ✅ 완료 |

---

## Phase 2: 리서치 워크플로우 검증 (2~3일)

**목표**: 4개 리서치 워크플로우를 각각 1회씩 실행하여 파이프라인 검증

| 단계 | 작업 | 대상 워크플로우 | 상태 |
|------|------|----------------|------|
| 2-1 | **주제 탐색** — 노트북 생성 → 소스 3개 + AI 리서치(43개 임포트) → 질의 → Study Guide/마인드맵/비교표 생성 → Obsidian 저장(MOC + 개념 스텁 11개) | WF1: Topic Exploration | ✅ |
| 2-2 | **논문 딥다이브** — RAG 원본 논문 PDF 업로드 → 구조화 질문 → 퀴즈/플래시카드/Briefing Doc 생성 → 다운로드 | WF2: Paper Deep Dive | ✅ |
| 2-3 | **비교 분석** — LangChain/LlamaIndex/Haystack 3개 URL 소스 → 비교표 + Briefing Doc 생성 → 다운로드 | WF3: Comparative Analysis | ✅ |
| 2-4 | **학습 최적화** — 기존 노트북(46개 소스) 활용 → 퀴즈/플래시카드 일괄 생성 → Obsidian SR 형식 변환(74장) | WF4: Study Optimization | ✅ |

**검증 결과**:
- CLI 직접 실행: 전체 파이프라인 정상 동작 확인
- Rate Limiting: 연속 질의 시 간헐적 `INVALID_ARGUMENT` 에러 발생 → 질의 간 딜레이 필요
- `nlm quiz create --difficulty` 옵션은 정수형 (가이드 문서의 `hard`/`medium` 문자열과 상이)
- `nlm data-table create`의 description은 positional argument (가이드 문서의 `--description` 플래그와 상이)
- `nlm download`는 `--id` 옵션으로 artifact ID 지정 (가이드 문서의 positional argument와 상이)

### 생성된 NotebookLM 노트북

| 노트북 | ID | 소스 수 |
|--------|-----|---------|
| RAG 시스템 동향 | `1018f8f3-236e-48a6-ae6c-491e3eac9bcd` | 46 |
| 논문: RAG for Knowledge-Intensive NLP | `1e89cd50-3909-4465-9170-1461f7e69d69` | 1 |
| RAG 프레임워크 비교 | `6d74c919-1eb9-49e3-9024-ae54e4bbad43` | 3 |

### 산출물 목록 (`output/`)

| 파일 | 워크플로우 | 유형 |
|------|-----------|------|
| `study-guide.md` | 2-1 | Study Guide |
| `mind-map.json` | 2-1 | Mind Map |
| `comparison.csv` | 2-1 | Data Table |
| `quiz.md` | 2-2 | Quiz |
| `flashcards.md` | 2-2 | Flashcards (JSON) |
| `briefing.md` | 2-2 | Briefing Doc |
| `framework-comparison.csv` | 2-3 | Data Table |
| `framework-briefing.md` | 2-3 | Briefing Doc |
| `rag-quiz.md` | 2-4 | Quiz |
| `rag-flashcards.md` | 2-4 | Flashcards (JSON) |

---

## Phase 3: Obsidian 연동 패턴 구축 (2일)

**목표**: NotebookLM 산출물 → Obsidian 변환 파이프라인 정립

| 단계 | 작업 | 대상 패턴 |
|------|------|----------|
| 3-1 | Study Guide → 프론트매터 + 위키링크 + 개념 스텁 노트 변환 | 패턴 A |
| 3-2 | Mind Map JSON → MOC(Map of Content) 마크다운 변환 | 패턴 B |
| 3-3 | Flashcards → Spaced Repetition 형식 변환 | 패턴 C |
| 3-4 | Data Table CSV → 개별 노트 + Dataview 쿼리 가능 프론트매터 | 패턴 D |
| 3-5 | 소스 원문 아카이브 노트 생성 | 패턴 E |
| 3-6 | Obsidian 템플릿 작성 (`templates/` 폴더) | 공통 |

---

## Phase 4: RAG 시스템 구축 (2~3일)

**목표**: 코딩 중 자동 질의되는 RAG 파이프라인 구축

| 단계 | 작업 | 대상 패턴 |
|------|------|----------|
| 4-1 | 실제 프로젝트 문서로 지식 베이스 노트북 구축 + 별칭 등록 | 패턴 A: 코딩 어시스턴트 RAG |
| 4-2 | 대상 프로젝트 CLAUDE.md에 Knowledge Base 섹션 + RAG 규칙 추가 | 패턴 A: 코딩 어시스턴트 RAG |
| 4-3 | 도메인별 멀티 노트북 분리 (인프라/보안/API 등) + CLAUDE.md 라우팅 규칙 | 패턴 B: 멀티 노트북 RAG |
| 4-4 | Obsidian 캐시 하이브리드 RAG 구현 (로컬 검색 우선 → NotebookLM 폴백 → 캐시 저장) | 패턴 C: 하이브리드 RAG |
| 4-5 | Chat 설정 최적화 (`nlm chat configure` — 용도별 응답 스타일) | 패턴 D: Chat 설정 최적화 |

---

## Phase 5: 운영 안정화 (1~2일)

**목표**: 대시보드, 모니터링, 제한사항 대응

| 단계 | 작업 |
|------|------|
| 5-1 | Dataview 대시보드 구축 (`dashboards/active-research.md` — 프로젝트 현황, 최근 소스, RAG 캐시 현황) |
| 5-2 | 멀티 프로필 설정 (개인/업무 Google 계정 분리) |
| 5-3 | Drive 소스 동기화 운영 절차 수립 (`nlm source stale` → `sync`) |
| 5-4 | 제한사항 대응 정리 (세션 만료 재로그인, Rate Limiting 딜레이, 노트북당 소스 제한 분리 전략) |
