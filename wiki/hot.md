---
type: meta
title: "Hot Cache"
created: 2026-04-23
updated: 2026-04-23T17:00:00
tags:
  - meta
  - hot-cache
status: evergreen
related:
  - "[[index]]"
  - "[[log]]"
  - "[[overview]]"
---

# 최근 컨텍스트

내비게이션: [[index]] | [[log]] | [[overview]]

## 마지막 업데이트
2026-04-23: **[[server-board]]** 인제스트 — B-github 모드 첫 사용. NestJS 11 + Prisma 7 + PostgreSQL 백엔드. [[NestJS]] [[Prisma]] [[PostgreSQL]] [[TypeScript]] 엔티티 생성.
2026-04-23: **[[위키-능동-사용법]]** 저장 — `/save` 첫 사용. 입력→트리거→회수 3박자 + 4가지 루틴 정리.
2026-04-23: **wiki/ 게으른 확장 규칙** 도입 ([[wiki-운영-매뉴얼]] v3). 모드 첫 사용 시 wiki/ 폴더 자동 신설.
2026-04-23: **`.raw/` 6모드 폴더로 재구성** (A/B/C/D/E/F). 모드 템플릿 6개 추가. [[wiki-운영-매뉴얼]] v2 갱신.
2026-04-23: [[wiki-운영-매뉴얼]] 작성. 첫 논문 인제스트 [[cao-2017-openpose-paf]].

## 볼트 상태
- **위치**: `/Users/admin/wiki/`
- **모드**: B + E + C + F 결합 (B-github 활성화됨)
- **도메인**: 풀스택 개발 ← **방금 첫 프로젝트 추가**, AI 논문 리서치, 취업시장 분석, 개발 노트
- **인제스트된 소스**: 2 (논문 1 + 저장소 1)
- **위키 페이지**: 25 + 4 도메인 허브 + 시드
- **운영 매뉴얼**: [[wiki-운영-매뉴얼]] (필독)

## 최근 인제스트

### [[server-board]] (프로젝트, 2026-04-23)
- **한 줄**: Jira 스타일 보드 백엔드 (VEASLY- 이슈 prefix)
- **스택**: [[NestJS]] 11 + [[Prisma]] 7 + [[PostgreSQL]] + [[TypeScript]] 5.7 (ESM)
- **도메인**: Users / Sprints / Workflows / Cards / Epics / Labels
- **현재 브랜치**: `feat/frontend-integration`
- **저장소**: `su-wone/Board-Server`
- **원본**: `.raw/B-github/server-board/` (README, package.json, git-log, tree)

### [[cao-2017-openpose-paf]] (논문, CVPR 2017)
- **한 줄**: PAF로 bottom-up 멀티 퍼슨 2D 포즈 추정을 실시간으로 해결
- **저자**: [[Zhe-Cao]], [[Tomas-Simon]], [[Shih-En-Wei]], [[Yaser-Sheikh]] ([[CMU-Robotics-Institute]])
- **벤치마크**: COCO 2016 keypoints 1위 (60.5 AP), 19명 비디오 8.8 fps
- **핵심 개념**: [[part-affinity-fields]], [[confidence-map]], [[bottom-up-pose-estimation]], [[bipartite-matching]]

## 활성 영역

### 1. 풀스택 개발 ([[fullstack-dev]]) ← **방금 첫 프로젝트 추가**
- 프로젝트 1개: [[server-board]]
- 다음 후보: 프로젝트 내 ADR 정리, Prisma 스키마 진화 로그, 프론트엔드 통합 결정사항

### 2. AI 논문 리서치 ([[ai-research]])
- 논문 1개: [[cao-2017-openpose-paf]]
- 다음 후보: 후속 OpenPose 확장 논문, bottom-up vs top-down 비교

### 3. 취업시장 분석 ([[job-market]])
- 아직 인제스트 없음

### 4. 개발 노트 ([[dev-notes]])
- 아직 인제스트 없음

## 다음 액션

1. **server-board 갱신 루틴**: 큰 변화 시 `.raw/B-github/server-board/`의 README/git-log/tree 재수집 → [[server-board]] 페이지 갱신
2. **ADR 후보 (decisions)**: `key` 컬럼 제거 결정, `baseUrl` 제거, Memo/Upload 제거 — `/save decision [이름]`로 기록 가능
3. **모듈별 분해 (선택)**: cards / sprints / workflows / epics 각각 `wiki/components/` (B 확장) 페이지로 파면 상세 추적
4. **크로스 연결 기회**: 포즈 추정 논문 개념이 서비스에 쓰일 여지 있는지 ([[fullstack-dev]] ↔ [[ai-research]])

## 열린 질문
- server-board 프론트엔드 통합이 끝나면 어떤 배포 전략을 쓸지 (AWS/Vercel/자체)
- `VEASLY-###` 이슈 시스템과 실제 Jira 연동을 할 것인지
- 소프트 삭제(`deletedAt`) 패턴을 어디까지 유지할지 (복구 필요한지)

## 스타일 선호
- 한국어로 작성 (본문, 섹션 제목, 안내)
- frontmatter 키와 위키링크 타깃은 영어 유지
