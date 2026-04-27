---
type: meta
title: "Dashboard"
created: 2026-04-27
updated: 2026-04-27
tags:
  - meta
  - dashboard
status: evergreen
related:
  - "[[index]]"
  - "[[hot]]"
  - "[[lint-report-2026-04-27]]"
---

# Wiki Dashboard

Dataview 기반 실시간 위키 상태판. Dataview 플러그인 활성화 필요.

## 최근 활동 (Recent Activity)

```dataview
TABLE type, status, updated
FROM "wiki"
WHERE updated
SORT updated DESC
LIMIT 15
```

## 개발 중 페이지 (Seed / Developing)

```dataview
LIST
FROM "wiki"
WHERE status = "seed" OR status = "developing"
SORT updated ASC
```

## 도메인별 페이지 분포

```dataview
TABLE length(rows) AS "페이지 수"
FROM "wiki"
WHERE type
GROUP BY type
SORT length(rows) DESC
```

## 엔티티 — 소스 누락

```dataview
LIST
FROM "wiki/entities"
WHERE !sources OR length(sources) = 0
```

## 프로젝트 (active)

```dataview
TABLE status, stack, repo
FROM "wiki/projects"
WHERE type = "project" AND status = "active"
```

## 논문

```dataview
TABLE year AS "연도", venue AS "발표"
FROM "wiki/papers"
WHERE type = "paper"
SORT year DESC
```

## 학습 노트

```dataview
LIST
FROM "wiki/learning"
WHERE type = "learning" OR type = "note"
SORT updated DESC
```

## 열린 질문 (Open Questions — draft)

```dataview
LIST
FROM "wiki/questions"
WHERE answer_quality = "draft"
SORT created DESC
```

## 최근 lint 보고서

```dataview
LIST
FROM "wiki/meta"
WHERE contains(file.name, "lint-report")
SORT file.name DESC
LIMIT 5
```

## 관련

- [[index]] — 마스터 카탈로그
- [[hot]] — 핫 캐시
- [[overview]] — 위키 개요
- [[lint-report-2026-04-27]] — 최근 헬스 체크 결과
