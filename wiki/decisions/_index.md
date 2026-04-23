---
type: meta
title: "기술 결정 인덱스 (ADR)"
created: 2026-04-23
updated: 2026-04-23
tags:
  - meta
  - index
  - decision
  - adr
domain: fullstack-dev
status: evergreen
related:
  - "[[index]]"
  - "[[fullstack-dev]]"
---

# 기술 결정 인덱스

Architecture Decision Records (ADR). 왜 이 기술을 선택했고 무엇을 거절했는지 기록합니다.

## 활성 결정 (Active)

<!-- 현재 유효한 결정 -->

## 대체됨 (Superseded)

<!-- 나중에 뒤집힌 결정. 배울 점 -->

## 검토 대기 (Pending Review)

<!-- 아직 확정 안 됨 -->

---

## ADR 페이지 만들기

`wiki/decisions/NNN-short-title.md` 형식 추천. 예: `001-choose-react-over-vue.md`

### 권장 frontmatter

```yaml
---
type: decision
title: "결정 제목"
status: active  # active|superseded|pending
adr_number: 001
date: 2026-04-23
context: "왜 이 결정이 필요했는가"
decision: "무엇을 선택했는가 (한 문장)"
alternatives:
  - "고려했던 대안 1"
  - "고려했던 대안 2"
consequences: "결과와 트레이드오프"
supersedes:
  - "[[옛 결정]]"
superseded_by:
  - "[[새 결정]]"
related_project: "[[프로젝트명]]"
---
```

### ADR 본문 구조

```markdown
## Context
문제와 배경

## Decision
내린 결정

## Alternatives Considered
- 대안 1: 왜 거절
- 대안 2: 왜 거절

## Consequences
- 긍정적
- 부정적
- 중립

## References
- [[관련 프로젝트]]
- [[관련 개념]]
```
