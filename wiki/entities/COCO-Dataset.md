---
type: entity
title: "COCO Dataset"
entity_type: dataset
role: "Object detection / segmentation / keypoints benchmark"
first_mentioned: 2026-04-23
created: 2026-04-23
updated: 2026-04-23
tags:
  - entity
  - dataset
  - computer-vision
  - benchmark
status: developing
related:
  - "[[entities/_index]]"
  - "[[multi-person-pose-estimation]]"
  - "[[cao-2017-openpose-paf]]"
sources:
  - "[[cao-2017-openpose-paf-source]]"
---

# COCO Dataset

## 개요

Microsoft Common Objects in Context. Lin et al., ECCV 2014. 객체 검출, 세그멘테이션, 키포인트 추정을 위한 대규모 벤치마크. 이미지 내 객체를 풍부한 맥락(자연 이미지, 복잡한 배경)에서 라벨링.

## 핵심 사실

- 발표: Lin et al., "Microsoft COCO: Common Objects in Context", ECCV 2014
- Keypoints 태스크:
  - 10만+ 사람 인스턴스, 100만+ 키포인트 라벨
  - 서브셋: test-challenge, test-dev, test-standard (~20K 이미지 each)
  - 평가 지표: Object Keypoint Similarity (OKS) 기반 AP, 10 OKS threshold 평균

## 이 위키와의 관련성

[[multi-person-pose-estimation]] 연구의 표준 벤치마크. 포즈 관련 논문을 비교할 때 공통 척도.

## 관련 소스

- [[cao-2017-openpose-paf-source]]

## 외부 링크

- 웹사이트: https://cocodataset.org/
- Keypoints 평가: https://cocodataset.org/#keypoints-eval
