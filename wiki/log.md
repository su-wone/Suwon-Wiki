---
type: meta
title: "작업 로그"
created: 2026-05-07
updated: 2026-05-07
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

## [2026-05-07] init | wiki bootstrap
- 빈 위키 골격으로 시작. `.raw/`에 소스 떨어뜨리고 `ingest <파일명>`으로 첫 페이지 생성.
