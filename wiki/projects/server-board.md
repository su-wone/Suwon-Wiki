---
type: project
title: "server-board"
mode: B
status: active
stack:
  - "[[NestJS]]"
  - "[[Prisma]]"
  - "[[PostgreSQL]]"
  - "[[TypeScript]]"
repo: https://github.com/su-wone/Board-Server.git
branch_active: feat/frontend-integration
issue_prefix: VEASLY
started: 2026-03-25
created: 2026-04-23
updated: 2026-04-23
tags:
  - project
  - mode-b
  - backend
  - nestjs
  - prisma
related:
  - "[[fullstack-dev]]"
  - "[[projects/_index]]"
sources:
  - "[[server-board-repo]]"
---

# server-board

## 한 줄

Jira 스타일 보드 서비스의 백엔드. Sprints / Workflows / Cards / Epics를 관리하는 REST API.

## 목적

- 스프린트 단위로 카드를 워크플로 컬럼에 배치하고 이동시키는 보드 시스템
- 이슈 키 prefix: `VEASLY-###` (Jira 연동을 염두)
- 프론트엔드 통합 단계 진행 중 (현재 브랜치: `feat/frontend-integration`)

## 스택

| 레이어   | 기술                                     | 버전           |
| ----- | -------------------------------------- | ------------ |
| 런타임   | Node.js (ESM)                          | —            |
| 프레임워크 | [[NestJS]]                             | ^11.0.1      |
| ORM   | [[Prisma]]                             | ^7.5.0       |
| DB    | [[PostgreSQL]]                         | — (pg ^8.20) |
| 언어    | [[TypeScript]]                         | ^5.7.3       |
| 문서    | `@nestjs/swagger`                      | ^11.3.0      |
| 스토리지  | `@aws-sdk/client-s3`                   | ^3.1019      |
| 검증    | `class-validator`, `class-transformer` | —            |
| 테스트   | [[Jest]]                               | ^30          |

## 구조

```
src/
  app.module.ts
  main.ts
  board/
    board.module.ts
    cards/       — 카드 CRUD, 상세 조회, 필터
    epics/       — 에픽 모델, GET /epics
    sprints/     — 스프린트 조회 + goal 필드
    workflows/   — 워크플로 컬럼 (7단계 보드)
prisma/
  schema.prisma  — Users, Sprints, Workflows, Cards, Epics, Labels, Memos
  migrations/    — 6개 마이그레이션 (2026-03-25 ~ 2026-04-22)
  seed.ts        — 워크플로/에픽 시드 ([[tsx]] 실행)
```

## 도메인 모델 (Prisma)

- **Users** — 이름, 이메일, assignedCards / reportedCards 릴레이션
- **Sprints** — title, status(`PLANNED|IN_PROGRESS|DONE`), startDate, endDate, goal
- **Workflows** — title, order (7단계 컬럼)
- **Cards** — workflowId, sprintId, assigneeId, reporterId, parentId (sub-task용), type(`EPIC|STORY|TASK|SUB_TASK|BUG`), priority
- **Epics** — 최근 추가 (2026-04-22)
- **Labels**, **Memos**, **MemoImages** — 부가 엔티티

모든 모델에 `createdAt / updatedAt / deletedAt` (soft delete 지원).

## 주요 결정 (커밋 기반)

- 카드 키는 `VEASLY-` prefix 사용 (이슈 트래커 일관성)
- `key` 컬럼 제거 후 동적 생성 방식으로 전환 (2026-04-21)
- Workflow → Workflows 네이밍 일관성 (복수형)
- Memo/Upload 모듈 제거 (2026-03-이후) 후 CardType 확장
- 인덱스 + onDelete 정책 명시 (스키마 개선)
- Swagger API 문서화 + CORS 화이트리스트 적용

## 진행 상황

- ✅ 대시보드 스키마 (Users, Sprints, Workflow, Cards, Labels)
- ✅ Workflows / Sprints / Cards 조회 API
- ✅ POST /cards, GET /cards/:id
- ✅ Epic 모델 및 GET /epics
- ✅ Swagger DTO 응답 스키마 정리 (진행 중)
- 🔜 프론트엔드 통합

## 원본

- `.raw/B-github/server-board/` — README, package.json, git-log, tree 스냅샷 (2026-04-23 수집)

## 추적 갱신 절차

큰 변화가 생겼을 때:

```bash
cd /Users/admin/server-board
cp README.md package.json /Users/admin/wiki/.raw/B-github/server-board/
git log --oneline -50 > /Users/admin/wiki/.raw/B-github/server-board/git-log.txt
```

그 다음 이 페이지의 **진행 상황**, **도메인 모델**, **주요 결정** 섹션을 갱신.

## 관련

- [[fullstack-dev]] — 도메인 허브
- [[projects/_index]] — 전체 프로젝트 목록
- [[server-board-repo]] — 소스 스냅샷 요약

## 적용된 학습

- [[esm-import-rule]] — ESM 모드 import 경로 `.js` 규칙 (이 프로젝트 전체에 적용)
