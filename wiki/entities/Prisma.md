---
type: entity
entity_type: tool
title: "Prisma"
created: 2026-04-23
updated: 2026-04-23
tags:
  - entity
  - tool
  - orm
  - database
related:
  - "[[PostgreSQL]]"
  - "[[TypeScript]]"
  - "[[server-board]]"
---

# Prisma

## 정체

TypeScript 우선 ORM. `schema.prisma` 선언으로 타입 안전한 클라이언트와 마이그레이션을 생성.

## 어디에 쓰이나

- [[server-board]] — v7.5 (`@prisma/client`, `@prisma/adapter-pg`)

## 본인 프로젝트에서 쓰는 기능

- `schema.prisma` 중앙 모델 정의 (Users, Sprints, Workflows, Cards, Epics, Labels)
- Migration (`prisma/migrations/`)
- `prisma.service.ts` + `prisma.module.ts`로 NestJS DI
- `generated/prisma` 커스텀 output 경로
- `tsx prisma/seed.ts`로 시딩
- Postgres adapter (`@prisma/adapter-pg`) 사용

## 관련 개념

- Soft delete: 모든 테이블에 `deletedAt` 컬럼 패턴
- `onDelete` 정책 명시 (카드-에픽-스프린트 릴레이션)

## 공식

- Docs: https://www.prisma.io/docs
