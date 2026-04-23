# 개인 위키 볼트

Mode: B + E + C + F (풀스택 개발, AI 리서치, 취업시장, 개발 노트) — 단, `.raw/`는 6모드 모두 수용
Purpose: 풀스택 개발, AI 논문 리서치, 취업시장 분석, 개발 노트를 위한 영속적 지식 베이스
Owner: admin (sw.kang@newndy.com)
Created: 2026-04-23

## 구조

```
.raw/                원시 소스 (불변, Claude는 읽기만) — 6모드 폴더
  A-website/         모드 A: 사이트 크롤, 웹페이지 dump, 분석 데이터
  B-github/          모드 B: 본인 코드, README, git log, 저장소 dump
  C-business/        모드 C: 회사 자료, 회의록, JD, Slack export
  D-personal/        모드 D: 일기, 음성 트랜스크립트, AI 대화 로그
  E-research/        모드 E: 논문 PDF, 웹 리서치 클립
  F-books/           모드 F: 책/강의 노트, 챕터 하이라이트

wiki/                Claude가 생성하는 지식 베이스
  index.md           마스터 카탈로그
  log.md             작업 로그 (추가 전용)
  hot.md             핫 캐시 (약 500단어)
  overview.md        위키 개요
  domains/           4개 도메인 허브
  projects/          본인 풀스택 프로젝트 (B)
  papers/            AI 논문 요약 (E)
  companies/         취업 시장 회사 (C)
  learning/          개발 학습 노트 (D, F)
  decisions/         기술 결정 / ADR (B, C)
  concepts/          공유 개념
  entities/          사람, 회사, 도구
  sources/           인제스트된 소스 요약
  comparisons/       기술/회사/논문 비교
  questions/         좋은 답변 정리
  meta/              운영 매뉴얼, 대시보드, 린트 보고서

_templates/          노트 템플릿
  A-website.md       모드 A 페이지
  B-module.md        모드 B 모듈
  C-decision.md      모드 C 결정
  D-personal.md      모드 D 개인 노트
  E-paper.md         모드 E 논문
  F-book.md          모드 F 책/강의
  concept.md         공유 (개념)
  entity.md          공유 (사람/조직/도구)
  source.md          공유 (소스 요약)
  comparison.md      공유 (비교)
  question.md        공유 (Q&A)

_attachments/        이미지, PDF
```

## 규칙

- 모든 노트는 YAML frontmatter 사용: type, status, created, updated, tags (최소)
- 위키링크는 `[[Note Name]]` 형식. 파일명은 고유해서 경로 불필요
- `.raw/`는 절대 수정 금지
- `wiki/index.md`는 마스터 카탈로그. 매 인제스트 시 갱신
- `wiki/log.md`는 추가 전용. 새 항목은 맨 위에. 과거 항목 수정 금지

## 작업

- **Ingest**: 소스를 `.raw/<모드폴더>/`에 떨어뜨리고 "ingest [파일명]" 입력
- **Query**: 아무 질문. Claude가 인덱스를 먼저 읽고 관련 페이지로 파고듦
- **Lint**: "lint the wiki"로 헬스 체크
- **Save**: `/save`로 현재 대화를 위키 노트로 정리
- **Autoresearch**: `/autoresearch [주제]`로 자율 웹 리서치

## 어떤 자료를 어디에 넣을지 (의사결정 룰 — 한 번만 묻기)

> 자료 받으면 → "이게 어느 모드인가?" → 해당 `.raw/<모드>/` 폴더에 던짐.

| 자료 | 모드 폴더 |
|---|---|
| 웹페이지 dump, 사이트 크롤, GSC | `.raw/A-website/` |
| 본인 코드, README, git log | `.raw/B-github/` |
| 회사 정보, JD, 회의록, Slack | `.raw/C-business/` |
| 일기, AI 대화 로그, 개인 메모 | `.raw/D-personal/` |
| 논문 PDF, 외부 리서치 클립 | `.raw/E-research/` |
| 책/강의 노트, 챕터 | `.raw/F-books/` |

## wiki/ 게으른 확장 규칙 (Lazy expansion)

`.raw/`는 6모드 모두 받지만, `wiki/` 출력 폴더는 **실제로 쓸 때만** 만든다. 빈 폴더로 결정 부하 늘리지 않기 위함.

### 첫 사용 시 자동 신설할 폴더

| 모드 | 첫 인제스트 시 추가할 폴더 (없으면) |
|---|---|
| **A 웹사이트** | `wiki/pages/`, `wiki/audits/`, `wiki/keywords/` |
| **B GitHub** (확장) | 필요 시 `wiki/components/`, `wiki/dependencies/`, `wiki/flows/` |
| **C 비즈니스** (확장) | 필요 시 `wiki/deliverables/`, `wiki/intel/`, `wiki/comms/`, `wiki/stakeholders/` |
| **D 개인** (확장) | 필요 시 `wiki/goals/`, `wiki/people/`, `wiki/areas/`, `wiki/resources/` |
| **E 리서치** (확장) | 필요 시 `wiki/thesis/`, `wiki/gaps/` |
| **F 책/강의** | `wiki/themes/`, `wiki/synthesis/`, `wiki/timeline/`, `wiki/characters/` (필요한 것만) |

### 절차

1. 인제스트 시작 시 해당 모드의 페이지가 들어갈 wiki/ 폴더가 없는지 확인
2. 없으면 폴더 + `_index.md` 시드 생성
3. log에 `## [날짜] structure | wiki/<폴더>/ 신설 (모드 X 첫 사용)` 항목 prepend
4. 그 다음 정상 인제스트 진행

### 절대 미리 만들지 않을 것

빈 폴더는 의사결정 부하를 늘림. **데이터가 그 폴더를 필요로 할 때만** 생성.

## 4개 활성 도메인

1. **풀스택 개발 (B)** — `wiki/projects/`, `wiki/decisions/`
2. **AI 논문 리서치 (E)** — `wiki/papers/`, `wiki/concepts/`
3. **취업시장 분석 (C)** — `wiki/companies/`, `wiki/decisions/`
4. **개발 노트 (D/F)** — `wiki/learning/`

## 크로스 도메인 연결

개념은 `wiki/concepts/`에, 사람/도구/라이브러리는 `wiki/entities/`에 공유됩니다. AI 논문에서 배운 개념이 본인 프로젝트에 적용되면 두 곳에서 같은 개념 페이지를 참조하게 됩니다. 이것이 위키가 복리로 쌓이는 방식입니다.

## 운영 매뉴얼

자세한 운영법은 [[wiki-운영-매뉴얼]] 참고.
