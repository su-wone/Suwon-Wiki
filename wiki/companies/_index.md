---
type: meta
title: "회사 인덱스"
created: 2026-04-23
updated: 2026-04-23
tags:
  - meta
  - index
  - company
domain: job-market
status: evergreen
related:
  - "[[index]]"
  - "[[job-market]]"
---

# 회사 인덱스

취업시장 분석 대상 회사들.

## 지원 상태별

### 관심 (Interested)
<!-- JD 읽고 관심 생긴 회사 -->

### 지원 (Applied)
<!-- 지원서 제출 -->

### 면접 진행 (Interviewing)
<!-- 1차~최종 -->

### 오퍼 (Offered)
<!-- 오퍼 받음 -->

### 거절됨 (Rejected)
<!-- 불합격 또는 직접 거절 -->

## 규모별

### 스타트업 (<50명)
<!-- -->

### 중견 (50-500명)
<!-- -->

### 대기업 (500+명)
<!-- -->

## 산업별

### AI / ML
<!-- -->

### SaaS
<!-- -->

### 핀테크
<!-- -->

### 기타
<!-- -->

---

## 회사 페이지 만들기

```
ingest .raw/jobs/<회사-직무>.md
```

### 권장 frontmatter

```yaml
---
type: company
title: "회사명"
company_size: "100-500"
industry: "AI"
location: "서울"
stack:
  - "[[React]]"
  - "[[Python]]"
salary_range: "8000-12000만"
application_status: interested  # interested|applied|interview|offered|rejected
applied_date:
---
```
