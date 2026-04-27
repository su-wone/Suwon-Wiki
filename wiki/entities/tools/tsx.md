---
type: entity
entity_type: tool
title: "tsx"
created: 2026-04-27
updated: 2026-04-27
tags:
  - entity
  - tool
  - typescript
  - runtime
status: seed
related:
  - "[[TypeScript]]"
  - "[[Node.js]]"
  - "[[server-board]]"
  - "[[esm-import-rule]]"
---

# tsx

## 정체

TypeScript를 **별도 빌드 단계 없이** Node.js에서 바로 실행하는 CLI/로더. `ts-node`의 후속/대안.

## 어디에 쓰이나

- [[server-board]] — v4.21
  - `npm run db:seed` → `tsx prisma/seed.ts` (시드 스크립트 실행)
  - dev tooling 일회성 실행

## ts-node와의 차이

- esbuild 기반 → 훨씬 빠름
- ESM 환경에서 `ts-node`보다 매끄러움 ([[esm-import-rule]] 참고)
- `--experimental-loader` 같은 flag 자동 처리

## 관련 멘탈 모델

[[esm-import-rule]]에서: TS 소스를 `.ts`로 import해도 실행 시점엔 컴파일된 `.js`가 존재해야 한다는 규칙. 단, **tsx는 트랜스파일하면서 실행하므로** 빌드 결과물 없이도 동작 — 그래서 seed/스크립트에 적합.

## 관련

- [[TypeScript]] — 트랜스파일 대상 언어
- [[Node.js]] — 실행 런타임
- [[esm-import-rule]] — ESM에서의 모듈 해석 규칙

## 공식

- GitHub: https://github.com/privatenumber/tsx
