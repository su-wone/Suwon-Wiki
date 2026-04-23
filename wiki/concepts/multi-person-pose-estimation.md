---
type: concept
title: "Multi-Person 2D Pose Estimation"
complexity: intermediate
domain: ai-research
aliases:
  - "멀티 퍼슨 2D 포즈 추정"
  - "2D Pose Estimation"
created: 2026-04-23
updated: 2026-04-23
tags:
  - concept
  - computer-vision
  - pose-estimation
status: developing
related:
  - "[[concepts/_index]]"
  - "[[bottom-up-pose-estimation]]"
  - "[[part-affinity-fields]]"
  - "[[COCO-Dataset]]"
  - "[[MPII-Dataset]]"
sources:
  - "[[cao-2017-openpose-paf-source]]"
---

# Multi-Person 2D Pose Estimation

## 정의

한 이미지 안의 **여러 사람**에 대해 2D 해부학적 키포인트(목, 어깨, 팔꿈치, 손목 등)를 동시에 위치 추정하고, 어느 키포인트가 어느 사람의 것인지 연결하는 문제.

## 왜 중요한가

비디오 이해, 스포츠 분석, AR/VR, 감시, HCI 등에서 사람 행동을 인식하기 위한 기초. 단일 인스턴스 포즈 추정보다 훨씬 어렵다:
1. 사람 수를 모름
2. 상호작용/겹침/가림(occlusion)
3. 실시간 제약 (사람 수에 따른 스케일)

## 핵심 아이디어

### 접근 방식 두 갈래

- **Top-down**: 사람 검출 → 각 바운딩 박스 안에서 단일 퍼슨 포즈. 작은 스케일에 강함. 단점: 검출기 실패 민감, O(N) 런타임.
- **Bottom-up**: 파트 먼저, 그 다음 연결. 실시간 가능. 단점: 작은 스케일에서 약함 ([[cao-2017-openpose-paf]]의 AP^M 항목 참고).

### 공통 파이프라인 요소

- 백본 CNN ([[VGG-19]], ResNet 등)
- 헤드: [[confidence-map]] (+ [[part-affinity-fields]] / associative embedding / midpoint 등)
- 후처리: NMS, 매칭/그루핑 ([[bipartite-matching]])

## 예시

벤치마크:
- [[COCO-Dataset]] keypoints: AP (OKS 기반, 10 threshold)
- [[MPII-Dataset]] Multi-Person: mAP (PCKh)

## 언제 쓰는가 / 쓰지 않는가

- **쓰는 상황**: 사람 행동 분석, 군중 분석, 스포츠 트래킹.
- **쓰지 않는 상황**: 3D 포즈/메시가 필요하면 다른 문제(SMPL, VIBE 등).

## 관련 개념

- [[bottom-up-pose-estimation]]
- [[part-affinity-fields]]
- [[confidence-map]]
- [[bipartite-matching]]

## 소스

- [[cao-2017-openpose-paf-source]]
- [[cao-2017-openpose-paf]]
