---
type: concept
title: "ESM (ECMAScript Modules)"
complexity: basic
domain: fullstack-dev
aliases:
  - "ECMAScript Modules"
  - "ES Modules"
  - "ESM"
created: 2026-04-26
updated: 2026-04-26
tags:
  - concept
  - javascript
  - typescript
  - module-system
status: seed
related:
  - "[[concepts/_index]]"
  - "[[commonjs]]"
  - "[[esm-import-rule]]"
  - "[[Node.js]]"
  - "[[TypeScript]]"
sources:
  - "[[esm-rule-notion-source]]"
---

# ESM (ECMAScript Modules)

## 정의

자바스크립트의 **표준 모듈 시스템** (ECMAScript 명세). `import`/`export` 구문을 사용하며, 브라우저와 Node.js가 공통으로 지원.

## 왜 중요한가

- 브라우저 ↔ Node.js 모듈 시스템 통일 (CommonJS는 Node 전용)
- 정적 분석 가능 → 트리 셰이킹, 비동기 로딩 최적화
- TS/Vite/Bun 등 현대 도구 체인의 기본값
- 단점: 명시적 경로 + 확장자 필요 → 학습 곡선 ([[esm-import-rule]] 참고)

## 핵심 아이디어

- **명시적 경로**: import 시 확장자 포함 (`.js`, `.mjs`) — 자동 추측 없음
- **`type: "module"`**: `package.json`에서 패키지 단위로 ESM 선언
- **정적 import**: 최상위 `import` 문은 정적 분석 가능 (`require`처럼 동적 위치 안 됨)
- **Top-level await** 지원
- **Live binding**: import한 값은 export 측 변경을 실시간 반영 (CommonJS는 복사본)

## 예시

```ts
// src/sprints.module.ts
import { SprintsService } from './sprints.service.js';
                              // ^ 컴파일 후 dist/sprints.service.js
export class SprintsModule {}
```

```json
// package.json
{
  "type": "module"
}
```

## 언제 쓰는가 / 쓰지 않는가

- **쓰는 상황**: 신규 Node 프로젝트, 브라우저 공유 코드, 트리 셰이킹 필요한 라이브러리
- **쓰지 않는 상황**: 레거시 CommonJS 패키지와 깊게 통합 (interop 비용), `__dirname` / `require` 의존이 많은 코드

## CommonJS와의 차이

| 항목 | ESM | [[commonjs]] |
|---|---|---|
| 구문 | `import` / `export` | `require` / `module.exports` |
| 확장자 | 필수 | 생략 가능 |
| 로딩 | 정적, 비동기 | 동적, 동기 |
| 표준 | 언어 표준 | Node.js 자체 |
| 브라우저 | ✅ | ❌ |

## 관련 개념

- [[commonjs]]
- [[esm-import-rule]] — TS + ESM에서 `.js` 확장자 규칙

## 소스

- [[esm-rule-notion-source]]
