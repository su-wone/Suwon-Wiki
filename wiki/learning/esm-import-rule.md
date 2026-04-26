---
type: personal
title: "ESM import 규칙 — 실행될 파일을 적어라"
mode: D
area: learning
priority: 4
target_date: ""
progress: 60
created: 2026-04-26
updated: 2026-04-26
tags:
  - personal
  - mode-d
  - learning
  - esm
  - typescript
  - nestjs
status: developing
related:
  - "[[index]]"
  - "[[dev-notes]]"
  - "[[learning/_index]]"
  - "[[esm-modules]]"
  - "[[commonjs]]"
  - "[[server-board]]"
sources:
  - "[[esm-rule-notion-source]]"
---

# ESM import 규칙 — 실행될 파일을 적어라

## 무엇 / 왜

NestJS + TypeScript ESM 모드(`"type": "module"`)에서 왜 `import { X } from './x.js'`처럼 **`.js` 확장자**를 쓰는지에 대한 학습 노트. 본인 프로젝트 [[server-board]]가 ESM이라 매번 부딪히는 실전 이슈.

## 핵심 내용

### 한 문장 요약

> 실행될 때 존재할 파일을 적어야 한다. `.ts`는 컴파일 후 `.js`가 되니, import는 `.js`로.

### 왜 그런가 — 4단계 멘탈 모델

1. **TypeScript는 그대로 안 돌아간다**
   - Node.js는 `.ts`를 직접 실행 못함 → `tsc`로 컴파일된 `dist/*.js`를 실행
   - 즉 **실제 돌아가는 파일은 `dist/*.js`**

2. **import 경로는 누가 보나? — 런타임(Node.js)**
   - Node.js는 `dist/`에서 동작 → 거기 기준으로 경로 해석
   - `'./sprints.service.js'` → 런타임엔 `dist/board/sprints/sprints.service.js`를 가리킴

3. **TS는 import 문을 손대지 않는다**
   - TS 철학: "타입만 추가하고 JS 코드는 손대지 않는다"
   - `import` 문은 JS 코드의 일부 → 컴파일 후에도 **그대로 남음**
   - 그래서 **사용자가 직접 `.js`로 적어야** 컴파일 후 정확한 경로가 됨
   - `.ts`로 쓰면 → 컴파일 후 `.ts`로 남아서 → Node.js가 못 찾고 에러

4. **CommonJS는 왜 다른가**
   - `const x = require('./sprints.service')` — 확장자 생략 OK
   - CommonJS는 Node.js 자체 해석 로직(확장자 자동 추측)이 있음
   - **ESM은 표준** → 그런 관용을 안 봐줌, 정확한 경로 필수

## 적용

### [[server-board]]
- `package.json`에 `"type": "module"` 설정됨
- `tsconfig` 모듈 시스템: ESM
- 모든 모듈 import에서 `.js` 확장자 필수
  ```ts
  // src/board/sprints/sprints.module.ts
  import { SprintsService } from './sprints.service.js';
  ```
- IDE 자동완성이 `.ts`를 제안할 때 주의 — 무조건 `.js`로

### 실전 함정

- `import './foo'` (확장자 생략) → ERR_MODULE_NOT_FOUND
- `import './foo.ts'` → 컴파일 후에도 `.ts`로 남아 ERR_MODULE_NOT_FOUND
- VS Code 자동완성이 종종 `.ts`를 제안 → 수동으로 `.js`로 바꿔야 함

## 액션

- [ ] `tsconfig`의 `module` / `moduleResolution` 옵션이 ESM 동작에 미치는 영향 정리
- [ ] `tsconfig-paths` + ESM 조합에서 path alias가 어떻게 풀리는지 확인 ([[server-board]] `baseUrl` 제거 결정과 연결)
- [ ] Node.js의 `--experimental-strip-types`로 `.ts` 직접 실행하면 이 규칙이 어떻게 바뀌나 조사
- [ ] `tsx` / `ts-node` ESM 로더가 우회해주는 범위 정리

## 관련 사람 / 자료

- [[esm-rule-notion-source]] — 본인 Notion 노트 (2026-04-21)

## 진행 메모

- 2026-04-21: Notion에 1차 정리 (PDF로 export → `.raw/D-personal/notion-study/ESM.pdf`)
- 2026-04-26: 위키로 인제스트 — D-personal 모드 첫 학습 노트

## 소스
- [[esm-rule-notion-source]]

## 관련 개념 / 엔티티
- [[esm-modules]] — ECMAScript Modules 표준
- [[commonjs]] — 대비되는 모듈 시스템
- [[TypeScript]] — 타입만 추가, JS 코드 미수정 철학
- [[Node.js]] — 런타임
- [[NestJS]] — ESM 모드 사용 프레임워크
