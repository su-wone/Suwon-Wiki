---
type: paper
title: "Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields"
year: 2017
authors:
  - "Zhe Cao"
  - "Tomas Simon"
  - "Shih-En Wei"
  - "Yaser Sheikh"
venue: "CVPR 2017"
key_claim: "Part Affinity Fields로 bottom-up 멀티 퍼슨 2D 포즈 추정을 실시간으로 해결"
methodology: "Two-branch multi-stage CNN으로 confidence map과 PAF를 동시에 예측 → 그리디 bipartite matching으로 파싱"
url: https://arxiv.org/abs/1611.08050
contradicts: []
supports:
  - "[[bottom-up-pose-estimation]]"
created: 2026-04-23
updated: 2026-04-23
tags:
  - paper
  - computer-vision
  - pose-estimation
  - deep-learning
status: developing
related:
  - "[[papers/_index]]"
  - "[[ai-research]]"
  - "[[cao-2017-openpose-paf-source]]"
  - "[[OpenPose]]"
  - "[[part-affinity-fields]]"
sources:
  - "[[cao-2017-openpose-paf-source]]"
---

# Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields

> [!key-insight] 한 줄 요약
> 림(limb)을 "방향이 있는 2D 벡터 필드"로 표현하면, bottom-up 접근이면서도 top-down 수준의 정확도와 **사람 수에 무관한 실시간 성능**을 동시에 얻을 수 있다.

## 메타
- **저자**: [[Zhe-Cao]], [[Tomas-Simon]], [[Shih-En-Wei]], [[Yaser-Sheikh]] ([[CMU-Robotics-Institute]])
- **발표**: CVPR 2017 (arXiv v2: 2017-04-14, arXiv:1611.08050)
- **코드**: [[OpenPose]] (공식 릴리스)
- **영상 결과**: https://youtu.be/pW6nZXeWlGM

## 해결하려는 문제

이미지 안의 **알 수 없는 수**의 사람들에 대해 2D 해부학적 키포인트(파트)를 동시에 검출·연결하는 문제.

전통적 접근의 한계:
- **Top-down**: 사람 검출기 → 단일 포즈 추정기. 검출기가 실패하면 복구 불가. 런타임이 사람 수에 비례.
- **Bottom-up(이전 SOTA: Deepcut, DeeperCut)**: 전 이미지 파트 검출 후 매칭. 풀리 커넥티드 그래프 위의 정수선형계획(NP-hard) → 이미지당 수 분.

## 핵심 아이디어: Part Affinity Fields (PAF)

- 각 **림 타입 c**마다 이미지 전체 크기의 2D 벡터 필드 $\mathbf{L}_c \in \mathbb{R}^{w\times h \times 2}$를 예측.
- 림의 서포트 영역(두 파트 사이 직사각형 밴드) 안의 픽셀은 **시작 파트 → 끝 파트 방향의 단위 벡터**를 값으로 가짐. 바깥은 0.
- 여러 사람의 림이 겹치면 평균. 정답은 $\mathbf{L}_c^*(\mathbf{p}) = \frac{1}{n_c(\mathbf{p})} \sum_k \mathbf{L}_{c,k}^*(\mathbf{p})$.
- 두 후보 파트 $\mathbf{d}_{j_1}, \mathbf{d}_{j_2}$의 연결 점수는 후보를 잇는 선분을 따라 PAF를 선적분:
  $$E = \int_{u=0}^{u=1} \mathbf{L}_c(\mathbf{p}(u)) \cdot \frac{\mathbf{d}_{j_2}-\mathbf{d}_{j_1}}{\|\mathbf{d}_{j_2}-\mathbf{d}_{j_1}\|_2}\, du$$
- 중점(midpoint) 표현(림의 중간에 가상의 "연결점"을 추가로 검출하는 대안)과 달리 **위치 + 방향**을 모두 인코딩해 밀집된 군중에서도 false association이 덜 생김.

> [!note] 왜 중점 표현이 실패하는가
> 파트 후보 군에서 연결의 **존재**만 보면, 두 사람의 팔이 교차했을 때 잘못된 대각선 연결도 중점 조건을 만족한다. PAF는 **방향**까지 보기 때문에 반대 방향 연결을 자동으로 거른다.

## 네트워크 아키텍처

- 입력: $w\times h$ 컬러 이미지.
- 백본: [[VGG-19]] 앞쪽 10 레이어(파인튜닝)로 피처 $\mathbf{F}$ 생성.
- 두 개의 브랜치로 분기:
  - **Branch 1**: confidence map $\mathbf{S}^t = \rho^t(\mathbf{F}, \mathbf{S}^{t-1}, \mathbf{L}^{t-1})$
  - **Branch 2**: PAF $\mathbf{L}^t = \phi^t(\mathbf{F}, \mathbf{S}^{t-1}, \mathbf{L}^{t-1})$
- **T 스테이지의 반복 정제**. 각 스테이지 끝에서 두 브랜치 모두에 L2 loss + intermediate supervision으로 vanishing gradient를 완화 ([[Pose Machines]] 계승).
- 누락 어노테이션에 대응하기 위해 마스크 $\mathbf{W}(\mathbf{p})$를 곱해 true positive를 페널티 처리하지 않게 함.

## 파싱: Bipartite Matching → Greedy Relaxation

- NMS로 파트 후보 집합 $\mathcal{D}_\mathcal{J}$ 확보.
- 모든 파트 쌍에 대한 최적 할당은 $K$-partite graph matching → NP-hard.
- **완화 두 가지**:
  1. 완전 그래프 대신 **트리 스켈레톤**만 사용 (13개 에지).
  2. 인접 트리 노드끼리만 **bipartite matching**(헝가리안 알고리즘)을 독립으로 풀고 공유 노드로 연결.
- 전역 inference가 아니지만 큰 수용 영역을 가진 CNN이 PAF에 암묵적 글로벌 컨텍스트를 인코딩하므로 품질 저하가 거의 없음.

## 결과

| 데이터셋 | 지표 | 이전 SOTA | 본 논문 |
|---|---|---|---|
| MPII Multi-Person (288장 서브셋) | mAP | 54.1 (Deepcut) | **79.7** |
| MPII (전체) | mAP | 71.2 (DeeperCut) | **75.6** (3-scale search) |
| COCO 2016 keypoints challenge | AP | — | **60.5** (1위) |

- MPII 서브셋 기준 **런타임 57,995s → 0.005s** (DeeperCut 대비 6자릿수 단축).
- 19명 비디오에서 **8.8 fps** (NVIDIA GTX-1080, 입력 368×654).
- 런타임 분해: CNN(99.6ms, O(1)) + 파싱(0.58ms, O(n²)). 실제 병목은 CNN 쪽.

## 실패 사례

> [!note] Figure 9 정리
> - 희귀 포즈/외관
> - 파트 미검출 또는 오검출
> - 겹친 파트(두 사람이 공유하는 것처럼 검출)
> - 두 사람 사이 잘못된 연결
> - 동상/동물 false positive

## 이 논문이 영향을 준 것

- 공개 구현이 [[OpenPose]]로 확장되어 손/얼굴 키포인트 포함 통합 시스템이 됨.
- "associative embedding" 스타일의 bottom-up 연구 계열을 확장.
- 실시간 스포츠 분석, AR, HCI 응용에서 사실상 표준 베이스라인.

## 관련 개념

- [[part-affinity-fields]] (이 논문의 핵심 기여)
- [[confidence-map]]
- [[bottom-up-pose-estimation]]
- [[bipartite-matching]]
- [[multi-person-pose-estimation]]

## 소스

- [[cao-2017-openpose-paf-source]] (원본 요약)
- 원본 PDF: `.raw/papers/Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields.pdf`
