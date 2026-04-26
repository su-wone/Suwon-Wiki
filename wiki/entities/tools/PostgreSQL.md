---
type: entity
entity_type: tool
title: "PostgreSQL"
created: 2026-04-23
updated: 2026-04-23
tags:
  - entity
  - tool
  - database
status: seed
related:
  - "[[Prisma]]"
  - "[[server-board]]"
---

# PostgreSQL

## 정체

오픈소스 관계형 DB.

## 어디에 쓰이나

- [[server-board]] — [[Prisma]] + `pg` 드라이버 (^8.20), `@prisma/adapter-pg`

## 관련 기능

- `@db.Timestamptz(3)` — 타임존 포함 타임스탬프 (밀리초 정밀도 3)
- Enum 타입 (SprintStatus, CardType, CardPriority)

## 공식

- Docs: https://www.postgresql.org/docs/
