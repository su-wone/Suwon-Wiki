---
type: overview
title: "위키 개요"
created: 2026-05-07
updated: 2026-05-07
tags:
  - meta
  - overview
status: developing
related:
  - "[[index]]"
  - "[[hot]]"
  - "[[log]]"
sources:
---

# 위키 개요

내비게이션: [[index]] | [[hot]] | [[log]]

---

## 목적

이 볼트는 4개 영역을 통합하는 영속적 지식 베이스입니다:

1. **풀스택 개발** — 본인 코드, 프로젝트, 아키텍처 결정
2. **AI 논문 리서치** — 논문 요약, 방법론 비교, 핵심 주장
3. **취업시장 분석** — 회사, JD, 연봉, 스택 트렌드
4. **개발 노트** — 학습 내용, 강의 시사점, 실무 적용

자세한 운영 규칙은 [[CLAUDE]] 참고.

---

## 시작하기

1. 자료를 받으면 모드 판단 → `.raw/<모드폴더>/`에 떨어뜨림
2. Claude Code에서 `ingest <파일명>` 입력
3. Claude가 frontmatter, 위키링크, 인덱스 갱신을 자동 처리
4. 질문은 그냥 입력 — `index.md` → 관련 페이지 순으로 회수

## 폴더 구조

- `.raw/` — 원시 소스 (불변, 6모드 폴더)
- `wiki/` — Claude가 생성한 지식 베이스
- `_templates/` — 노트 템플릿
- `_attachments/` — 이미지, PDF
