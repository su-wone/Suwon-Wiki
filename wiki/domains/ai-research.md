---
type: domain
title: "AI 논문 리서치"
created: 2026-04-23
updated: 2026-04-23
tags:
  - domain
  - ai
  - research
  - papers
status: developing
related:
  - "[[index]]"
  - "[[overview]]"
  - "[[domains/_index]]"
  - "[[fullstack-dev]]"
  - "[[dev-notes]]"
sources:
---

# AI 논문 리서치

읽은 AI 논문을 요약하고, 핵심 주장을 추적하고, 방법론을 비교하는 도메인입니다.

## 이 도메인의 폴더

- **`wiki/papers/`** — 개별 논문 요약 페이지 (파일명은 논문 슬러그 또는 첫 저자-연도)

## 이 도메인의 소스 위치

- `.raw/E-research/` — PDF 논문, 외부 리서치 클립

## 논문 (1개)

- [[cao-2017-openpose-paf]] (2017, CVPR) — Part Affinity Fields로 bottom-up 멀티 퍼슨 2D 포즈 추정 실시간화

## 주요 연구 테마

- **컴퓨터 비전 / 포즈 추정**: [[multi-person-pose-estimation]], [[bottom-up-pose-estimation]], [[part-affinity-fields]]

## 핵심 저자

- [[Yaser-Sheikh]] (CMU → Meta Reality Labs) — 포즈 / 휴먼 캡처 그룹 리더
- [[Zhe-Cao]], [[Tomas-Simon]], [[Shih-En-Wei]] — OpenPose / PAF 공저자

## 방법론 비교

<!-- comparisons/ 에 논문 간 비교 페이지가 쌓이면 여기에 링크 -->

## 열린 질문 / 격차

<!-- 답을 찾지 못한 리서치 질문. wiki/questions/ 로 연결 -->

## 크로스 도메인 연결

- **풀스택 개발**: [[fullstack-dev]] — 논문 기법을 프로젝트에 적용한 사례
- **학습 노트**: [[dev-notes]] — 논문 개념 학습 메모

---

## 이 도메인을 키우는 방법

1. arXiv나 관심 논문 PDF를 `.raw/papers/`에 저장
2. `ingest .raw/papers/<paper>.pdf`
3. Claude가 논문 요약 페이지, 주요 저자(엔티티), 핵심 개념을 생성
4. 여러 논문 사이 모순이나 공통점 발견 시 `wiki/comparisons/`에 정리

### 권장 frontmatter 필드 (논문용)

- `year`, `authors`, `venue`, `key_claim`, `methodology`, `contradicts`, `supports`
