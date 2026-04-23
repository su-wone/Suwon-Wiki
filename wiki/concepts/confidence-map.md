---
type: concept
title: "Confidence Map (Heatmap)"
complexity: basic
domain: ai-research
aliases:
  - "Heatmap"
  - "Part Confidence Map"
  - "히트맵"
created: 2026-04-23
updated: 2026-04-23
tags:
  - concept
  - computer-vision
  - deep-learning
status: developing
related:
  - "[[concepts/_index]]"
  - "[[part-affinity-fields]]"
  - "[[bottom-up-pose-estimation]]"
sources:
  - "[[cao-2017-openpose-paf-source]]"
---

# Confidence Map (Heatmap)

## 정의

각 픽셀 위치에 **특정 파트가 존재할 확률(또는 확신도)**를 담는 2D 스칼라 맵. 파트 타입 개수 $J$에 대해 $J$개의 채널이 출력된다.

## 왜 중요한가

키포인트의 좌표를 직접 회귀(regression)하는 대신, 공간적으로 분산된 표현을 학습하면 CNN이 다루기 쉽고 **멀티 모달**(여러 후보가 공존)을 자연스럽게 표현할 수 있다. 멀티 퍼슨에서는 같은 파트 타입의 여러 피크가 여러 사람에 대응한다.

## 핵심 아이디어

- 정답 맵은 각 사람 $k$의 파트 $j$에 대해 가우시안 범프:
  $$\mathbf{S}_{j,k}^*(\mathbf{p}) = \exp\left(-\frac{\|\mathbf{p}-\mathbf{x}_{j,k}\|^2}{\sigma^2}\right)$$
- 여러 사람은 **평균이 아닌 max**로 합성: $\mathbf{S}_j^*(\mathbf{p}) = \max_k \mathbf{S}_{j,k}^*(\mathbf{p})$
  (가까운 피크들의 분리를 유지하기 위함.)
- 테스트 시 **NMS(non-maximum suppression)**로 파트 후보 좌표 추출.
- 학습 손실은 마스킹된 L2: 누락 어노테이션 영역은 페널티 면제.

## 예시

오른쪽 팔꿈치 채널 맵에서 두 사람이 있으면 피크 2개, 각 피크 주변에 가우시안 블롭.
NMS → 로컬 맥시마만 남음 → `(120, 200), (250, 180)` 같은 후보 좌표.

## 언제 쓰는가 / 쓰지 않는가

- **쓰는 상황**: 키포인트 검출(포즈, 얼굴 랜드마크, 손), 객체 중심 예측, 세그멘테이션 전단계.
- **쓰지 않는 상황**: 저해상도 출력으로 충분한 단일 회귀 문제, 좌표를 직접 미분해야 하는 경우(이 때는 soft-argmax 필요).

## 관련 개념

- [[part-affinity-fields]]
- [[bottom-up-pose-estimation]]

## 소스

- [[cao-2017-openpose-paf-source]]
- [[cao-2017-openpose-paf]]
