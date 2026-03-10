# Claude Code + NotebookLM + Obsidian 워크플로우 가이드

**Last Updated:** 2026-03-10

Claude Code(AI 에이전트) + notebooklm-mcp-cli(NotebookLM MCP 서버 & CLI) + Obsidian(지식 관리)을 조합한 리서치 및 RAG 시스템 구축 가이드.

---

## 목차

1. [개요](#개요)
2. [초기 설정](#초기-설정)
3. [리서치 워크플로우](#리서치-워크플로우)
4. [RAG 워크플로우](#rag-워크플로우)
5. [Obsidian 연동 패턴](#obsidian-연동-패턴)
6. [MCP vs CLI 사용 가이드](#mcp-vs-cli-사용-가이드)
7. [팁과 제한사항](#팁과-제한사항)

---

## 개요

### 아키텍처

```
┌──────────────────────────────────────────────────────────┐
│                     Claude Code                          │
│                (오케스트레이터 & 분석가)                    │
│                                                          │
│  MCP 도구 호출 ──┐              ┌── 파일 읽기/쓰기        │
│  자연어 지시 ──┐  │              │                        │
└────────────────┼──┼──────────────┼────────────────────────┘
                 │  │              │
                 ▼  ▼              ▼
┌────────────────────────┐  ┌──────────────────────────────┐
│  notebooklm-mcp-cli    │  │         Obsidian              │
│                        │  │     (지식 그래프 저장소)        │
│  MCP 서버 (29개 도구)   │  │                              │
│  + nlm CLI             │  │  - 마크다운 영구 노트          │
│                        │  │  - 위키링크 지식 구조화         │
│  - 노트북/소스 관리     │  │  - 태그, 검색, 그래프 뷰       │
│  - AI 질의 (RAG)       │  │  - Dataview 대시보드           │
│  - 웹/Drive 리서치      │  │  - Spaced Repetition          │
│  - 아티팩트 생성/다운로드│  │                              │
│  - 공유 관리            │  │                              │
└────────────────────────┘  └──────────────────────────────┘
        ↕ 내부 API                    ↕ 파일시스템
┌────────────────────────┐
│   Google NotebookLM    │
│   (Gemini 기반 RAG)    │
└────────────────────────┘
```

### 두 가지 활용 모드

| 모드 | 목적 | 패턴 |
|------|------|------|
| **리서치** | 주제 탐구, 논문 분석, 비교 연구 | 소스 수집 → AI 분석 → 산출물 생성 → Obsidian 저장 |
| **RAG** | 코딩 중 문서 참조, 도메인 지식 조회 | CLAUDE.md에 노트북 등록 → 작업 중 자동 질의 |

---

## 초기 설정

### 1. notebooklm-mcp-cli 설치

```bash
# uv 사용 (권장)
uv tool install notebooklm-mcp-cli

# 또는 pip
pip install notebooklm-mcp-cli

# 또는 pipx
pipx install notebooklm-mcp-cli
```

### 2. NotebookLM 인증

```bash
# 브라우저에서 Google 계정 로그인
nlm login

# 인증 확인
nlm login --check

# 노트북 목록으로 동작 확인
nlm notebook list
```

> 여러 Google 계정을 사용할 경우:
> ```bash
> nlm login --profile work
> nlm login --profile personal
> nlm login switch work
> nlm login profile list
> ```

### 3. Claude Code에 MCP 서버 연결

```bash
# 자동 설정 (권장)
nlm setup add claude-code

# 설정 확인
nlm setup list

# 문제 진단
nlm doctor
```

수동 설정이 필요한 경우:
```bash
claude mcp add notebooklm-mcp notebooklm-mcp
```

### 4. 노트북 별칭 설정 (선택)

자주 쓰는 노트북에 별칭을 지정하면 편리합니다:

```bash
nlm alias set research <notebook_id>
nlm alias set api-docs <notebook_id>
nlm alias list
```

### 5. Obsidian Vault 생성

```bash
mkdir -p ~/obsidian-vault/research/{inbox,projects,sources,concepts,artifacts,templates,dashboards}
```

Obsidian 앱에서 `~/obsidian-vault/research`를 vault로 열기.

#### 권장 Obsidian 플러그인

| 플러그인 | 용도 |
|---------|------|
| **Dataview** | 리서치 대시보드, 메타데이터 쿼리 |
| **Templater** | 노트 템플릿 자동화 |
| **Spaced Repetition** | 플래시카드 기반 복습 |
| **Excalidraw** | 마인드맵 시각화 |
| **Kanban** | 리서치 진행 관리 |

---

## 리서치 워크플로우

### 워크플로우 1: 주제 탐색 (Topic Exploration)

새로운 주제를 처음 리서치할 때.

```
[1] 주제 정의 → [2] 소스 수집 → [3] AI 분석 → [4] 산출물 생성 → [5] Obsidian 저장
```

#### Step 1: 노트북 생성 및 초기 소스 투입

CLI:
```bash
# 노트북 생성
nlm notebook create "양자컴퓨팅과 암호화의 미래"

# 소스 추가 (URL, 파일, YouTube)
nlm source add <notebook_id> --url "https://en.wikipedia.org/wiki/Post-quantum_cryptography"
nlm source add <notebook_id> --file ./papers/quantum-threat-2025.pdf --wait
nlm source add <notebook_id> --youtube "https://www.youtube.com/watch?v=VIDEO_ID"
```

MCP (Claude Code에서 자연어):
> "양자컴퓨팅과 암호화의 미래에 대한 NotebookLM 노트북을 만들고, 이 URL들을 소스로 추가해줘: ..."

Claude Code가 MCP 도구를 자동 호출:
- `notebook_create` → 노트북 생성
- `source_add` → 소스 추가 (source_type: url/file/youtube)

#### Step 2: AI 리서치로 소스 확장

```bash
# 빠른 웹 리서치
nlm research start "post-quantum cryptography NIST 2025" --notebook-id <id> --mode fast

# 깊은 리서치 (2-5분 소요)
nlm research start "quantum computing RSA threat" --notebook-id <id> --mode deep

# 리서치 완료 대기
nlm research status <notebook_id> --max-wait 300

# 발견된 소스 임포트
nlm research import <notebook_id> <task_id>
```

MCP: `research_start` → `research_status` → `research_import`

#### Step 3: 다각도 분석

```bash
# AI 요약
nlm notebook describe <notebook_id>

# 핵심 질문
nlm notebook query <notebook_id> "각 소스에서 공통으로 언급하는 가장 시급한 위협은?"
nlm notebook query <notebook_id> "NIST PQC 표준화 현황을 시간순으로 정리해줘"
nlm notebook query <notebook_id> "양자컴퓨팅이 현행 암호체계에 미치는 영향을 비교해줘"
```

MCP: `notebook_query` 도구로 자동 호출. Claude Code가 답변을 분석하고 후속 질문을 자동 생성.

#### Step 4: 산출물 생성 및 다운로드

```bash
# 리포트 생성
nlm report create <notebook_id> --format "Study Guide" --confirm

# 마인드맵 (즉시 생성)
nlm mindmap create <notebook_id> --confirm

# 비교표
nlm data-table create <notebook_id> --description "양자 알고리즘별 위협 수준과 대응 방안 비교" --confirm

# 생성 상태 확인
nlm studio status <notebook_id>

# 다운로드
nlm download report <notebook_id> <artifact_id> --output ./output/study-guide.md
nlm download mind-map <notebook_id> <artifact_id> --output ./output/mind-map.json
nlm download data-table <notebook_id> <artifact_id> --output ./output/comparison.csv
```

MCP: `studio_create` → `studio_status` → `download_artifact`

#### Step 5: Obsidian에 구조화 저장

Claude Code에게 요청:
> "output/ 폴더의 study-guide.md, mind-map.json을 Obsidian vault에 맞게 변환해서
> ~/obsidian-vault/research/projects/quantum-security/ 에 저장해줘.
> 프론트매터, 위키링크, 개념 스텁 노트도 만들어줘."

(상세한 Obsidian 변환 패턴은 [아래 섹션](#obsidian-연동-패턴) 참고)

---

### 워크플로우 2: 논문 딥다이브 (Paper Deep Dive)

```bash
# 1. 노트북 + 논문 업로드
nlm notebook create "논문: Attention Is All You Need"
nlm source add <id> --file ./papers/attention.pdf --wait

# 2. 소스 분석
nlm source describe <source_id>

# 3. 구조화된 질문
nlm notebook query <id> "이 논문의 핵심 기여(contribution)를 3가지로 요약해줘"
nlm notebook query <id> "Transformer 아키텍처의 각 구성 요소를 설명해줘"
nlm notebook query <id> "이전 연구(RNN, LSTM)와 비교했을 때 장단점은?"
nlm notebook query <id> "실험 결과에서 가장 주목할 만한 수치는?"

# 4. 학습 자료 생성
nlm quiz create <id> --difficulty hard --focus "핵심 개념에 집중" --confirm
nlm flashcards create <id> --difficulty medium --confirm
nlm report create <id> --format "Briefing Doc" --confirm

# 5. 다운로드
nlm download quiz <id> <artifact_id> --format markdown --output ./output/quiz.md
nlm download flashcards <id> <artifact_id> --format markdown --output ./output/flashcards.md
nlm download report <id> <artifact_id> --output ./output/briefing.md
```

---

### 워크플로우 3: 비교 분석 (Comparative Analysis)

```bash
# 1. 비교 대상 소스 추가
nlm notebook create "AI 규제: 미국 vs EU vs 한국"
nlm source add <id> --url "https://...us-ai-executive-order" --wait
nlm source add <id> --url "https://...eu-ai-act" --wait
nlm source add <id> --url "https://...korea-ai-framework" --wait

# 2. 교차 분석 질문
nlm notebook query <id> "세 지역의 AI 규제 접근 방식의 핵심 차이점을 비교해줘"
nlm notebook query <id> "공통적으로 규제하는 고위험 AI 카테고리는?"

# 3. 비교표 + 토론 팟캐스트
nlm data-table create <id> --description "미국, EU, 한국 AI 규제 비교: 범위, 집행기관, 벌칙, 시행시기" --confirm
nlm audio create <id> --format debate --confirm

# 4. 다운로드
nlm download data-table <id> <artifact_id> --output ./output/regulation-comparison.csv
nlm download audio <id> <artifact_id> --output ./output/regulation-debate.mp3
```

---

### 워크플로우 4: 학습 최적화 (Study Optimization)

```bash
# 1. 강의 자료 일괄 업로드
nlm notebook create "머신러닝 기말고사 준비"
for f in ./lecture-notes/*.pdf; do
  nlm source add <id> --file "$f" --wait
done

# 2. 전체 요약
nlm notebook describe <id>
nlm notebook query <id> "전체 강의에서 가장 중요한 개념 10가지를 뽑아줘"

# 3. 학습 자료 일괄 생성
nlm report create <id> --format "Study Guide" --confirm
nlm quiz create <id> --difficulty medium --count 20 --confirm
nlm flashcards create <id> --difficulty medium --confirm
nlm mindmap create <id> --confirm

# 4. 다운로드 → Obsidian
nlm download report <id> <artifact_id> --output ./output/study-guide.md
nlm download quiz <id> <artifact_id> --format markdown --output ./output/quiz.md
nlm download flashcards <id> <artifact_id> --format markdown --output ./output/flashcards.md
nlm download mind-map <id> <artifact_id> --output ./output/concepts.json
```

Claude Code에게:
> "flashcards.md를 Obsidian Spaced Repetition 형식으로 변환해줘"

---

## RAG 워크플로우

NotebookLM을 벡터DB 없이 사용하는 관리형 RAG 시스템으로 활용.

### 패턴 A: 코딩 어시스턴트 RAG

프로젝트 문서를 NotebookLM에 올려두고 코딩 중 자동 참조.

#### 1. 지식 베이스 구축

```bash
# API 문서 노트북
nlm notebook create "프로젝트 X - API 문서"
nlm source add <id> --url "https://docs.library.com/api" --wait
nlm source add <id> --file ./internal-api-spec.pdf --wait
nlm source add <id> --file ./architecture-decisions.md --wait
nlm alias set project-x-docs <id>
```

#### 2. CLAUDE.md에 RAG 지시 추가

프로젝트의 `CLAUDE.md`에 다음을 추가:

```markdown
## Knowledge Base (NotebookLM RAG)

이 프로젝트는 NotebookLM을 지식 베이스로 사용합니다.
notebooklm-mcp-cli MCP 서버가 연결되어 있습니다.

- 노트북: "프로젝트 X - API 문서" (ID: <notebook_id>)
- 포함 문서: API 스펙, 아키텍처 결정 문서, 내부 위키

### RAG 규칙
1. 이 프로젝트의 API나 아키텍처에 대해 불확실할 때,
   코드를 작성하기 전에 notebook_query로 NotebookLM에 질의하세요.
2. NotebookLM이 "소스에 정보가 없다"고 하면, 추측하지 말고 사용자에게 물어보세요.
3. 유용한 답변은 ~/obsidian-vault/research/cache/ 에 캐싱하세요.
```

#### 3. 작동 방식

Claude Code가 코드를 작성하다가 API 사용법이 불확실하면:

```
Claude Code: "이 API의 인증 방식이 뭐지?"
    ↓ MCP 도구 자동 호출
notebook_query(notebook_id, "이 API의 인증 방식을 설명해줘")
    ↓ NotebookLM이 업로드된 문서에서 검색
    ↓ Gemini가 소스 기반 답변 + 인용 반환
Claude Code: 답변을 기반으로 정확한 코드 작성
```

### 패턴 B: 도메인별 멀티 노트북 RAG

```bash
# 도메인별 노트북 구축
nlm notebook create "인프라 문서"     # k8s, terraform, aws
nlm notebook create "보안 가이드"     # owasp, compliance
nlm notebook create "사내 API 레퍼런스"  # internal apis

# 별칭 등록
nlm alias set infra <id1>
nlm alias set security <id2>
nlm alias set internal-api <id3>
```

CLAUDE.md에 여러 노트북을 등록하면, Claude Code가 질문 성격에 따라 적절한 노트북을 선택합니다:

```markdown
## Knowledge Bases

| 노트북 | ID | 용도 |
|--------|-----|------|
| 인프라 문서 | <id1> | k8s, terraform, AWS 관련 |
| 보안 가이드 | <id2> | 보안 정책, OWASP, 컴플라이언스 |
| 사내 API | <id3> | 내부 API 스펙, 엔드포인트 |

인프라 관련 → id1에 질의, 보안 관련 → id2에 질의.
```

### 패턴 C: Obsidian 캐시 하이브리드 RAG

```
질문 발생
    ↓
1차: Obsidian vault에서 로컬 검색 (Grep/Read)
    ↓ 답변 있음? → 바로 사용 (빠름, API 호출 없음)
    ↓ 없으면?
2차: NotebookLM에 질의 (MCP notebook_query)
    ↓
답변을 Obsidian cache 폴더에 저장 (다음에 재사용)
```

CLAUDE.md 지시:

```markdown
### RAG 캐시 규칙
1. 질문 전에 먼저 ~/obsidian-vault/research/cache/ 에서 관련 파일 검색
2. 캐시에 없으면 NotebookLM에 질의
3. 질의 결과를 cache/ 에 마크다운으로 저장:
   - 파일명: `{주제}-{날짜}.md`
   - 프론트매터: notebook_id, query, created
```

### 패턴 D: Chat 설정으로 RAG 최적화

```bash
# 간결한 코드 참조 모드
nlm chat configure <notebook_id> --goal default --length shorter

# 학습 가이드 모드
nlm chat configure <notebook_id> --goal learning_guide --length longer

# 커스텀 페르소나
nlm chat configure <notebook_id> --goal custom \
  --prompt "당신은 시니어 백엔드 엔지니어입니다. API 사용법을 코드 예제와 함께 간결하게 설명하세요."
```

---

## Obsidian 연동 패턴

### 패턴 A: Study Guide → Obsidian 노트

Claude Code에게 요청:
> "output/study-guide.md를 Obsidian 형식으로 변환해줘.
> 프론트매터, 위키링크, 개념 스텁 노트 추가."

변환 결과:

```markdown
---
type: study-guide
source: notebooklm
notebook_id: "abc123"
created: 2026-03-07
tags: [study-guide, quantum-computing, security]
---

# 양자컴퓨팅 보안 위협 Study Guide

## 1. [[양자 우위 (Quantum Supremacy)]]
현재 고전 컴퓨터로 풀 수 없는 문제를 양자 컴퓨터가 풀 수 있는 상태...

## 2. [[Shor 알고리즘]]
RSA, ECC 등 현행 공개키 암호를 다항시간에 파괴할 수 있는 양자 알고리즘...

## 3. [[포스트양자암호 (PQC)]]
양자 컴퓨터에도 안전한 새로운 암호체계. [[NIST]]가 2024년 표준 발표...
```

### 패턴 B: Mind Map → Obsidian MOC

```markdown
---
type: moc
created: 2026-03-07
tags: [moc, quantum-computing]
---

# 양자컴퓨팅 보안 MOC

## 핵심 개념
- [[양자 우위]]
  - [[구글 시커모어]]
  - [[IBM 양자 로드맵]]

## 위협 분석
- [[현행 암호체계 취약점]]
  - [[RSA 취약성]]
  - [[ECC 취약성]]

## 대응 방안
- [[포스트양자암호]]
  - [[격자 기반 암호]]
  - [[NIST PQC 표준화]]
```

### 패턴 C: Flashcards → Spaced Repetition

Obsidian Spaced Repetition 플러그인 형식:

```markdown
---
type: flashcards
source: notebooklm
tags: [flashcard, quantum-computing]
---

# 양자컴퓨팅 플래시카드

Shor 알고리즘이 파괴할 수 있는 암호체계는?
?
RSA, ECC, DSA 등 정수 인수분해와 이산로그 기반 공개키 암호체계

---

NIST PQC 표준으로 선정된 알고리즘 4가지는?
?
CRYSTALS-Kyber(KEM), CRYSTALS-Dilithium(서명), FALCON(서명), SPHINCS+(서명)
```

### 패턴 D: Data Table → Dataview 연동

CSV 다운로드 후 Claude Code에게:
> "이 CSV의 각 행을 개별 Obsidian 노트로 변환해줘.
> Dataview에서 쿼리할 수 있도록 프론트매터에 메타데이터를 넣어줘."

Dataview 쿼리 예시:

```
```dataview
TABLE threat_level, algorithm, timeline
FROM #quantum-threat
SORT threat_level DESC
```
```

### 패턴 E: 소스 원문 아카이브

```bash
# 소스 원문 텍스트 추출
nlm source get <source_id>               # 소스 메타데이터
nlm source describe <source_id>          # AI 요약 + 키워드
```

Claude Code에게:
> "이 소스의 요약과 키워드를 Obsidian source 노트로 저장해줘"

---

## MCP vs CLI 사용 가이드

### 언제 MCP (Claude Code 자동)

- Claude Code에서 자연어로 지시할 때
- RAG 패턴으로 코딩 중 자동 질의할 때
- 복잡한 멀티스텝 파이프라인을 자동화할 때

```
사용자: "이 논문을 NotebookLM에 올리고 핵심 요약해줘"
Claude Code → notebook_create → source_add → notebook_query
```

### 언제 CLI (직접 실행)

- 빠른 일회성 작업
- 셸 스크립트 자동화
- 배치 처리
- 디버깅/상태 확인

```bash
# 빠른 상태 확인
nlm notebook list
nlm studio status <id>
nlm doctor

# 배치 소스 추가
for url in $(cat urls.txt); do
  nlm source add <notebook_id> --url "$url" --wait
done
```

### MCP 도구 매핑 (주요)

| 작업 | MCP 도구 | CLI 명령 |
|------|----------|----------|
| 노트북 생성 | `notebook_create` | `nlm notebook create` |
| 소스 추가 | `source_add` | `nlm source add` |
| AI 질의 | `notebook_query` | `nlm notebook query` |
| 리서치 시작 | `research_start` | `nlm research start` |
| 아티팩트 생성 | `studio_create` | `nlm audio/video/report create` |
| 다운로드 | `download_artifact` | `nlm download` |
| 공유 | `notebook_share_public` | `nlm share public` |
| 인증 확인 | `server_info` | `nlm login --check` |

---

## 팁과 제한사항

### 효과적인 사용 팁

1. **별칭 활용**: 자주 쓰는 노트북에 `nlm alias set` 등록.

2. **`--json` 플래그**: 스크립트에서 결과 파싱에 유용.
   ```bash
   nlm notebook list --json
   ```

3. **`--wait` 플래그**: 소스 추가 시 인덱싱 완료까지 대기.
   ```bash
   nlm source add <id> --file doc.pdf --wait
   ```

4. **Chat 설정 활용**: 용도에 맞게 응답 스타일을 조정.
   ```bash
   nlm chat configure <id> --goal custom --prompt "간결하게 코드 예제 위주로"
   ```

5. **Drive 소스 동기화**: Google Drive 문서가 변경되면 자동 갱신.
   ```bash
   nlm source stale <notebook_id>     # 변경된 소스 확인
   nlm source sync <notebook_id> --confirm  # 동기화
   ```

6. **멀티 프로필**: 개인/업무 계정 분리.
   ```bash
   nlm login --profile personal
   nlm login --profile work
   nlm login switch work
   ```

7. **진단**: 문제 발생 시 `nlm doctor`로 자동 진단.

### 제한사항

| 제한 | 설명 | 우회 방법 |
|------|------|----------|
| **비공식 API** | Google이 언제든 변경 가능 | `uv tool upgrade notebooklm-mcp-cli` |
| **세션 만료** | 쿠키 만료 시 재로그인 | `nlm login` 재실행 |
| **Rate Limiting** | 생성 작업에 Google 제한 | 작업 간 딜레이, 시간 두고 재시도 |
| **소스 제한** | 플랜별 상이 (50~600개) | 주제별 노트북 분리 |
| **생성 시간** | 오디오 10-20분, 비디오 15-45분 | `studio_status`로 폴링 |
| **MCP 29개 도구** | 컨텍스트 윈도우 소모 | 미사용 시 MCP 비활성화 |
| **Obsidian 연동** | 파일시스템 기반 (API 없음) | Claude Code가 직접 파일 생성 |

### 문제 해결

```bash
# 종합 진단
nlm doctor --verbose

# 인증 확인
nlm login --check

# MCP 설정 확인
nlm setup list

# 설정 보기
nlm config show
```

---

## 추천 디렉토리 구조

```
~/obsidian-vault/research/
├── inbox/                    # 미분류 노트
├── projects/                 # 리서치 프로젝트별 폴더
│   ├── quantum-security/
│   │   ├── _index.md         # MOC (Map of Content)
│   │   ├── study-guide.md
│   │   ├── mind-map.md
│   │   └── q-and-a.md
│   └── ai-regulation/
│       └── ...
├── sources/                  # 소스 원문/요약 아카이브
├── concepts/                 # 개념 노트 (위키링크 대상)
├── artifacts/                # NotebookLM 산출물 원본
│   ├── podcasts/
│   ├── reports/
│   └── data-tables/
├── cache/                    # RAG 캐시 (자동 생성)
├── templates/                # Obsidian 템플릿
└── dashboards/               # Dataview 대시보드
```

### Dataview 대시보드 예시

`dashboards/active-research.md`:

```markdown
# Active Research Dashboard

## 진행 중인 프로젝트

```dataview
TABLE status, created, tags
FROM "projects"
WHERE type = "research-project" AND status = "active"
SORT created DESC
```

## 최근 추가된 소스

```dataview
TABLE source_type, created
FROM "sources"
SORT created DESC
LIMIT 10
```

## RAG 캐시 (최근 질의)

```dataview
TABLE query, notebook_id, created
FROM "cache"
SORT created DESC
LIMIT 10
```
```

---

## 빠른 시작 체크리스트

- [ ] `uv tool install notebooklm-mcp-cli`
- [ ] `nlm login` → Google 계정 인증
- [ ] `nlm login --check` → 인증 확인
- [ ] `nlm setup add claude-code` → MCP 서버 연결
- [ ] `nlm doctor` → 전체 진단 통과
- [ ] Obsidian vault 생성 (`~/obsidian-vault/research/`)
- [ ] 첫 리서치 사이클:
  1. `nlm notebook create "테스트 리서치"`
  2. `nlm source add <id> --url "https://en.wikipedia.org/wiki/..." --wait`
  3. `nlm notebook query <id> "핵심 요약해줘"`
  4. `nlm report create <id> --format "Study Guide" --confirm`
  5. `nlm download report <id> <artifact_id> --output ./output/test.md`
  6. Claude Code에게 "이 파일을 Obsidian 형식으로 변환해줘" 요청
- [ ] 첫 RAG 테스트:
  1. CLAUDE.md에 Knowledge Base 섹션 추가
  2. Claude Code에게 "NotebookLM에서 [주제]에 대해 확인해줘" 요청
  3. 자동으로 `notebook_query` MCP 호출 확인
