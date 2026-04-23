---
type: meta
title: "작업 로그"
created: 2026-04-23
updated: 2026-04-23
tags:
  - meta
  - log
status: evergreen
related:
  - "[[index]]"
  - "[[hot]]"
  - "[[overview]]"
---

# 작업 로그

내비게이션: [[index]] | [[hot]] | [[overview]]

추가 전용. 새 항목은 맨 위에 추가합니다. 과거 항목은 절대 수정하지 마세요.

항목 형식: `## [YYYY-MM-DD] operation | Title`

최근 항목 파싱: `grep "^## \[" wiki/log.md | head -10`

---

## [2026-04-23] setup | 볼트 초기화
- 위치: `/Users/admin/wiki/`
- 모드: B + E + C + F 결합 (풀스택 + AI 리서치 + 취업시장 + 개발 노트)
- 생성된 도메인 허브: [[fullstack-dev]], [[ai-research]], [[job-market]], [[dev-notes]]
- 시드 파일: [[index]], [[hot]], [[overview]], [[log]]
- 템플릿 5종 (concept, entity, source, question, comparison) 생성
- 다음 단계: 첫 소스를 `.raw/`에 떨어뜨리고 "ingest [파일명]"
