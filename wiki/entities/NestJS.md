---
type: entity
entity_type: tool
title: "NestJS"
created: 2026-04-23
updated: 2026-04-23
tags:
  - entity
  - tool
  - framework
  - backend
  - typescript
related:
  - "[[TypeScript]]"
  - "[[server-board]]"
---

# NestJS

## 정체

TypeScript 기반 Node.js 서버 프레임워크. 데코레이터 + DI 컨테이너 + 모듈 구조로 Angular 스타일 백엔드를 만든다.

## 어디에 쓰이나

- [[server-board]] — v11 (`@nestjs/common`, `@nestjs/core`, `@nestjs/platform-express`)

## 본인 프로젝트에서 쓰는 기능

- 모듈 시스템 (`*.module.ts`)
- Controller + Service 분리
- `class-validator` / `class-transformer`로 DTO 검증
- `@nestjs/swagger`로 API 문서화
- 테스트는 `@nestjs/testing`

## 공식

- Docs: https://docs.nestjs.com
