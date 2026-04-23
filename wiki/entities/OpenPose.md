---
type: entity
title: "OpenPose"
entity_type: tool
role: "Realtime multi-person 2D pose estimation library"
first_mentioned: 2026-04-23
created: 2026-04-23
updated: 2026-04-23
tags:
  - entity
  - tool
  - library
  - computer-vision
  - pose-estimation
status: developing
related:
  - "[[entities/_index]]"
  - "[[cao-2017-openpose-paf]]"
  - "[[part-affinity-fields]]"
  - "[[CMU-Robotics-Institute]]"
sources:
  - "[[cao-2017-openpose-paf-source]]"
---

# OpenPose

## 개요

CMU Perceptual Computing Lab (Yaser Sheikh 그룹)에서 공개한 실시간 멀티 퍼슨 키포인트 추정 오픈소스 라이브러리. [[cao-2017-openpose-paf]] 논문의 구현을 기반으로 시작해, 이후 손(hand) 키포인트, 얼굴(face) 랜드마크, 풋(foot) 키포인트를 추가한 통합 시스템으로 확장됐다.

## 핵심 사실

- 라이선스: 비상업 연구/학술 사용 (상업 라이선스 별도)
- 구현: C++ / CUDA, Python 바인딩 제공
- 지원 키포인트: body (COCO 18 or BODY_25), hand (21), face (70)
- 기반 논문: [[cao-2017-openpose-paf]]
- 저장소: https://github.com/CMU-Perceptual-Computing-Lab/openpose

## 이 위키와의 관련성

[[ai-research]]와 [[fullstack-dev]] 교차점. 포즈 추정 프로젝트에 실제 적용할 때 참조할 엔티티. [[part-affinity-fields]]의 참조 구현.

## 관련 소스

- [[cao-2017-openpose-paf-source]]

## 외부 링크

- GitHub: https://github.com/CMU-Perceptual-Computing-Lab/openpose
- 영상 결과: https://youtu.be/pW6nZXeWlGM
