---
type: entity
entity_type: tool
title: "Jest"
created: 2026-04-27
updated: 2026-04-27
tags:
  - entity
  - tool
  - testing
  - javascript
status: seed
related:
  - "[[TypeScript]]"
  - "[[NestJS]]"
  - "[[server-board]]"
---

# Jest

## 정체

Meta(구 Facebook)가 만든 JavaScript/TypeScript 테스트 프레임워크. Node.js 백엔드 테스트의 사실상 표준.

## 어디에 쓰이나

- [[server-board]] — v30 (`jest`, `@types/jest`, `ts-jest` ^29, `supertest` ^7)

## 본인 프로젝트 설정

- `package.json` `jest` 섹션에 인라인 설정 (`rootDir: src`, `testRegex: .*\.spec\.ts$`)
- `ts-jest` 트랜스폼으로 TS 직접 실행
- e2e는 `test/jest-e2e.json` 별도 설정
- 스크립트: `npm run test`, `test:watch`, `test:cov`, `test:e2e`

## 관련

- [[TypeScript]] — ts-jest로 TS 트랜스폼
- [[NestJS]] — `@nestjs/testing`이 Jest 위에서 동작

## 공식

- Docs: https://jestjs.io
