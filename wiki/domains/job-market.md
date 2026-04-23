---
type: domain
title: "취업시장 분석"
created: 2026-04-23
updated: 2026-04-23
tags:
  - domain
  - job-market
  - career
status: developing
related:
  - "[[index]]"
  - "[[overview]]"
  - "[[domains/_index]]"
  - "[[fullstack-dev]]"
sources:
---

# 취업시장 분석

회사, 채용 공고, 연봉, 요구 스택 패턴을 체계적으로 추적하는 도메인입니다.

## 이 도메인의 폴더

- **`wiki/companies/`** — 회사별 프로필 페이지 (회사명 슬러그 파일)

## 이 도메인의 소스 위치

- `.raw/jobs/` — 채용 공고 (JD), 회사 테크 블로그 포스트
- `.raw/articles/` — 연봉 리포트, 산업 분석

## 회사 (0개)

<!-- 첫 회사 인제스트 후 여기 나열 -->

## 자주 요구되는 스택

<!-- JD 인제스트가 쌓이면서 자연 발현 -->

## 연봉 / 보상 트렌드

<!-- 데이터 포인트 누적 -->

## 인터뷰 패턴

<!-- 알고리즘, 시스템 디자인, 컬처 핏 등 패턴 -->

## 핵심 채용 담당자 / 연락처

<!-- wiki/entities/ 의 person 페이지로 연결 -->

## 회사 비교

<!-- comparisons/ 에 연봉/스택/문화 비교 페이지 -->

## 크로스 도메인 연결

- **풀스택 개발**: [[fullstack-dev]] — 본인 스택과 시장 요구 매칭
- **개발 노트**: [[dev-notes]] — 면접에서 자주 나오는 주제 학습 추적

---

## 이 도메인을 키우는 방법

1. 관심 JD 페이지를 복사해 `.raw/jobs/<회사-직무.md>`로 저장
2. `ingest .raw/jobs/<회사-직무.md>`
3. Claude가 회사 페이지, 요구 스택(엔티티), 연봉/위치를 추출
4. 본인 스택과 매칭 분석은 [[fullstack-dev]]와 연결

### 권장 frontmatter 필드 (회사용)

- `company_size`, `industry`, `location`, `stack` (위키링크 목록), `salary_range`, `application_status` (not-applied|applied|interview|offered|rejected)
