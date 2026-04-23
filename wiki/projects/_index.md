---
type: meta
title: "프로젝트 인덱스"
created: 2026-04-23
updated: 2026-04-23
tags:
  - meta
  - index
  - project
domain: fullstack-dev
status: evergreen
related:
  - "[[index]]"
  - "[[fullstack-dev]]"
---

# 프로젝트 인덱스

본인의 풀스택 개발 프로젝트.

## 진행 중 (Active)

<!-- 현재 개발 중 -->

## 유지 (Maintenance)

<!-- 배포됨, 유지보수만 -->

## 보류 (Archived)

<!-- 중단/완료된 프로젝트 -->

## 아이디어 단계 (Ideas)

<!-- 아직 시작 안 한 것 -->

---

## 프로젝트 페이지 만들기

```
ingest [README 또는 프로젝트 설명 파일]
```

또는 수동으로:

```
/save
(진행 중 대화를 프로젝트 노트로 정리)
```

### 권장 frontmatter

```yaml
---
type: project
title: "프로젝트명"
status: active  # active|maintenance|archived|idea
stack: ["[[React]]", "[[FastAPI]]", "[[PostgreSQL]]"]
repo: https://github.com/...
started: 2026-04-23
---
```
