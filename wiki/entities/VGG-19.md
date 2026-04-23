---
type: entity
title: "VGG-19"
entity_type: model
role: "Backbone CNN architecture"
first_mentioned: 2026-04-23
created: 2026-04-23
updated: 2026-04-23
tags:
  - entity
  - model
  - neural-network
  - backbone
  - deep-learning
status: developing
related:
  - "[[entities/_index]]"
  - "[[cao-2017-openpose-paf]]"
sources:
  - "[[cao-2017-openpose-paf-source]]"
---

# VGG-19

## 개요

Simonyan & Zisserman(Oxford VGG), ICLR 2015. 19개 가중치 레이어의 CNN. 3×3 컨볼루션을 반복 쌓아 깊이로 성능을 끌어올린 대표 아키텍처. ImageNet 사전학습 가중치가 널리 공개되어 이후 백본으로 다수 채택됐다.

## 핵심 사실

- 발표: Simonyan & Zisserman, "Very Deep Convolutional Networks for Large-Scale Image Recognition", ICLR 2015
- [[cao-2017-openpose-paf]]는 VGG-19의 **앞쪽 10 레이어**만 피처 추출기로 사용하고 파인튜닝.
- ResNet이 등장하기 전 사실상 표준 백본 중 하나.

## 이 위키와의 관련성

포즈 추정 초기(2016~2017) 논문의 공통 백본. 모델 선택 비교 기준점.

## 관련 소스

- [[cao-2017-openpose-paf-source]]

## 외부 링크

- 논문: https://arxiv.org/abs/1409.1556
