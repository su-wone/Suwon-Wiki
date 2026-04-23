---
type: concept
title: "Part Affinity Fields (PAF)"
complexity: intermediate
domain: ai-research
aliases:
  - "PAF"
  - "Part Affinity Field"
  - "파트 어피니티 필드"
created: 2026-04-23
updated: 2026-04-23
tags:
  - concept
  - computer-vision
  - pose-estimation
  - deep-learning
status: developing
related:
  - "[[concepts/_index]]"
  - "[[cao-2017-openpose-paf]]"
  - "[[bottom-up-pose-estimation]]"
  - "[[confidence-map]]"
  - "[[bipartite-matching]]"
sources:
  - "[[cao-2017-openpose-paf-source]]"
---

# Part Affinity Fields (PAF)

## 정의

이미지의 모든 픽셀 위치에 대해 **특정 림(limb)의 방향을 가리키는 2D 단위 벡터**를 저장하는 비모수 벡터 필드. 각 림 타입마다 하나씩 총 C개의 필드가 존재한다.

## 왜 중요한가

멀티 퍼슨 포즈 추정에서 **검출된 파트를 어느 사람의 것으로 연결할지** 결정해야 한다. PAF는 이 "연결 관계"를 공간에 분산된 벡터장으로 직접 학습 가능한 형태로 표현한다.

- **위치 + 방향**을 둘 다 인코딩 → 중점(midpoint) 기반 연결보다 false association이 훨씬 적다.
- **비모수**이므로 사람 수에 무관한 고정 크기 출력.
- CNN으로 예측 가능 → 엔드투엔드 학습.

## 핵심 아이디어

- 림 $c$가 사람 $k$의 파트 $j_1 \to j_2$를 연결할 때, 림 서포트 영역 안의 점 $\mathbf{p}$에서:
  $$\mathbf{L}_{c,k}^*(\mathbf{p}) = \begin{cases} \mathbf{v} & \text{if } \mathbf{p} \text{ on limb } c, k \\ \mathbf{0} & \text{otherwise} \end{cases}$$
  $\mathbf{v}$는 $j_1\to j_2$ 방향의 단위 벡터.
- 서포트는 "두 파트를 잇는 선분에서 거리 $\sigma_l$ 이내, 길이 $l_{c,k}$ 이내"인 사각형.
- 여러 사람의 같은 림이 겹치면 평균: $\mathbf{L}_c^*(\mathbf{p}) = \frac{1}{n_c(\mathbf{p})} \sum_k \mathbf{L}_{c,k}^*(\mathbf{p})$.
- 두 후보 파트 사이 연결 점수: 두 점을 잇는 선분을 따라 PAF를 선적분.
  $$E = \int_0^1 \mathbf{L}_c(\mathbf{p}(u)) \cdot \hat{\mathbf{d}}_{j_1 j_2}\, du$$
- 실제로는 균등 샘플링으로 합산 근사.

## 예시

오른팔 림(오른쪽 팔꿈치 → 오른쪽 손목)에 대해:
- 팔꿈치 후보 `(120, 200)`, 손목 후보 `(160, 240)`
- 선분을 10개 점으로 샘플링 → 각 점에서 PAF 벡터 읽기 → 연결 방향 $(40, 40)/\|\cdot\|$와 내적 → 평균
- 점수가 높으면 이 두 후보는 같은 사람의 팔로 연결.

## 언제 쓰는가 / 쓰지 않는가

- **쓰는 상황**: 멀티 인스턴스 키포인트 연결 일반 (사람 포즈, 동물, 손 관절). Bottom-up 파이프라인.
- **쓰지 않는 상황**: 단일 인스턴스(연결 모호성 없음), 또는 고해상도 3D 포즈(차원이 맞지 않음 — 대신 3D 확장이나 다른 표현 필요).

## 관련 개념

- [[confidence-map]] — 파트 위치 예측
- [[bottom-up-pose-estimation]] — 파이프라인 맥락
- [[bipartite-matching]] — PAF 점수를 엣지 가중치로 사용
- [[multi-person-pose-estimation]]

## 소스

- [[cao-2017-openpose-paf-source]]
- [[cao-2017-openpose-paf]]
