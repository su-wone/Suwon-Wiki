---
type: concept
title: "Bottom-up Pose Estimation"
complexity: intermediate
domain: ai-research
aliases:
  - "상향식 포즈 추정"
created: 2026-04-23
updated: 2026-04-23
tags:
  - concept
  - computer-vision
  - pose-estimation
status: developing
related:
  - "[[concepts/_index]]"
  - "[[part-affinity-fields]]"
  - "[[multi-person-pose-estimation]]"
sources:
  - "[[cao-2017-openpose-paf-source]]"
---

# Bottom-up Pose Estimation

## 정의

이미지 전체에서 먼저 **모든 사람의 파트(키포인트)**를 검출한 뒤, 어느 파트가 어느 사람의 것인지 **연결(assembly)** 단계를 거치는 접근.

## 왜 중요한가

Top-down 접근(사람 먼저 검출 → 각각 포즈 추정)의 두 가지 약점을 우회한다.
1. 사람 검출기가 실패하면 복구 불가 (early commitment).
2. 런타임이 사람 수에 비례해 선형 증가.

Bottom-up은 CNN 추론을 한 번만 하고, 연결 단계의 계산만 사람 수에 의존한다.

## 핵심 아이디어

- 전 이미지에 대한 **파트 검출 맵** (보통 [[confidence-map]]) 예측.
- 파트 쌍 사이 **연관성 표현** (예: [[part-affinity-fields]], associative embedding, midpoint) 예측.
- **그래프 매칭**으로 파트를 사람별로 그루핑.

## 예시

OpenPose/PAF 파이프라인:
1. CNN → confidence map $\mathbf{S}$ + PAF $\mathbf{L}$
2. 각 파트 타입에 대해 NMS → 후보 집합 $\mathcal{D}_\mathcal{J}$
3. 인접 파트 쌍에 대해 PAF 선적분으로 연결 점수 계산
4. 트리 스켈레톤 기반 [[bipartite-matching]] → 사람 단위 골격 조립

## 언제 쓰는가 / 쓰지 않는가

- **쓰는 상황**: 사람 수 많음 (군중), 실시간 필요, 사람 검출 신뢰도 낮음.
- **쓰지 않는 상황**: 인원 적고 충분한 GPU + 최고 정확도 필요 → top-down (각 사람을 크롭·리스케일해 단일 포즈 추정기에 넣는 편이 작은 스케일에서 유리).

## 관련 개념

- [[multi-person-pose-estimation]]
- [[part-affinity-fields]]
- [[confidence-map]]
- [[bipartite-matching]]

## 소스

- [[cao-2017-openpose-paf-source]]
- [[cao-2017-openpose-paf]]
