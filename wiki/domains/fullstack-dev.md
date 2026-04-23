---
type: domain
title: "풀스택 개발"
created: 2026-04-23
updated: 2026-04-23
tags:
  - domain
  - fullstack
  - development
status: developing
related:
  - "[[index]]"
  - "[[overview]]"
  - "[[domains/_index]]"
  - "[[ai-research]]"
  - "[[dev-notes]]"
sources:
---

# 풀스택 개발

이 도메인은 본인이 진행하는 풀스택 프로젝트와 관련된 모든 것을 추적합니다.

## 이 도메인의 폴더

- **`wiki/projects/`** — 개별 프로젝트 페이지 (저장소 슬러그 또는 프로젝트 이름 파일)
- **`wiki/decisions/`** — Architecture Decision Records (ADR). 왜 이 기술을 골랐는지

## 이 도메인의 소스 위치

- `.raw/code/` — 코드 스니펫, README 복사본, 아키텍처 다이어그램
- `.raw/articles/` — 기술 블로그 포스트 (관련 주제)

## 프로젝트 (0개)

<!-- 첫 프로젝트 인제스트 후 여기 나열 -->

## 기술 결정 (0개)

<!-- 첫 ADR 생성 후 여기 나열 -->

## 자주 사용하는 스택

<!-- 사용 중인 주요 기술들이 자연스럽게 누적됨 -->

## 열린 질문

<!-- 아직 답을 찾지 못한 설계 문제들 -->

## 크로스 도메인 연결

- **AI 리서치**: [[ai-research]] — 프로젝트에 적용한 AI 기법
- **취업시장**: [[job-market]] — 이 스택을 요구하는 회사들
- **학습 노트**: [[dev-notes]] — 프로젝트 중 배운 개념

---

## 이 도메인을 키우는 방법

1. 진행 중인 프로젝트 README를 `.raw/code/`에 저장
2. `ingest .raw/code/<project-readme>.md`
3. Claude가 프로젝트 페이지, 사용 스택(엔티티), 핵심 개념을 자동 생성
4. 아키텍처 결정이 있을 때마다 `wiki/decisions/`에 ADR 생성
