---
type: entity
entity_type: tool
title: "Node.js"
role: "JavaScript 런타임"
first_mentioned: 2026-04-26
created: 2026-04-26
updated: 2026-04-26
tags:
  - entity
  - tool
  - runtime
  - javascript
status: seed
related:
  - "[[entities/_index]]"
  - "[[esm-modules]]"
  - "[[commonjs]]"
  - "[[esm-import-rule]]"
  - "[[NestJS]]"
  - "[[server-board]]"
sources:
  - "[[esm-rule-notion-source]]"
---

# Node.js

## 개요

V8 기반 서버사이드 JavaScript 런타임. [[server-board]] 백엔드의 실행 환경. ESM과 CommonJS 두 모듈 시스템을 모두 지원하지만 동작 방식이 다름.

## 핵심 사실

- `.ts` 파일을 **직접 실행하지 못함** → 반드시 `tsc` 등으로 컴파일된 `.js`만 실행
- 모듈 시스템: [[esm-modules]] (`"type": "module"`) 또는 [[commonjs]] (기본값)
- ESM에서는 **확장자 자동 추측 없음** → import 경로에 `.js` 명시 필수
- CommonJS에서는 자체 해석 로직으로 확장자 생략 가능

## 이 위키와의 관련성

- [[server-board]] 백엔드 런타임 (NestJS 11 + TS 5.7 ESM 모드)
- [[esm-import-rule]] 학습 노트의 핵심 주체 — "import 경로는 누가 보나? Node.js가 본다"
- [[NestJS]] / [[Prisma]] 등 모든 백엔드 도구의 기반

## 관련 소스

- [[esm-rule-notion-source]]
- [[server-board-repo]]

## 외부 링크

- 웹사이트: https://nodejs.org
- ESM 문서: https://nodejs.org/api/esm.html
