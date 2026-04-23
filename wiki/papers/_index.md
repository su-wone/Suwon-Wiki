---
type: meta
title: "논문 인덱스"
created: 2026-04-23
updated: 2026-04-23
tags:
  - meta
  - index
  - paper
domain: ai-research
status: evergreen
related:
  - "[[index]]"
  - "[[ai-research]]"
---

# 논문 인덱스

AI/ML 논문 요약 페이지.

## 연도별

### 2026
<!-- 올해 논문 -->

### 2025
<!-- -->

### 이전

- 2017 — [[cao-2017-openpose-paf]] — Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields (Cao et al., CVPR)

## 주제별

### 언어 모델 (LLMs)
<!-- -->

### 에이전트 / 툴 사용
<!-- -->

### RAG / 검색
<!-- -->

### 정렬 / 안전
<!-- -->

### 기타
<!-- -->

---

## 논문 페이지 만들기

```
ingest .raw/papers/<paper>.pdf
```

### 권장 frontmatter

```yaml
---
type: paper
title: "논문 제목"
year: 2026
authors: ["First Author", "Second Author"]
venue: "NeurIPS"
key_claim: "핵심 주장 한 문장"
methodology: "간단한 방법론"
contradicts:
  - "[[다른 논문]]"
supports:
  - "[[관련 논문]]"
url: https://arxiv.org/...
---
```
