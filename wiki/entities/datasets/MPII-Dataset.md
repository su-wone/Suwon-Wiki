---
type: entity
title: "MPII Human Pose Dataset"
entity_type: dataset
role: "Human pose estimation benchmark"
first_mentioned: 2026-04-23
created: 2026-04-23
updated: 2026-04-23
tags:
  - entity
  - dataset
  - computer-vision
  - benchmark
  - pose-estimation
status: developing
related:
  - "[[entities/_index]]"
  - "[[multi-person-pose-estimation]]"
  - "[[cao-2017-openpose-paf]]"
sources:
  - "[[cao-2017-openpose-paf-source]]"
---

# MPII Human Pose Dataset

## 개요

Max Planck Institute(MPI)에서 공개한 인간 포즈 추정 벤치마크. Andriluka et al., CVPR 2014. 약 25,000장 이미지, 40,000 이상 사람 인스턴스. 일상 활동의 실제 이미지(410개 활동 카테고리)로 구성. 멀티 퍼슨 서브셋(288장)은 bottom-up 평가의 표준.

## 핵심 사실

- 발표: Andriluka et al., "2D human pose estimation: New benchmark and state of the art analysis", CVPR 2014
- 평가 지표: mAP (PCKh 기반)
- 멀티 퍼슨 테스트 서브셋: 288장 (bottom-up 비교 표준)
- 전체 테스트 세트에서 [[cao-2017-openpose-paf]]가 75.6 mAP (SOTA 기준)

## 이 위키와의 관련성

포즈 추정 논문에서 [[COCO-Dataset]]과 함께 언급되는 양대 벤치마크.

## 관련 소스

- [[cao-2017-openpose-paf-source]]

## 외부 링크

- 웹사이트: http://human-pose.mpi-inf.mpg.de/
