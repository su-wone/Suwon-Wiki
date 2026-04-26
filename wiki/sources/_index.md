---
type: meta
title: "소스 인덱스"
created: 2026-04-23
updated: 2026-04-26
tags:
  - meta
  - index
  - source
status: evergreen
related:
  - "[[index]]"
  - "[[log]]"
---

# 소스 인덱스

`.raw/`에서 인제스트된 원시 소스의 요약 페이지들. 각 원시 파일마다 하나의 요약 페이지.

## 기사 (Articles)

<!-- .raw/articles/ 출처 -->

## 논문 (Papers)

- [[cao-2017-openpose-paf-source]] — Cao et al., CVPR 2017 — Part Affinity Fields 기반 멀티 퍼슨 2D 포즈 추정

## 트랜스크립트 (Transcripts)

<!-- .raw/transcripts/ 출처 -->

## 개인 노트 (Personal Notes)

- [[esm-rule-notion-source]] — ESM 규칙 (Notion 학습 노트, 2026-04-21) — `.raw/D-personal/notion-study/ESM.pdf`

## 채용 공고 (Jobs)

<!-- .raw/jobs/ 출처 -->

## 코드 (Code)

<!-- .raw/code/ 출처 -->

---

## 소스 페이지 포맷

인제스트 시 Claude가 각 원시 파일당 1개의 요약 페이지를 `wiki/sources/`에 만듭니다.

### 권장 frontmatter

```yaml
---
type: source
title: "소스 제목"
source_type: article  # article|paper|transcript|job|code
author: "저자"
date_published: 2026-04-23
url: https://...
confidence: high  # high|medium|low
key_claims:
  - "주장 1"
  - "주장 2"
raw_file: ".raw/articles/source.md"
---
```
