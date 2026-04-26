---
type: domain
title: "개발 노트"
created: 2026-04-23
updated: 2026-04-26
tags:
  - domain
  - learning
  - notes
status: developing
related:
  - "[[index]]"
  - "[[overview]]"
  - "[[domains/_index]]"
  - "[[fullstack-dev]]"
  - "[[ai-research]]"
sources:
---

# 개발 노트

강의, 튜토리얼, 책, 유튜브, 블로그에서 배운 내용을 정리하는 도메인입니다. 새 개념을 소화하고 본인 프로젝트에 적용할 수 있게 연결합니다.

## 이 도메인의 폴더

- **`wiki/learning/`** — 학습 주제별 페이지 (개념, 패턴, 기법)

## 이 도메인의 소스 위치

- `.raw/transcripts/` — 유튜브 트랜스크립트, 강의 녹음
- `.raw/articles/` — 블로그 포스트, 튜토리얼
- `.raw/code/` — 실습 코드 스니펫

## 학습 주제 (1개)

- [[esm-import-rule]] — ESM에서 import 경로에 `.js` 쓰는 규칙 ([[server-board]] 적용)

## 진행 중 코스 / 책

<!-- 현재 학습 중인 리소스. 상태 추적 -->

## 핵심 멘토 / 강사

<!-- wiki/entities/ 의 person 페이지로 연결 -->

## 적용 사례

- [[esm-import-rule]] → [[server-board]]: NestJS 11 + TS 5.7 ESM 모드에서 import 경로 `.js` 적용

## 크로스 도메인 연결

- **풀스택 개발**: [[fullstack-dev]] — 학습을 프로젝트에 어떻게 적용했는가
- **AI 리서치**: [[ai-research]] — 학습한 AI 논문 개념
- **취업시장**: [[job-market]] — 면접에서 자주 나오는 학습 주제

---

## 이 도메인을 키우는 방법

1. 유튜브 트랜스크립트를 `.raw/transcripts/<주제>.md`에 저장
2. `ingest .raw/transcripts/<주제>.md`
3. Claude가 핵심 개념 페이지, 강사(엔티티), 적용 아이디어를 정리
4. 프로젝트에 실제 적용했다면 프로젝트 페이지에 "Applied concepts: [[개념]]" 섹션 추가

### 권장 frontmatter 필드 (학습 노트용)

- `source_type` (video|course|book|article), `instructor`, `difficulty` (beginner|intermediate|advanced), `applied_to` (프로젝트 위키링크)
