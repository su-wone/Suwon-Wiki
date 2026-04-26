---
type: entity
entity_type: tool
title: "TypeScript"
created: 2026-04-23
updated: 2026-04-23
tags:
  - entity
  - tool
  - language
status: seed
related:
  - "[[NestJS]]"
  - "[[Prisma]]"
  - "[[server-board]]"
---

# TypeScript

## 정체

JavaScript에 정적 타입을 더한 언어. Microsoft 개발.

## 어디에 쓰이나

- [[server-board]] — ^5.7.3, ESM (`"type": "module"`) 모드

## 본인 프로젝트 관련 설정

- `tsconfig.json` + `tsconfig.build.json` 분리
- `tsconfig-paths` 사용
- `baseUrl` 제거 (2026-04-21 결정) — 경로 모호성 제거
- ts-jest로 테스트 실행

## 공식

- Docs: https://www.typescriptlang.org/docs/
