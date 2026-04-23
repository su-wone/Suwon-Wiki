---
type: session
title: "wiki 운영 매뉴얼"
created: 2026-04-23
updated: 2026-04-23
tags:
  - meta
  - manual
  - workflow
  - operations
status: developing
related:
  - "[[index]]"
  - "[[hot]]"
  - "[[log]]"
  - "[[overview]]"
sources: []
---

# wiki 운영 매뉴얼

이 위키를 어떻게 채우고, 어떻게 유지하고, 무엇을 어디에 두는지 정리한 운영 가이드. 첫 세션(2026-04-23)에서 정해진 원칙이며, 새 패턴이 생기면 이 문서를 갱신한다.

## 1. 채우는 4가지 길

| 채우는 방법 | 빈도 | 명령 | 결과물 | 어떤 자료에 |
|---|---|---|---|---|
| **외부 자료 인제스트** | 새 자료 발견 시 | `.raw/`에 떨어뜨리고 `ingest [파일]` | 8~15개 페이지 다발 (소스 + 개념 + 엔티티) | 논문, 기사, JD, 트랜스크립트, 코드 |
| **대화에서 통찰 저장** | 좋은 대화 끝날 때 | `/save` (또는 `/save [이름]`) | 1개 노트 + 메타 갱신 | 클로드와의 토론, 분석, 결정 |
| **자율 리서치** | 깊이 파고 싶은 주제 | `/autoresearch [주제]` | 웹 검색 + 다수 페이지 자동 생성 | 모르는 주제 깊이 파기 |
| **본인 손 작성** | 결정/프로젝트 변할 때 | 직접 `wiki/projects/`, `wiki/decisions/`에 작성 | 1차 노트 | 본인 프로젝트, ADR, 진행 상황 |

## 2. `.raw/` 폴더 운영 원칙

> [!key-insight] 원칙
> `.raw/`는 **"외부에서 가져온 불변 원본"** 을 둘 때만 쓴다. Claude 대화나 본인 머릿속에서 정리한 내용은 `.raw`를 거치지 않고 `/save`로 바로 `wiki/`에 쓴다.

### 자료별 저장 위치 — 6모드 단순 룰 (v2, 2026-04-23 재구성)

> "이 자료는 어느 모드인가?" 한 번만 묻고 해당 모드 폴더에 던지면 끝.

| 모드 | `.raw/` 폴더 | 받는 자료 | 적용 템플릿 | wiki/ 결과 |
|---|---|---|---|---|
| **A — 웹사이트** | `.raw/A-website/` | 사이트 크롤, 웹페이지 dump, GSC, 분석 데이터 | `A-website.md` | `wiki/pages/`, `wiki/audits/` 등 |
| **B — GitHub/저장소** | `.raw/B-github/` | 본인 코드, README, git log, 저장소 dump | `B-module.md` | `wiki/projects/`, `wiki/decisions/` |
| **C — 비즈니스/프로젝트** | `.raw/C-business/` | 회사 정보, JD, 회의록, Slack export | `C-decision.md` | `wiki/companies/`, `wiki/decisions/` |
| **D — 개인/두 번째 두뇌** | `.raw/D-personal/` | 일기, AI 대화 로그, 음성 트랜스크립트, 개인 메모 | `D-personal.md` | `wiki/learning/`, `wiki/questions/` |
| **E — 리서치** | `.raw/E-research/` | 논문 PDF, 웹 리서치 클립, 외부 자료 | `E-paper.md` | `wiki/papers/`, `wiki/concepts/` |
| **F — 책/강의** | `.raw/F-books/` | 책 노트, 강의 자료, 챕터 하이라이트 | `F-book.md` | `wiki/learning/`, `wiki/concepts/` |

### `.raw/`를 거치지 않는 것

- Claude 채팅에서 즉석으로 정리한 메모 → `/save` (대화 자체가 원본)
- 본인이 직접 쓴 학습 노트 → `wiki/learning/`에 직접
- 위키 운영 결정 → `wiki/meta/` 또는 `wiki/decisions/`에 직접

### 모드를 못 정하겠을 때

- 외부에서 가져왔고 시간순/주제순으로 학습할 자료 → **F (책/강의)** 또는 **E (리서치)**
- 본인 일기/생각/AI 대화 → **D (개인)**
- 회사/프로젝트 진행 자료 → **C (비즈니스)**
- 코드/저장소 → **B (GitHub)**
- 사이트/웹페이지 → **A (웹사이트)**

### `.raw/`에 두지 말아야 할 것
- Claude 채팅에서 즉석으로 정리한 메모 → `/save`
- 본인이 직접 쓴 학습 노트 → `wiki/learning/`에 직접
- 위키 운영 결정 → `wiki/meta/` 또는 `wiki/decisions/`에 직접

## 3. `/save` vs `ingest` 차이

|  | `ingest` | `/save` |
|---|---|---|
| **입력** | `.raw/`의 외부 파일 | 지금 진행 중인 대화 |
| **`.raw/` 거치는가** | 그렇다 (원본 보존) | 아니다 (대화 자체가 원본) |
| **결과물 갯수** | 8~15개 (소스 + 개념 + 엔티티 다발) | 보통 1개 + 메타 갱신 |
| **타입 분류** | 자동 (소스 중심으로 부수 페이지 생성) | synthesis / concept / source / decision / session 중 자동 선택 |
| **언제 쓰나** | 외부 문서를 위키화할 때 | 대화에서 얻은 통찰을 영속화할 때 |

### `/save` 호출 형태

- `/save` — 알맹이 추출 + 적절한 타입으로 저장 (이름 묻기)
- `/save [이름]` — 이름 미리 지정
- `/save concept [이름]` — concept 페이지로 강제
- `/save decision [이름]` — 결정 기록으로 강제
- `/save session` — 세션 전체 요약

### `/save`가 분류하는 노트 타입

| 타입 | 폴더 | 언제 |
|---|---|---|
| synthesis | `wiki/questions/` | 다단계 분석, 비교, 특정 질문 답 |
| concept | `wiki/concepts/` | 개념/패턴/프레임워크 정의 |
| source | `wiki/sources/` | 외부 자료 요약 |
| decision | `wiki/meta/` | 아키텍처/프로젝트/전략 결정 |
| session | `wiki/meta/` | 세션 전체 요약 (← 이 매뉴얼이 그 예) |

## 4. 두뇌가 살찌는 신호 (체크리스트)

- [ ] 매주 새 인제스트 1~3건
- [ ] `wiki/concepts/`가 도메인 간 연결되기 시작 (예: AI 논문 개념이 본인 프로젝트 결정에 인용됨)
- [ ] `wiki/comparisons/`에 비교 페이지 생김 (PAF vs HRNet, 회사 A vs B 등)
- [ ] `lint the wiki` 정기 실행으로 dead link / orphan / stale claim 정리
- [ ] [[hot]] 캐시가 "오늘 머릿속에 있는 것"을 정확히 반영
- [ ] 한 도메인이 다른 도메인의 페이지를 참조하기 시작 (예: 논문 개념 → 본인 프로젝트 결정)

## 5. 첫 2주 추천 플랜

1. **이번 주**: 이 매뉴얼 노트 확보 (← 지금 함). 운영 기준선 마련.
2. **OpenPose 후속**: 관련 논문 1편 더 인제스트해서 [[cao-2017-openpose-paf]]와 비교 페이지 생성. 비교가 생기면 위키가 살아남.
3. **본인 프로젝트** 중 하나를 `wiki/projects/`에 시드 노트로 작성. 풀스택 도메인 첫 진입점.
4. **취업시장**: 관심 회사 JD 1개 `.raw/jobs/`에 넣고 인제스트. 4번째 도메인 활성화.

## 6. 정기 유지보수

| 주기 | 작업 | 명령 |
|---|---|---|
| 인제스트 직후 | hot/index/log 갱신 (자동) | (인제스트 워크플로 안에 포함) |
| 주 1회 | 위키 헬스 체크 | `lint the wiki` |
| 새 도메인 활성화 시 | 도메인 허브 페이지 가지치기 | 직접 편집 |
| 한 달에 한 번 | overview, hot 캐시 점검 | 직접 검토 |

## 관련

- [[index]] — 마스터 카탈로그
- [[hot]] — 핫 캐시
- [[log]] — 작업 로그
- [[overview]] — 위키 개요

## 갱신 이력

- 2026-04-23 (v1): 첫 세션에서 작성. `/save` vs `ingest` 차이 정립. `.raw` 폴더 운영 원칙 정립.
- 2026-04-23 (v2): `.raw/` 6모드 폴더로 재구성 (A-website / B-github / C-business / D-personal / E-research / F-books). 모드별 템플릿 6개 추가. 자료 종류별 → 모드별로 의사결정 단순화.
