---
type: meta
title: "Hot Cache"
created: 2026-04-23
updated: 2026-04-23T16:00:00
tags:
  - meta
  - hot-cache
status: evergreen
related:
  - "[[index]]"
  - "[[log]]"
  - "[[overview]]"
---

# 최근 컨텍스트

내비게이션: [[index]] | [[log]] | [[overview]]

## 마지막 업데이트
2026-04-23: 첫 논문 인제스트 완료 — [[cao-2017-openpose-paf]] (OpenPose 원조 논문). 16개 신규 페이지 생성.

## 볼트 상태
- **위치**: `/Users/admin/wiki/`
- **모드**: B + E + C + F 결합
- **도메인**: 풀스택 개발, AI 논문 리서치, 취업시장 분석, 개발 노트
- **인제스트된 소스**: 1
- **위키 페이지**: 16 + 4 도메인 허브 + 시드

## 최근 인제스트

### [[cao-2017-openpose-paf]] (2017, CVPR)
- **한 줄**: PAF로 bottom-up 멀티 퍼슨 2D 포즈 추정을 실시간으로 해결
- **저자**: [[Zhe-Cao]], [[Tomas-Simon]], [[Shih-En-Wei]], [[Yaser-Sheikh]] ([[CMU-Robotics-Institute]])
- **출력 도구**: [[OpenPose]] (오픈소스)
- **벤치마크**: COCO 2016 keypoints 1위 (60.5 AP), MPII multi-person 75.6 mAP, 19명 비디오 8.8 fps
- **핵심 개념**: [[part-affinity-fields]], [[confidence-map]], [[bottom-up-pose-estimation]], [[bipartite-matching]]

## 활성 영역

### 1. 풀스택 개발 ([[fullstack-dev]])
- 본인 프로젝트 코드 추적
- 아키텍처 결정 (ADR)
- 기술 스택 진화

### 2. AI 논문 리서치 ([[ai-research]]) ← **방금 첫 논문 추가**
- 논문 1개: [[cao-2017-openpose-paf]]
- 활성 테마: 컴퓨터 비전 / 포즈 추정
- 다음 후보: 후속 OpenPose 확장 논문, 또는 다른 비전 논문 비교

### 3. 취업시장 분석 ([[job-market]])
- 회사 프로필
- JD 패턴, 요구 스택

### 4. 개발 노트 ([[dev-notes]])
- 새 개념 학습
- 강의/튜토리얼 시사점

## 다음 액션

1. **OpenPose를 본인 프로젝트에 적용**해보고 [[fullstack-dev]] 또는 [[learning]]에 노트
2. **비교 페이지 후보**: PAF vs Associative Embedding, OpenPose vs HRNet, bottom-up vs top-down
3. **다른 소스**: 두 번째 인제스트로 비교 자료 만들기

## 열린 질문
- 작은 스케일에서 bottom-up이 약한 문제(AP^M 부진)를 후속 연구가 어떻게 풀었는지
- PAF 그리디 파스의 O(n²) 비용을 더 줄일 수 있는지
- 본인 프로젝트(어떤 영역?)에 포즈 추정이 어떻게 쓰일 수 있는지

## 스타일 선호
- 한국어로 작성 (본문, 섹션 제목, 안내)
- frontmatter 키와 위키링크 타깃은 영어 유지
