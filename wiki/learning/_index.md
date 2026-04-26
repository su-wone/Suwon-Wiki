---
type: meta
title: "학습 인덱스"
created: 2026-04-23
updated: 2026-04-26
tags:
  - meta
  - index
  - learning
domain: dev-notes
status: evergreen
related:
  - "[[index]]"
  - "[[dev-notes]]"
---

# 학습 인덱스

강의, 튜토리얼, 책, 유튜브에서 나온 학습 노트.

## 진행 중 (In Progress)

- [[esm-import-rule]] — ESM에서 import 경로에 `.js` 쓰는 이유 ([[server-board]] 적용 중)

## 완료 (Completed)

<!-- 마친 리소스 -->

## 대기열 (Queued)

<!-- 나중에 할 것 -->

## 주제별

### 프론트엔드
<!-- React, TypeScript, 상태 관리 등 -->

### 백엔드
- [[esm-import-rule]] — Node.js ESM 모드 + TypeScript 컴파일 모델

### 인프라 / 데브옵스
<!-- Docker, K8s, CI/CD 등 -->

### AI / ML
<!-- 논문 외의 실용 AI 학습 -->

### 알고리즘 / CS 기초
<!-- -->

### 소프트 스킬
<!-- 시스템 디자인, 커뮤니케이션, 협업 -->

---

## 학습 페이지 만들기

```
ingest .raw/transcripts/<주제>.md
```

### 권장 frontmatter

```yaml
---
type: learning
title: "학습 주제"
source_type: video  # video|course|book|article|tutorial
instructor: "강사명"
difficulty: intermediate  # beginner|intermediate|advanced
status: in-progress  # queued|in-progress|completed
applied_to:
  - "[[프로젝트명]]"
url: https://youtube.com/...
---
```
