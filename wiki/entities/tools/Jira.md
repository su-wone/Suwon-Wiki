---
type: entity
entity_type: tool
title: "Jira"
created: 2026-04-27
updated: 2026-04-27
tags:
  - entity
  - tool
  - issue-tracker
  - product-management
status: seed
related:
  - "[[server-board]]"
---

# Jira

## 정체

Atlassian의 이슈 트래커 / 프로젝트 관리 도구. 스프린트, 칸반 보드, 워크플로 컬럼 모델의 사실상 표준.

## 어디에 영향을 주나

- [[server-board]] — Jira **워크플로 모델을 그대로 차용**한 백엔드. 이슈 키 prefix `VEASLY-###`도 Jira 스타일.
  - Sprints / Workflows / Cards / Epics 도메인 모델이 Jira 1:1 대응
  - Card type enum: EPIC / STORY / TASK / SUB_TASK / BUG (Jira issue type과 동일)
  - Sprint status: PLANNED / IN_PROGRESS / DONE

## 아직 결정되지 않은 것

> [!gap]
> server-board가 실제 Jira와 **연동(API)** 할지, 아니면 Jira **모델만 흡수한 자체 시스템**으로 갈지 미정. ([[hot]]의 열린 질문)

## 공식

- Docs: https://www.atlassian.com/software/jira
