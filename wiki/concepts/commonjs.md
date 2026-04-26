---
type: concept
title: "CommonJS"
complexity: basic
domain: fullstack-dev
aliases:
  - "CJS"
created: 2026-04-26
updated: 2026-04-26
tags:
  - concept
  - javascript
  - module-system
  - legacy
status: seed
related:
  - "[[concepts/_index]]"
  - "[[esm-modules]]"
  - "[[Node.js]]"
sources:
  - "[[esm-rule-notion-source]]"
---

# CommonJS

## 정의

Node.js가 전통적으로 써온 **모듈 시스템**. `require()`로 가져오고 `module.exports`로 내보냄. 언어 표준이 아닌 Node.js 자체 명세.

## 왜 중요한가

- 2009년 등장 이래 npm 생태계 대부분의 기반
- ESM 등장 이후에도 수많은 레거시 패키지가 CommonJS
- ESM과의 interop 이해가 현대 Node 개발의 필수 지식

## 핵심 아이디어

- **`require()`**: 동기, 동적. 파일 어디서나 호출 가능
- **확장자 자동 추측**: `require('./foo')` → `./foo.js`, `./foo/index.js` 순으로 탐색
- **`module.exports`**: 객체 통째로 export
- **값 복사**: import한 값은 export 시점의 스냅샷 (live binding 아님)
- **Node.js 자체 로직**: 언어 표준이 아니라 Node가 직접 해석

## 예시

```js
// CommonJS
const { SprintsService } = require('./sprints.service');
                                  // ^ 확장자 생략 OK

module.exports = { SprintsService };
```

## 언제 쓰는가 / 쓰지 않는가

- **쓰는 상황**: 레거시 코드 유지보수, CommonJS-only 라이브러리, `__dirname` / 동적 require 필요
- **쓰지 않는 상황**: 신규 프로젝트, 브라우저 공유 코드, 트리 셰이킹 필요

## ESM과의 대비

[[esm-modules]]와 비교 표 참고. 핵심: ESM은 **표준이라 관용을 안 봐줌** → CommonJS의 확장자 생략 같은 편의가 사라지고 정확한 경로 필요 ([[esm-import-rule]]).

## 관련 개념

- [[esm-modules]]
- [[esm-import-rule]]

## 소스

- [[esm-rule-notion-source]]
