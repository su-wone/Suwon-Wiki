# WIKI.md — LLM 위키 스키마

> claude-obsidian 플러그인을 사용 중이라면, 여기 모든 것을 스킬이 자동으로 처리합니다.
> 이 파일은 레퍼런스 문서입니다. 시스템이 어떻게 동작하는지 이해하려면 읽으세요.
> Andrej Karpathy의 LLM 위키 패턴 기반.

---

## 이것이 무엇인가

당신은 Obsidian 볼트 안의 영속적이고 복리로 쌓이는 위키를 유지하고 있습니다. 단순히 질문에 답하지 않습니다. 매 소스가 추가될 때마다, 매 질문이 던져질 때마다 더 풍부해지는 구조화된 지식 베이스를 만들고 유지합니다. 사람이 소스를 큐레이팅하고 질문합니다. 당신은 모든 작성, 상호 참조, 정리, 유지를 합니다.

위키가 결과물입니다. 채팅은 그저 인터페이스입니다.

RAG와의 핵심 차이: 위키는 영속적인 산물입니다. 상호 참조가 이미 있습니다. 모순은 표시되었습니다. 종합은 이미 읽힌 모든 것을 반영합니다. 지식이 이자처럼 복리로 쌓입니다.

---

## 0 — 부트스트랩: 첫 실행 셋업

새 프로젝트에서 처음 실행할 때 이 단계를 순서대로 실행합니다. 이미 완료된 단계는 건너뛰세요.

### 0.1 Obsidian 설치 확인

```bash
# Linux: check flatpak first, then PATH
flatpak list 2>/dev/null | grep -i obsidian && echo "FOUND via flatpak" || \
which obsidian 2>/dev/null && echo "FOUND in PATH" || echo "NOT FOUND"

# macOS
ls /Applications/Obsidian.app 2>/dev/null && echo "FOUND" || echo "NOT FOUND"

# Windows (PowerShell)
Test-Path "$env:LOCALAPPDATA\Obsidian" && echo "FOUND" || echo "NOT FOUND"
```

설치되어 있지 않다면:

```bash
# Linux (Flatpak)
flatpak install flathub md.obsidian.Obsidian

# macOS (Homebrew)
brew install --cask obsidian

# Windows (winget)
winget install Obsidian.Obsidian

# All platforms: https://obsidian.md/download
```

설치 후: Obsidian > Manage Vaults > Open Folder as Vault > 볼트 디렉터리 선택.

패키지 매니저가 없다면 사용자에게 알리세요: "https://obsidian.md에서 Obsidian을 다운로드하세요. 설치하고, 볼트를 만들고, 경로를 알려주세요."

### 0.2 볼트 위치

볼트 경로를 묻거나 기본값을 사용:

```
VAULT_PATH=~/Documents/Obsidian Vault
```

확인: `ls "$VAULT_PATH/.obsidian" 2>/dev/null`

### 0.3 Local REST API 플러그인 설치

사용자에게 안내합니다 (프로그래밍 방식으로는 할 수 없음):

1. Obsidian > Settings > Community Plugins > Restricted Mode 끄기
2. Browse > "Local REST API" 검색 > Install > Enable
3. Settings > Local REST API > API 키 복사
4. 플러그인은 `https://127.0.0.1:27124`에서 실행됨 (자체 서명 인증서)

테스트: `curl -sk -H "Authorization: Bearer <KEY>" https://127.0.0.1:27124/`

### 0.4 MCP 서버 구성

**옵션 A: mcp-obsidian (REST API 기반, 가장 인기)**

```bash
claude mcp add-json obsidian-vault '{
  "type": "stdio",
  "command": "uvx",
  "args": ["mcp-obsidian"],
  "env": {
    "OBSIDIAN_API_KEY": "<KEY>",
    "OBSIDIAN_HOST": "127.0.0.1",
    "OBSIDIAN_PORT": "27124",
    "NODE_TLS_REJECT_UNAUTHORIZED": "0"
  }
}' --scope user
```

**옵션 B: MCPVault (파일시스템 기반, 플러그인 불필요)**

```bash
claude mcp add-json obsidian-vault '{
  "type": "stdio",
  "command": "npx",
  "args": ["-y", "@bitbonsai/mcpvault@latest", "<VAULT_PATH>"]
}' --scope user
```

**옵션 C: curl을 통한 직접 REST API**: 항상 동작, MCP 불필요. Section 11 참조.

볼트가 모든 프로젝트에서 사용 가능하도록 `--scope user` 사용.

**확인:**

```bash
claude mcp list               # confirm server appears
claude mcp get obsidian-vault # confirm path is correct
```

Claude Code 세션에서 `/mcp`를 입력해 연결 상태 확인.

### 0.5 권장 플러그인

Settings > Community Plugins > Browse를 통해 설치:

| 플러그인              | 이유                         |
| ----------------- | -------------------------- |
| **Dataview**      | 볼트를 데이터베이스로 쿼리. 대시보드의 동력.  |
| **Templater**     | 노트 생성 시 frontmatter 자동 채움. |
| **Obsidian Git**  | 15분마다 자동 커밋. 데이터 손실 방지.    |
| **Iconize**       | 시각적 폴더 아이콘.                |
| **Minimal Theme** | 밀도 높은 정보 표시에 가장 좋은 다크 테마.  |

선택: Smart Connections (시맨틱 검색), QuickAdd (매크로), Folder Notes (클릭 가능 폴더).

또한 **Obsidian Web Clipper** 브라우저 확장을 설치하세요. 웹 기사를 마크다운으로 변환해 한 번의 클릭으로 `.raw/`에 보냅니다. Chrome, Firefox, Safari용 제공.

---

## 1 — 아키텍처

```
vault/
├── .raw/                   # Layer 1: immutable source documents
│   ├── articles/
│   ├── transcripts/
│   ├── screenshots/
│   ├── data/
│   └── assets/
│
├── wiki/                   # Layer 2: LLM-generated knowledge base
│   ├── index.md            # master catalog of all wiki pages
│   ├── log.md              # chronological record of all operations
│   ├── hot.md              # hot cache: recent context summary (~500 words)
│   ├── overview.md         # executive summary of the entire wiki
│   ├── sources/            # one summary page per raw source
│   ├── entities/           # people, orgs, products, repos
│   │   └── _index.md
│   ├── concepts/           # ideas, patterns, frameworks
│   │   └── _index.md
│   ├── domains/            # top-level topic areas
│   │   └── _index.md
│   ├── comparisons/        # side-by-side analyses
│   ├── questions/          # filed answers to user queries
│   └── meta/               # dashboards, lint reports, conventions
│
├── _templates/             # Templater templates
├── _attachments/           # images and PDFs referenced by wiki pages
│
├── WIKI.md                 # Layer 3: this file
└── .obsidian/              # Obsidian config (auto-managed)
```

### 규칙

- `.raw/`는 읽기 전용. 절대 소스 파일을 수정하지 말 것.
- `wiki/`는 당신의 것. 자유롭게 만들고, 갱신하고, 이름 바꾸고, 삭제.
- 모든 위키 페이지는 frontmatter를 가짐. 예외 없음.
- 경로보다 위키링크. `[text](path/to/file.md)` 대신 `[[Page Name]]` 사용.
- 원자적 노트. 페이지당 한 개념. 두 가지를 다룬다면 분리.
- 갱신, 복제 금지. 페이지가 존재하면 갱신.

---

## 2 — 핫 캐시

`wiki/hot.md`는 가장 최근 컨텍스트의 약 500단어 요약입니다. 다른 프로젝트가 이 볼트를 가리킬 때 전체 위키를 뒤지지 않고도 최근 컨텍스트를 얻을 수 있도록 존재합니다.

매 인제스트 후, 의미 있는 쿼리 교환 후, 매 세션 종료 시 hot.md를 갱신.

형식:

```markdown
---
type: meta
title: "Hot Cache"
updated: 2026-04-07T14:30:00
---

# Recent Context

## Last Updated
2026-04-07 — Ingested 3 new YouTube transcripts

## Key Recent Facts
- [Most important recent takeaway]
- [Second most important]

## Recent Changes
- Created: [[New Page 1]], [[New Page 2]]
- Updated: [[Existing Page]] (added section on X)
- Flagged: Contradiction between [[Page A]] and [[Page B]] on topic Y

## Active Threads
- User is currently researching [topic]
- Open question: [thing still being investigated]
```

500단어 이하로 유지. 일지가 아니라 캐시. 매번 완전히 덮어쓰기.

---

## 3 — Frontmatter 스키마

모든 위키 페이지는 평면 YAML frontmatter로 시작. 중첩 객체 없음. Obsidian의 Properties UI가 지원하지 않음.

### 보편 필드 (모든 페이지):

```yaml
---
type: <source|entity|concept|domain|comparison|question|overview|meta>
title: "Human-Readable Title"
created: 2026-04-07
updated: 2026-04-07
tags:
  - <domain-tag>
  - <type-tag>
status: <seed|developing|mature|evergreen>
related:
  - "[[Other Page]]"
sources:
  - "[[.raw/articles/source-file.md]]"
---
```

### 타입별 추가:

**source**: `source_type`, `author`, `date_published`, `url`, `confidence` (high|medium|low), `key_claims` (목록)

**entity**: `entity_type` (person|organization|product|repository|place), `role`, `first_mentioned`

**concept**: `complexity` (basic|intermediate|advanced), `domain`, `aliases` (목록)

**comparison**: `subjects` (위키링크 목록), `dimensions` (목록), `verdict` (한 줄)

**question**: `question` (원래 쿼리), `answer_quality` (draft|solid|definitive)

---

## 4 — 작업

### 4.1 SCAFFOLD — 첫 실행 구조

트리거: 사용자가 볼트의 용도를 설명.

1. 위키 모드를 결정합니다 (아래 모드 표 및 4.1a의 전체 모드 상세 참조).
2. 한 가지 질문: "이 볼트는 무엇을 위한 것입니까?"
3. `wiki/` 아래 전체 폴더 구조 생성.
4. 도메인별로 도메인 페이지와 `_index.md` 서브 인덱스 생성.
5. `wiki/overview.md`, `wiki/index.md`, `wiki/log.md`, `wiki/hot.md` 생성.
6. `_templates/`에 노트 타입별 템플릿 생성.
7. 시각적 커스터마이징 적용 (Section 7). `.obsidian/snippets/vault-colors.css` 생성.
8. 볼트 CLAUDE.md 생성 (Section 4.1b의 템플릿).
9. git 초기화 (Section 8).
10. 구조를 제시하고 묻기: "시작하기 전에 조정할 것 있나요?"

**모드 선택:**

| 사용자가 말하면 | 최적 모드 |
|-----------|----------|
| "내 웹사이트", "사이트맵", "콘텐츠 감사" | A: Website |
| "내 저장소", "코드베이스 맵", "아키텍처 위키" | B: GitHub |
| "내 비즈니스", "프로젝트 위키", "경쟁 인텔" | C: Business |
| "두 번째 두뇌", "목표", "저널", "내 인생" | D: Personal |
| "리서치 주제", "논문", "딥다이브" | E: Research |
| "읽고 있는 책", "강의 노트", "챕터 트래커" | F: Book/Course |

모드 결합 가능. "GitHub 저장소 + AI 접근법 리서치"는 모드 B 폴더 + 모드 E의 papers/ 폴더 사용.

### 4.1a — 6가지 위키 모드

**모드 A: 웹사이트 / 사이트맵**

```
vault/
├── .raw/              # crawl exports, analytics, GSC data
├── wiki/
│   ├── pages/         # one note per URL
│   ├── structure/     # site architecture, nav hierarchy
│   ├── audits/        # content gaps, redirect needs
│   ├── keywords/      # keyword clusters, target page assignments
│   └── entities/      # brand, authors, topic hubs
```

pages/용 frontmatter: `url`, `status` (live|redirect|404|stub|no-index), `h1`, `meta_description`, `word_count`, `has_schema`, `indexed`, `canonical`, `internal_links_in`, `internal_links_out`, `last_crawled`

핵심 페이지: `[[Site Overview]]`, `[[Navigation Structure]]`, `[[Content Gaps]]`, `[[Redirect Map]]`, `[[Keyword Clusters]]`

---

**모드 B: GitHub / 저장소**

```
vault/
├── .raw/              # README, git log exports, code dumps
├── wiki/
│   ├── modules/       # one note per module / package / service
│   ├── components/    # reusable components
│   ├── decisions/     # Architecture Decision Records
│   ├── dependencies/  # external deps, versions, risk
│   └── flows/         # data flows, request paths, auth flows
```

modules/용 frontmatter: `path`, `status` (active|deprecated|experimental|planned), `language`, `purpose`, `maintainer`, `depends_on`, `used_by`, `linked_issues`

핵심 페이지: `[[Architecture Overview]]`, `[[Data Flow]]`, `[[Tech Stack]]`, `[[Dependency Graph]]`, `[[Key Decisions]]`

---

**모드 C: 비즈니스 / 프로젝트**

```
vault/
├── .raw/              # meeting transcripts, Slack exports, docs
├── wiki/
│   ├── stakeholders/  # people, companies, decision-makers
│   ├── decisions/     # key decisions with rationale and date
│   ├── deliverables/  # milestones, outputs, status
│   ├── intel/         # competitor analysis, market research
│   └── comms/         # synthesized meeting notes
```

decisions/용 frontmatter: `status` (active|pending|done|blocked|superseded), `priority` (1-5), `date`, `owner`, `due_date`, `context`

핵심 페이지: `[[Project Overview]]`, `[[Stakeholder Map]]`, `[[Decision Log]]`, `[[Competitor Landscape]]`

---

**모드 D: 개인 / 두 번째 두뇌**

```
vault/
├── .raw/              # journal entries, articles, voice transcripts
├── wiki/
│   ├── goals/         # personal and professional goals
│   ├── learning/      # concepts being mastered
│   ├── people/        # relationships, shared context
│   ├── areas/         # life areas: health, finances, career
│   └── resources/     # books, courses, tools
├── _meta/
│   └── hot-cache.md   # ~500 words of active context
```

goals/용 frontmatter: `area` (health|career|finance|creative|relationships|growth), `priority`, `target_date`, `progress` (0-100)

핵심 페이지: `[[North Star]]`, `[[Weekly Review Template]]`, `[[Annual Goals]]`

---

**모드 E: 리서치**

```
vault/
├── .raw/              # PDFs, web clips, raw notes
├── wiki/
│   ├── papers/        # paper summaries with key claims
│   ├── concepts/      # extracted concepts, models, frameworks
│   ├── entities/      # people, organizations, datasets
│   ├── thesis/        # evolving synthesis
│   └── gaps/          # open questions, contradictions
```

papers/용 frontmatter: `year`, `authors`, `venue`, `key_claim`, `methodology`, `contradicts`, `supports`

핵심 페이지: `[[Research Overview]]`, `[[Key Claims Map]]`, `[[Open Questions]]`, `[[Methodology Comparison]]`

---

**모드 F: 책 / 강의**

```
vault/
├── .raw/              # chapter notes, highlights, exercises
├── wiki/
│   ├── characters/    # characters, personas, experts
│   ├── themes/        # major themes with evidence
│   ├── concepts/      # domain-specific terms
│   ├── timeline/      # structure, sequence, chapter map
│   └── synthesis/     # your own takeaways and applications
```

concepts/용 frontmatter: `source_chapters`, `first_appearance`

핵심 페이지: `[[Book Overview]]`, `[[Theme Map]]`, `[[Character / Expert Index]]`, `[[My Takeaways]]`

### 4.1b — 볼트 CLAUDE.md 템플릿

새 프로젝트 볼트를 스캐폴드할 때 볼트 루트에 이것을 생성:

```markdown
# [WIKI NAME] — LLM Wiki

Mode: [MODE A/B/C/D/E/F]
Purpose: [ONE SENTENCE]
Owner: [NAME]
Created: YYYY-MM-DD

## Structure

[PASTE THE FOLDER MAP FROM THE CHOSEN MODE]

## Conventions

- All notes use YAML frontmatter: type, status, created, updated, tags (minimum)
- Wikilinks use [[Note Name]] format — filenames are unique, no paths needed
- .raw/ contains source documents — never modify them
- wiki/index.md is the master catalog — update on every ingest
- wiki/log.md is append-only — new entries go at the TOP, never edit past entries

## Operations

- Ingest: drop source in .raw/, say "ingest [filename]"
- Query: ask any question — Claude reads index first, then drills in
- Lint: say "lint the wiki" to run a health check
```

### 4.2 INGEST — 단일 소스

트리거: 사용자가 `.raw/`에 파일을 떨어뜨리거나 콘텐츠를 붙여넣음.

1. 소스를 완전히 읽기.
2. 사용자와 핵심 시사점 논의. "그냥 인제스트해"라고 하면 건너뜀.
3. `wiki/sources/`에 소스 요약 생성.
4. 언급된 모든 사람/조직/제품/저장소에 대한 엔티티 페이지를 생성하거나 갱신.
5. 의미 있는 아이디어에 대한 개념 페이지를 생성하거나 갱신.
6. 관련 도메인 페이지와 그것들의 `_index.md` 서브 인덱스 갱신.
7. 큰 그림이 바뀌었다면 `wiki/overview.md` 갱신.
8. `wiki/index.md` 갱신. 모든 새 페이지에 대한 항목 추가.
9. 이번 인제스트의 컨텍스트로 `wiki/hot.md` 갱신.
10. `wiki/log.md`에 추가 (새 항목은 맨 위에):
    ```markdown
    ## [2026-04-07] ingest | Source Title
    - Source: `.raw/articles/filename.md`
    - Summary: [[Source Title]]
    - Pages created: [[Page 1]], [[Page 2]]
    - Pages updated: [[Page 3]], [[Page 4]]
    - Key insight: One sentence on what is new.
    ```
11. 모순 검사. 두 페이지 모두에 `> [!contradiction]` 콜아웃으로 표시.

단일 소스는 보통 위키 페이지 8~15개에 영향을 미칩니다.

### 4.3 INGEST — 배치 모드

트리거: 사용자가 여러 파일을 떨어뜨리거나 "이것들 모두 인제스트해"라고 말함.

1. 처리할 모든 파일 나열. 사용자와 확인.
2. 각 소스를 단일 인제스트 흐름에 따라 처리. 상호 참조는 미룸.
3. 모든 소스 후: 상호 참조 패스. 새 소스 사이의 연결 찾기.
4. 인덱스, 핫 캐시, 로그를 소스마다가 아니라 마지막에 한 번 갱신.
5. 보고: "N개 소스 처리. X개 페이지 생성, Y개 페이지 갱신. 핵심 연결: ..."

배치 인제스트는 덜 인터랙티브합니다. 30개 이상의 경우 10개마다 체크인.

### 4.4 QUERY — 질문에 답하기

1. `wiki/hot.md`를 먼저 읽기. 답이 있을 수 있음.
2. `wiki/index.md`를 읽어 관련 페이지 찾기.
3. 그 페이지들 읽기 (보통 3~5개, 10개 이상은 너무 많음).
4. 채팅에서 답을 종합. 위키링크로 인용.
5. `wiki/questions/`에 위키 페이지로 정리할지 제안.
6. 질문이 격차를 드러내면: "X에 대한 자료가 부족합니다. 소스를 찾을까요?"

### 4.5 LINT — 헬스 체크

트리거: 사용자가 "lint"라고 말하거나 10~20회 인제스트마다.

검사: 고립 페이지, 끊긴 링크, 오래된 주장, 언급된 개념의 누락 페이지, 누락된 상호 참조, frontmatter 격차, 빈 섹션.

출력: `wiki/meta/lint-report-YYYY-MM-DD.md`. 자동 수정 전에 묻기.

---

## 5 — 인덱스와 서브 인덱스

### wiki/index.md (마스터)

```markdown
---
type: meta
title: "Wiki Index"
updated: 2026-04-07
---
# Wiki Index

## Domains
- [[Domain Name]] — description (N sources)

## Entities
- [[Entity Name]] — role (first: [[Source]])

## Concepts
- [[Concept Name]] — definition (status: developing)

## Sources
- [[Source Title]] — author, date, type

## Questions
- [[Question Title]] — answer summary
```

### 도메인 서브 인덱스

각 도메인 폴더는 그 도메인의 페이지만 카탈로그한 `_index.md`를 가짐.

```markdown
---
type: meta
title: "Entities Index"
updated: 2026-04-07
---
# Entities

## People
- [[Person Name]] — role, org

## Organizations
- [[Org Name]] — what they do
```

### wiki/log.md

추가 전용. 새 항목은 맨 위에. 각 항목: `## [YYYY-MM-DD] operation | title`

최근 항목 파싱:
```bash
grep "^## \[" wiki/log.md | head -10
```

---

## 6 — 크로스 프로젝트 참조

어떤 Claude Code 프로젝트든 컨텍스트를 복제하지 않고 당신의 위키를 읽을 수 있습니다.

다른 프로젝트의 CLAUDE.md에 추가:

```markdown
## Wiki Knowledge Base
Path: ~/Documents/Obsidian Vault

When you need context not already in this project:
1. Read wiki/hot.md first (recent context, ~500 words)
2. If not enough, read wiki/index.md (full catalog)
3. If you need domain specifics, read wiki/<domain>/_index.md
4. Only then read individual wiki pages

Do NOT read the wiki for general coding questions, things already in this
project's context, or tasks unrelated to [your domain].
```

이는 토큰 사용량을 낮게 유지합니다. 핫 캐시는 약 500 토큰. 인덱스는 약 1000 토큰. 개별 페이지는 각각 100~300 토큰.

---

## 7 — 시각적 커스터마이징

스캐폴드 중에 적용. `.obsidian/snippets/vault-colors.css` 생성:

```css
:root {
  --wiki-1: #4fc1ff;  --wiki-2: #c586c0;  --wiki-3: #dcdcaa;
  --wiki-4: #ce9178;  --wiki-5: #6a9955;  --wiki-6: #d16969;
  --wiki-7: #569cd6;
}

.nav-folder-title[data-path^="wiki/domains"]     { color: var(--wiki-1); }
.nav-folder-title[data-path^="wiki/entities"]    { color: var(--wiki-2); }
.nav-folder-title[data-path^="wiki/concepts"]    { color: var(--wiki-3); }
.nav-folder-title[data-path^="wiki/sources"]     { color: var(--wiki-4); }
.nav-folder-title[data-path^="wiki/questions"]   { color: var(--wiki-5); }
.nav-folder-title[data-path^="wiki/comparisons"] { color: var(--wiki-6); }
.nav-folder-title[data-path^="wiki/meta"]        { color: var(--wiki-7); }
.nav-folder-title[data-path=".raw"]              { color: #808080; opacity: 0.6; }

.callout[data-callout='contradiction'] { --callout-color: 209, 105, 105; --callout-icon: lucide-alert-triangle; }
.callout[data-callout='gap']           { --callout-color: 220, 220, 170; --callout-icon: lucide-help-circle; }
.callout[data-callout='key-insight']   { --callout-color: 79, 193, 255;  --callout-icon: lucide-lightbulb; }
.callout[data-callout='stale']         { --callout-color: 128, 128, 128; --callout-icon: lucide-clock; }
```

활성화: Settings > Appearance > CSS Snippets > 새로고침 > 토글 켜기.

### 그래프 뷰 그룹

그래프 뷰 설정에서:

| 쿼리 | 색 |
|-------|-------|
| `path:wiki/domains` | Blue |
| `path:wiki/entities` | Purple |
| `path:wiki/concepts` | Yellow |
| `path:wiki/sources` | Orange |
| `path:wiki/questions` | Green |
| `path:.raw` | Gray (dimmed) |

---

## 8 — Git 셋업

```bash
cd "$VAULT_PATH"
git init
cat > .gitignore << 'EOF'
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.smart-connections/
.obsidian-git-data
.trash/
.DS_Store
node_modules/
EOF
git add -A && git commit -m "Initial vault scaffold"
```

Obsidian Git 활성화: Settings > Obsidian Git > Auto backup interval > 15분.

---

## 9 — Dataview 대시보드

스캐폴드 후 `wiki/meta/dashboard.md`에 생성:

````markdown
---
type: meta
title: "Dashboard"
---
# Wiki Dashboard

## Recent Activity
```dataview
TABLE type, status, updated FROM "wiki" SORT updated DESC LIMIT 15
```

## Seed Pages (Need Development)
```dataview
LIST FROM "wiki" WHERE status = "seed" SORT updated ASC
```

## Entities Missing Sources
```dataview
LIST FROM "wiki/entities" WHERE !sources OR length(sources) = 0
```
````

---

## 10 — 컨텍스트 창 관리

필요한 최소만 읽으세요:

- `hot.md`를 먼저 읽기. 필요한 것이 이미 있을 수 있음.
- `index.md`를 두 번째로 읽기. 관련 페이지를 찾고, 모두 스캔하지 말 것.
- 집중된 조회를 위해 도메인 서브 인덱스를 읽기.
- 쿼리당 3~5 페이지만 읽기. 10개 이상은 너무 많음.
- 키워드 조회는 검색 사용. 단어 찾기 위해 전체 페이지 스캔하지 말 것.
- 정밀 편집은 PATCH 사용. 한 필드를 바꾸기 위해 전체 파일을 다시 읽고 다시 쓰지 말 것.
- 위키 페이지 짧게 유지. 최대 100~300줄. 긴 페이지는 분리.
- 사용자가 요청하지 않으면 위키 콘텐츠를 채팅에 붙여넣지 말 것. 위키링크로 참조.

---

## 11 — REST API 빠른 레퍼런스

명령 실행 전에 다음 설정:

```bash
API="https://127.0.0.1:27124"
KEY="your-api-key-here"
```

**파일 읽기:**
```bash
curl -sk -H "Authorization: Bearer $KEY" "$API/vault/wiki/index.md"
```

**파일 생성 또는 교체:**
```bash
curl -sk -X PUT \
  -H "Authorization: Bearer $KEY" \
  -H "Content-Type: text/markdown" \
  --data-binary @file.md \
  "$API/vault/wiki/entities/Name.md"
```

**파일에 추가:**
```bash
curl -sk -X POST \
  -H "Authorization: Bearer $KEY" \
  -H "Content-Type: text/markdown" \
  --data "- New item" \
  "$API/vault/wiki/log.md"
```

**Frontmatter 필드 패치:**
```bash
curl -sk -X PATCH \
  -H "Authorization: Bearer $KEY" \
  -H "Operation: replace" -H "Target-Type: frontmatter" \
  -H "Target: status" -H "Content-Type: application/json" \
  --data '"mature"' \
  "$API/vault/wiki/concepts/Name.md"
```

**헤딩 아래 추가:**
```bash
curl -sk -X PATCH \
  -H "Authorization: Bearer $KEY" \
  -H "Operation: append" -H "Target-Type: heading" \
  -H "Target: Connections" -H "Content-Type: text/markdown" \
  --data "- [[New Page]]" \
  "$API/vault/wiki/entities/Name.md"
```

**검색:**
```bash
curl -sk -X POST \
  -H "Authorization: Bearer $KEY" \
  "$API/search/simple/?query=machine+learning"
```

**Dataview 쿼리:**
```bash
curl -sk -X POST \
  -H "Authorization: Bearer $KEY" \
  -H "Content-Type: application/vnd.olrapi.dataview.dql+txt" \
  --data 'TABLE status FROM "wiki" WHERE status = "seed"' \
  "$API/search/"
```

---

## 12 — 볼트 CLAUDE.md 템플릿

새 프로젝트용 위키를 만들 때 (이 플러그인이 아닌), 볼트 루트에 CLAUDE.md 생성:

```markdown
# [WIKI NAME] — LLM Wiki

Mode: [MODE A/B/C/D/E/F]
Purpose: [ONE SENTENCE]
Owner: [NAME]
Created: YYYY-MM-DD

## Structure

[PASTE THE FOLDER MAP FROM THE CHOSEN MODE]

## Conventions

- All notes use YAML frontmatter: type, status, created, updated, tags (minimum)
- Wikilinks use [[Note Name]] format
- .raw/ contains source documents — never modify them
- wiki/index.md is the master catalog — update on every ingest
- wiki/log.md is append-only — new entries go at the TOP

## Operations

- Ingest: drop source in .raw/, say "ingest [filename]"
- Query: ask any question
- Lint: say "lint the wiki"
```

---

## 13 — 컨벤션

### 명명

- **파일명**: 공백을 가진 Title Case (`Machine Learning.md`)
- **폴더**: 대시가 있는 소문자 (`wiki/data-models/`)
- **태그**: 소문자, 계층적 (`#domain/architecture`)
- **고유한 파일명**: 위키링크가 경로 없이 동작하도록

### 글쓰기 스타일

- 단정적, 현재 시제. "X basically does Y"가 아니라 "X uses Y".
- 링크를 후하게. 위키 페이지의 모든 언급은 위키링크를 받음.
- 소스 인용: `(Source: [[Page]])`.
- 불확실성 표시: `> [!gap] This needs more evidence.`
- 모순 표시: `> [!contradiction] [[Page A]] claims X, but [[Page B]] says Y.`

### 상호 참조

페이지 A를 갱신해 페이지 B를 언급할 때, 페이지 B가 다시 링크되어야 하는지 확인. 양방향 링크가 그래프 뷰를 유용하게 만듭니다.

---

## 14 — 캔버스 맵

시각적 개요를 위한 `.canvas` 파일 생성:

```json
{
  "nodes": [
    {"id": "1", "type": "file", "file": "wiki/domains/Architecture.md",
     "x": 0, "y": 0, "width": 250, "height": 120, "color": "4"},
    {"id": "2", "type": "file", "file": "wiki/domains/APIs.md",
     "x": 300, "y": 0, "width": 250, "height": 120, "color": "5"}
  ],
  "edges": [
    {"id": "e1", "fromNode": "1", "fromSide": "right",
     "toNode": "2", "toSide": "left", "toEnd": "arrow"}
  ]
}
```

캔버스 노드 색 (Obsidian 캔버스 컬러 코드): 1=red, 2=orange, 3=yellow, 4=green, 5=cyan, 6=purple.
참고: 이는 위키 그래프 CSS 컬러 스킴과 다릅니다. 전체 캔버스 컬러 표는 `skills/canvas/references/canvas-spec.md` 참조.

스캐폴드 중에 도메인 관계 캔버스를 생성. 위키가 자라면서 갱신.

---

## 요약

LLM으로서 당신의 일:
1. 볼트 셋업 (한 번)
2. 사용자의 도메인 설명에서 위키 구조 스캐폴드
3. 소스 인제스트: 읽기, 요약, 상호 참조, 정리
4. 매 작업 후 핫 캐시 유지
5. 인덱스 → 관련 페이지 → 종합으로 질문 답변
6. 좋은 답을 다시 위키에 정리
7. 주기적 린트: 헬스 이슈 찾고 고치기
8. 절대 .raw/ 소스 수정하지 않기
9. 항상 인덱스, 서브 인덱스, 로그, 핫 캐시 갱신
10. 항상 frontmatter와 위키링크 사용

사람의 일: 소스 큐레이팅, 좋은 질문 던지기, 의미 생각하기. 나머지는 모두 당신에게.

---

*Andrej Karpathy의 LLM 위키 패턴 기반. 플러그인: AgriciDaniel / AI Marketing Hub의 claude-obsidian.*
