# Smart Research

Claude Code + NotebookLM + Obsidian을 조합한 리서치 및 RAG 시스템.

## 아키텍처

```
┌─────────────────────────────────────────────┐
│              Claude Code                     │
│          (오케스트레이터 & 분석가)              │
│                                             │
│  MCP 도구 호출 ──┐        ┌── 파일 읽기/쓰기  │
└──────────────────┼────────┼─────────────────┘
                   │        │
                   ▼        ▼
┌──────────────────────┐  ┌─────────────────────┐
│  notebooklm-mcp-cli  │  │      Obsidian        │
│                      │  │  (지식 그래프 저장소)  │
│  - 노트북/소스 관리   │  │                      │
│  - AI 질의 (RAG)     │  │  - 마크다운 영구 노트  │
│  - 웹/Drive 리서치    │  │  - 위키링크 구조화     │
│  - 아티팩트 생성      │  │  - Dataview 대시보드   │
└──────────┬───────────┘  └──────────────────────┘
           │
           ▼
┌──────────────────────┐
│  Google NotebookLM   │
│  (Gemini 기반 RAG)   │
└──────────────────────┘
```

## 주요 기능

### 리서치 모드
소스 수집 → AI 분석 → 산출물 생성 → Obsidian 저장

- **주제 탐색**: 웹 리서치로 소스 자동 수집 → Study Guide, 마인드맵, 비교표 생성
- **논문 딥다이브**: PDF 업로드 → 구조화 질문 → 퀴즈/플래시카드/Briefing Doc
- **비교 분석**: 복수 소스 교차 분석 → 비교표 + 토론 오디오
- **학습 최적화**: 자료 일괄 업로드 → 학습 자료 일괄 생성 → Obsidian Spaced Repetition

### RAG 모드
프로젝트 CLAUDE.md에 NotebookLM 노트북 등록 → 코딩 중 MCP `notebook_query`로 자동 질의

## 시작하기

### 사전 요구사항

- Python 3.11+
- [uv](https://docs.astral.sh/uv/)
- Google 계정 (NotebookLM 접근용)
- [Obsidian](https://obsidian.md/)

### 설치

```bash
# 저장소 클론
git clone https://github.com/<your-username>/smart-research.git
cd smart-research

# 의존성 설치 (프로젝트 로컬 가상환경)
uv sync

# NotebookLM 인증
uv run nlm login

# 인증 확인
uv run nlm login --check

# Claude Code MCP 서버 연결
uv run nlm setup add claude-code

# 진단
uv run nlm doctor
```

### Obsidian vault 설정

```bash
# vault 디렉토리 생성
mkdir -p ~/obsidian-vault/research/{inbox,projects,sources,concepts,artifacts,cache,templates,dashboards}
```

Obsidian 앱에서 `~/obsidian-vault/research`를 vault로 열고, Community plugins에서 **Dataview**, **Templater**, **Spaced Repetition** 설치.

## 프로젝트 구조

```
smart-research/
├── CLAUDE.md                          # Claude Code 가이드
├── docs/
│   ├── research-workflow-guide.md     # 워크플로우 상세 가이드
│   └── project-plan.md               # 구축 일정 및 진행 상황
├── output/                            # NotebookLM 산출물 원본
├── papers/                            # 리서치 논문 PDF
└── pyproject.toml
```

## 문서

- **[워크플로우 가이드](docs/research-workflow-guide.md)** — 전체 워크플로우 상세 설명, CLI/MCP 사용법, Obsidian 연동 패턴
- **[프로젝트 계획](docs/project-plan.md)** — 5단계 구축 일정 및 진행 현황

## 라이선스

MIT
