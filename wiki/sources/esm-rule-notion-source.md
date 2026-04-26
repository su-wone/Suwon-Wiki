---
type: source
title: "ESM 규칙 (Notion 학습 노트)"
source_type: note
author: "본인 (Notion 작성)"
date_published: 2026-04-21
url: ""
confidence: high
key_claims:
  - "TypeScript는 .ts를 직접 실행하지 못하고, tsc로 dist/*.js 컴파일 결과만 실행된다"
  - "ESM에서 import 경로는 런타임(컴파일 후)에 존재할 .js를 가리켜야 한다"
  - "TS는 import 문을 손대지 않는다 — 그래서 사용자가 직접 .js를 적어야 한다"
  - "CommonJS는 확장자 자동 추측이 있어 생략 가능하지만, ESM은 표준이라 정확한 경로 필수"
created: 2026-04-26
updated: 2026-04-26
tags:
  - source
  - mode-d
  - esm
  - typescript
status: developing
raw_file: ".raw/D-personal/notion-study/ESM.pdf"
related:
  - "[[sources/_index]]"
  - "[[esm-import-rule]]"
  - "[[esm-modules]]"
---

# ESM 규칙 (Notion 학습 노트)

## 요약

본인이 2026-04-21 Notion에 정리한 ESM 규칙 학습 노트. NestJS + TypeScript ESM 모드(`"type": "module"`)에서 import 경로에 `.js` 확장자를 써야 하는 이유를 정리. 핵심: 실행되는 파일은 컴파일된 `dist/*.js`이므로 import 경로도 런타임 기준으로 적어야 한다는 설명. CommonJS와의 대비로 ESM이 왜 명시적인지 보여줌.

## 핵심 인용

> "실행될 때 존재할 파일을 적어야 한다. .ts 는 컴파일 후 .js 가 되니, import는 .js 로."

> "TS의 철학: '타입만 추가하고 JS 코드는 손대지 않는다.' import 문은 JS 코드의 일부라서, TS가 컴파일할 때 그대로 둡니다."

> "ESM은 표준이라 그런 관용을 안 봐줌 — 정확한 경로 필수."

## 이 소스에서 만들어진 페이지

- [[esm-import-rule]] (학습 노트, 메인)
- [[esm-modules]] (개념)
- [[commonjs]] (개념)
- [[Node.js]] (엔티티)

## 내 생각

- [[server-board]] 백엔드가 NestJS 11 + TS 5.7 **ESM 모드**라서 이 규칙이 매일 부딪히는 실전 이슈
- 한 줄 멘탈 모델("실행될 파일을 적어라")이 좋음 — IDE 자동완성이 `.ts`를 제안할 때 헷갈림 방지
- 다음 후속: `tsconfig`의 `module`/`moduleResolution` 옵션이 어떻게 이 동작에 영향 주는지

## 원본 파일

`.raw/D-personal/notion-study/ESM.pdf`
