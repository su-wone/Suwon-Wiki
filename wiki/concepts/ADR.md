---
type: concept
title: "ADR"
aliases:
  - "Architecture Decision Record"
created: 2026-04-27
updated: 2026-04-27
tags:
  - concept
  - software-engineering
  - decision
  - documentation
status: seed
related:
  - "[[decisions/_index]]"
  - "[[fullstack-dev]]"
---

# ADR (Architecture Decision Record)

## 정의

아키텍처/기술 결정을 **결정 시점의 맥락과 함께** 기록하는 짧은 문서. "왜 이걸 골랐는가"를 미래의 자신과 동료에게 남기기 위함.

## 표준 구조 (Michael Nygard 형식)

```
# Title (결정의 명사구)
## Status        — proposed | accepted | superseded | deprecated
## Context       — 이 결정이 필요했던 상황, 제약
## Decision      — 무엇을 정했는가
## Consequences  — 따라오는 트레이드오프 (긍정/부정 모두)
```

## 이 위키에서

- 위치: `wiki/decisions/`
- 트리거: `/save decision [이름]` 또는 직접 작성
- 인덱스: [[decisions/_index]]

## ADR가 가치 있어지는 순간

결정 후 6개월~몇 년이 지나서 누군가 "왜 이렇게 했지?"라고 물을 때. **현재의 코드는 결정의 결과만 보여주지, 이유는 잃어버리기 쉽다.**

## server-board에서 후보 ADR

[[hot]]의 다음 액션에 정리된 결정 후보들 — `/save decision`으로 기록 가능:

- `key` 컬럼 제거 후 동적 생성 (2026-04-21)
- `tsconfig` `baseUrl` 제거
- Memo/Upload 모듈 제거 + CardType 확장
- ESM 모드 채택 (TypeScript 5.7 + `"type": "module"`)

## 외부 참고

- Michael Nygard, *Documenting Architecture Decisions* (2011)
- ADR Tools: https://github.com/npryce/adr-tools

## 관련

- [[decisions/_index]] — 본 위키의 ADR 목록
- [[fullstack-dev]] — 풀스택 도메인 (ADR 주 소비자)
- [[wiki-운영-매뉴얼]]
