---
type: source
title: "Cao 2017 — Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields (Source)"
source_type: paper
author: "Zhe Cao, Tomas Simon, Shih-En Wei, Yaser Sheikh"
date_published: 2017-04-14
url: https://arxiv.org/abs/1611.08050
confidence: high
key_claims:
  - "Part Affinity Fields(PAF)라는 비모수 2D 벡터 필드로 파트를 개인에게 연결할 수 있다"
  - "Bottom-up 방식이지만 top-down 수준의 정확도를 유지하며, 사람 수에 무관하게 실시간 성능(약 8.8 fps, GTX-1080)"
  - "COCO 2016 keypoints 챌린지 1위, MPII multi-person 벤치마크 SOTA"
created: 2026-04-23
updated: 2026-04-23
tags:
  - source
  - paper
  - computer-vision
  - pose-estimation
status: developing
raw_file: ".raw/papers/Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields.pdf"
related:
  - "[[sources/_index]]"
  - "[[cao-2017-openpose-paf]]"
  - "[[ai-research]]"
---

# Cao 2017 — Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields

## 요약

Zhe Cao 외 3인(CMU Robotics Institute)이 2017 CVPR(arXiv v2, 2017-04-14)에 발표한 멀티 퍼슨 2D 포즈 추정 논문. Part Affinity Fields(PAF)라는 **림(limb)별 2D 벡터 필드** 표현을 제안해, 각 신체 파트를 어느 사람에게 연결해야 하는지를 비모수적으로 인코딩한다. 두 개의 브랜치를 가진 반복 CNN(Pose Machine 아키텍처 계승)이 confidence map과 PAF를 동시에 예측하고, 그 결과를 그리디 파스(bipartite matching)로 풀어 다수 인원의 풀바디 포즈를 조립한다. 상향식 접근이면서도 정확도가 뛰어나며, 사람 수에 비례해 느려지는 top-down 방식과 달리 **O(1) CNN + O(n²) 파싱**으로 실시간이 가능하다. COCO 2016 keypoints 챌린지에서 1위, MPII 멀티 퍼슨에서 SOTA를 갱신했고, 공개된 코드는 이후 [[OpenPose]] 프로젝트의 기반이 되었다.

## 핵심 인용

> "We present the first bottom-up representation of association scores via Part Affinity Fields (PAFs), a set of 2D vector fields that encode the location and orientation of limbs over the image domain."

> "Our method has achieved the speed of 8.8 fps for a video with 19 people."

> "Our inference time is 6 orders of magnitude less [than Deepcut]."

## 이 소스에서 만들어진 페이지

- 논문 요약: [[cao-2017-openpose-paf]]
- 개념: [[part-affinity-fields]], [[bottom-up-pose-estimation]], [[confidence-map]], [[bipartite-matching]], [[multi-person-pose-estimation]]
- 저자 엔티티: [[Zhe-Cao]], [[Tomas-Simon]], [[Shih-En-Wei]], [[Yaser-Sheikh]]
- 조직/도구/데이터셋: [[CMU-Robotics-Institute]], [[OpenPose]], [[COCO-Dataset]], [[MPII-Dataset]], [[VGG-19]]

## 내 생각

- 비모수 벡터 필드로 "연결 관계"를 공간에 분산 인코딩한다는 아이디어는, 이후 다양한 associative embedding / 관계 필드 연구에 영향을 줬다.
- 런타임이 사람 수와 거의 무관하다는 점이 현업 적용에서 결정적. top-down은 인원이 늘수록 선형 증가.
- 실패 사례(Fig. 9): 희귀 포즈, 파트 중첩, 동상/동물 false positive. 도메인 갭을 보여주는 대표 예시들.
- 후속: [[OpenPose]]가 이 논문 구현을 확장해 손/얼굴 키포인트까지 커버. `O(n²)` 파싱 비용은 파스 단계 최적화 여지가 남음.

## 원본 파일

`.raw/papers/Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields.pdf`
