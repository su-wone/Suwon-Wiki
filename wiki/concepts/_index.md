---
type: meta
title: "개념 인덱스"
created: 2026-04-23
updated: 2026-04-26
tags:
  - meta
  - index
  - concept
status: evergreen
related:
  - "[[index]]"
  - "[[entities/_index]]"
---

# 개념 인덱스

모든 도메인이 공유하는 개념 페이지. 아이디어, 패턴, 프레임워크, 알고리즘.

이 폴더의 페이지는 **도메인 중립**입니다. AI 논문에서 배운 개념이든 프론트엔드 패턴이든 여기에 옵니다.

## 프론트엔드

<!-- React 패턴, 상태 관리, 렌더링 등 -->

## 백엔드

- [[esm-modules]] — ECMAScript Modules 표준
- [[commonjs]] — Node.js 전통 모듈 시스템

## 데이터베이스

<!-- 인덱싱, 트랜잭션, 복제 등 -->

## 인프라 / 분산 시스템

<!-- 로드 밸런싱, 컨테이너, 오케스트레이션 -->

## AI / ML

<!-- RAG, 에이전트 루프, 임베딩, 정렬 -->

## 알고리즘 / CS 기초

<!-- 자료구조, 복잡도, 패러다임 -->

## 소프트웨어 엔지니어링

<!-- 디자인 패턴, 테스트 전략, 리팩토링 -->

## 커리어 / 소프트 스킬

<!-- 시스템 디자인 면접, 협업, 기술 리더십 -->

---

## 개념 페이지 만들기

인제스트 시 Claude가 자동 생성합니다. 수동 생성도 가능:

```
/save
(대화를 개념 노트로 정리)
```

### 권장 frontmatter

```yaml
---
type: concept
title: "개념 이름"
complexity: intermediate  # basic|intermediate|advanced
domain: ai  # 주요 도메인 태그
aliases:
  - "동의어 1"
  - "Alternative Name"
status: developing  # seed|developing|mature|evergreen
---
```
