---
type: decision
title: "<% tp.file.title %>"
mode: C
status: active  # active|pending|done|blocked|superseded
priority: 3  # 1-5
date: <% tp.date.now("YYYY-MM-DD") %>
owner: ""
due_date: ""
context: ""
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.date.now("YYYY-MM-DD") %>
tags:
  - decision
  - mode-c
related:
  - "[[index]]"
sources:
---

# <% tp.file.title %>

## 배경

왜 이 결정이 필요한가. 어떤 문제/제약?

## 결정 내용

한 문장으로 무엇을 정했는지.

## 대안

- **옵션 A**: ... → 기각 이유
- **옵션 B**: ... → 채택 이유
- **옵션 C**: ... → 기각 이유

## 결과 / 트레이드오프

- 얻는 것:
- 잃는 것:
- 미래 점검 시점:

## 이해관계자

- 결정자: 
- 영향 받는 사람: [[]]

## 관련

- 결정 영향: [[]]
- 후속 결정: [[]]

## 소스
- [[]]
