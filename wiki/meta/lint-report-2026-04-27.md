---
type: meta
title: "Lint Report 2026-04-27"
created: 2026-04-27
updated: 2026-04-27
tags:
  - meta
  - lint
status: developing
related:
  - "[[index]]"
  - "[[wiki-운영-매뉴얼]]"
---

# Lint Report: 2026-04-27

위키 헬스 체크 결과. 첫 lint 패스 (3 인제스트 + 구조 개편 후).

## Summary

| 항목 | 결과 |
|---|---|
| 페이지 스캔 | 48 |
| 발견 이슈 | 13 (즉시 수정 권장) + 8 (제안) |
| Orphans | **0** ✅ |
| Frontmatter gaps | **0** ✅ |
| Dead links (실제) | 6 |
| Stale paths | 4 (index/ai-research/papers `_index`) |
| Stale claims | 1 (overview 통계) |
| Empty sections (실제 도메인 허브) | 11 |
| Missing pages 후보 | 8 |

## 1. Orphans (0개)

모든 페이지가 최소 1개 이상 인바운드 링크 보유. ✅

## 2. Dead Links (6개)

### 즉시 수정 권장 (4개)

| 파일:라인 | 링크 | 진단 | 제안 |
|---|---|---|---|
| `entities/_index.md:22` | `[[Name]]` | 위키링크 **문법 설명** 텍스트인데 실제 링크로 파싱됨 | 백틱으로 감싸서 `` `[[Name]]` `` |
| `overview.md:36` | `[[React]]` | 크로스도메인 예시 설명 (React 페이지 미존재) | 백틱: `` `[[React]]` `` 또는 React 엔티티 stub 생성 |
| `domains/dev-notes.md:63` | `[[개념]]` | 사용법 예시 안의 placeholder | 꺾쇠로 `<개념>` 또는 이탤릭 *개념* |
| `questions/위키-능동-사용법.md:57` | `[[wiki/questions/]]` | 경로형 위키링크 (trailing slash → 빈 target) | `[[questions/_index]]` 또는 폴더 표기 |

### 추가 전용 로그 (수정 금지)

| 파일:라인 | 링크 |
|---|---|
| `log.md:48` | `[[Name]]` (문법 설명) |
| `log.md:77` | `[[wiki/questions/]]` |

→ `log.md`는 append-only 규칙이라 손대지 않음. 단, 향후 같은 실수를 막으려면 `[[N]]` 표기 시 백틱으로 감싸는 컨벤션 필요.

## 3. Stale Paths (4개)

`.raw/` 6모드 재구성 (2026-04-23) 이후 `.raw/papers/` → `.raw/E-research/`로 이동했지만 일부 페이지가 옛 경로를 가리킴.

| 파일:라인 | 옛 경로 | 새 경로 |
|---|---|---|
| `index.md:108` | `.raw/papers/Realtime ... PAF.pdf` | `.raw/E-research/...` |
| `domains/ai-research.md:31` | `.raw/papers/` | `.raw/E-research/` |
| `domains/ai-research.md:65,66` | `ingest .raw/papers/<paper>.pdf` | `ingest .raw/E-research/<paper>.pdf` |
| `papers/_index.md:57` | `ingest .raw/papers/<paper>.pdf` | `ingest .raw/E-research/<paper>.pdf` |

> [!note] log.md의 옛 경로 언급(line 90, 102)은 시간순 기록이라 보존.

## 4. Stale Claims (1개)

| 파일:라인 | 문제 |
|---|---|
| `overview.md:62-64` | "인제스트된 소스: 0", "위키 페이지: 시드 파일 + 도메인 허브 4개" → 실제는 소스 3, 페이지 48 |

## 5. Empty Sections

`_index.md` 카탈로그 페이지의 미래 카테고리 placeholder는 정상(예: `companies/_index` Active/Applied/Interviewing) — 게으른 확장 규칙상 의도된 빈 섹션. **여기서 제외.**

### 도메인 허브의 실제 빈 섹션 (11개)

| 파일 | 섹션 | 제안 |
|---|---|---|
| `domains/ai-research.md` | 방법론 비교, 열린 질문 / 격차 | placeholder 주석 추가 또는 항목 시드 |
| `domains/dev-notes.md` | 진행 중 코스 / 책, 핵심 멘토 / 강사 | placeholder 주석 추가 |
| `domains/fullstack-dev.md` | 기술 결정 (0개), 열린 질문 | 실제 ADR 시드 또는 주석 |
| `domains/job-market.md` | 회사 (0개), 자주 요구되는 스택, 연봉 / 보상 트렌드, 인터뷰 패턴, 핵심 채용 담당자, 회사 비교 | 모두 빈 도메인 — 첫 JD 인제스트 전까진 정상 |
| `index.md:121` | 비교 (Comparisons) | placeholder 주석 추가 |

→ 도메인 허브 자체가 "비어있음"을 시각적으로 드러내는 건 의도일 수 있음. **자동 수정 안 함**, 사용자 판단.

## 6. Missing Pages (8개 후보)

자주 언급되지만 자체 페이지 없는 항목:

| 후보 | 유형 | 등장 페이지 수 | 우선순위 |
|---|---|---|---|
| **Jira** | tool | 6 | 🔥 높음 (server-board 워크플로 핵심) |
| **ADR** | concept | 6 | 🔥 높음 (decisions 도메인 진입점) |
| **ORM** | concept | 6 | 중간 |
| **tsx** | tool | 5 | 중간 (server-board seed/ESM 운영) |
| **VEASLY** | project-meta | 4 | 낮음 (코드네임/이슈 prefix) |
| **Jest** | tool | 3 | 중간 |
| **AWS** | platform | 3 | 낮음 (S3만 사용 중) |
| **DTO** | concept | 3 | 낮음 (NestJS 페이지에 흡수 가능) |

## 7. Missing Cross-References

샘플로 본 결과 [[server-board]] / [[esm-import-rule]] 본문에서 [[Node.js]], [[NestJS]] 등은 잘 링크됨. 큰 누락 없음. 단:

- `questions/Board 풀스택 학습 가이드.md`에서 `Jest`, `tsx`, `Jira` 언급되지만 위키링크 없음 → 페이지 생성 후 자동 연결 가능

## 8. 네이밍 컨벤션 검사

| 항목 | 결과 |
|---|---|
| 파일명 고유성 | ✅ 모든 basename 고유 (위키링크 경로 불필요) |
| 폴더 lowercase-dash | ⚠️ 대체로 OK. `wiki/entities/{people,tools,...}` 정상 |
| 파일명 형식 | 혼합 — Kebab/CamelCase/spaces/한글 공존 (의도된 패턴) |

## 9. 권장 작업

### 자동 수정 가능 (사용자 승인 시 진행)

- [ ] **stale paths 4건** 일괄 치환 (`.raw/papers/` → `.raw/E-research/`)
- [ ] `overview.md:62-64` 통계 갱신 (소스 0→3, 페이지 4→48)
- [ ] `entities/_index.md:22`의 `[[Name]]` → 백틱 처리
- [ ] `overview.md:36`의 `[[React]]` → 백틱 처리
- [ ] `domains/dev-notes.md:63`의 `[[개념]]` → `<개념>` 처리
- [ ] `questions/위키-능동-사용법.md:57`의 `[[wiki/questions/]]` → `[[questions/_index]]`

### 사용자 판단 필요

- [ ] **Jira / ADR stub** 생성 여부 (개념 페이지)
- [ ] **Jest / tsx** 엔티티 stub 생성 여부 (도구 페이지)
- [ ] 도메인 허브 빈 섹션 — 그대로 둘지 placeholder 주석 추가할지
- [ ] **React / FastAPI / Python** stub 생성 여부 (overview / index 예시 텍스트에서 언급)

## 다음 lint 시점

운영 매뉴얼상 주 1회 또는 10~15 인제스트 후. 다음 후보: 5번째 인제스트 직후.

## 관련

- [[wiki-운영-매뉴얼]] — 정기 유지보수 섹션
- [[index]] — 마스터 카탈로그
