---
type: concept
title: "Bipartite Matching"
complexity: intermediate
domain: ai-research
aliases:
  - "이분 매칭"
  - "Maximum Weight Bipartite Matching"
created: 2026-04-23
updated: 2026-04-23
tags:
  - concept
  - graph-theory
  - optimization
  - pose-estimation
status: developing
related:
  - "[[concepts/_index]]"
  - "[[part-affinity-fields]]"
  - "[[bottom-up-pose-estimation]]"
sources:
  - "[[cao-2017-openpose-paf-source]]"
---

# Bipartite Matching

## 정의

두 disjoint 노드 집합 $U, V$ 사이의 엣지들 중, **어떤 노드도 두 번 이상 선택되지 않는** 매칭을 찾는 문제. 엣지에 가중치가 있으면 **최대 가중치 이분 매칭(maximum weight bipartite matching)**.

## 왜 중요한가

여러 타입의 후보를 최적으로 짝지어야 하는 할당 문제에서 핵심 도구. [[bottom-up-pose-estimation]]에서 한 종류의 파트(예: 팔꿈치) 후보와 다른 종류의 파트(예: 손목) 후보를 림 가중치 기준으로 짝짓는 데 쓰임.

## 핵심 아이디어

- 최대화: $\max_{\mathcal{Z}_c} \sum_{m\in \mathcal{D}_{j_1}} \sum_{n\in \mathcal{D}_{j_2}} E_{mn}\cdot z_{j_1 j_2}^{mn}$
- 제약: 각 노드는 최대 하나의 엣지에만 참여.
- **헝가리안 알고리즘** $O(n^3)$로 풀 수 있음.

### 더 큰 K-partite 문제와의 관계

- 사람의 풀바디 포즈는 $K$개의 파트 타입을 묶는 $K$-partite 매칭 → NP-hard.
- [[cao-2017-openpose-paf]]는 **두 가지 완화**로 다항 시간에 근사:
  1. 완전 그래프 대신 **트리 스켈레톤** (13개 림 에지).
  2. 트리 위에서 **인접 노드 쌍마다 독립적 bipartite matching**.

## 예시

팔꿈치 후보 3개 vs 손목 후보 3개:
- 3×3 = 9개 엣지 각각에 PAF 선적분 점수
- 헝가리안으로 최적 3개 매칭 선택
- 각 매칭이 한 사람의 오른팔(또는 왼팔 등) 후보가 됨

## 언제 쓰는가 / 쓰지 않는가

- **쓰는 상황**: 할당 문제, 매칭 문제, 그래프 위 라벨링, 다중 인스턴스 연결.
- **쓰지 않는 상황**: 매칭 제약이 복잡한 경우(네트워크 플로우, ILP 필요).

## 관련 개념

- [[part-affinity-fields]]
- [[bottom-up-pose-estimation]]

## 소스

- [[cao-2017-openpose-paf-source]]
- [[cao-2017-openpose-paf]]
