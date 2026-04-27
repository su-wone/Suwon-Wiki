---
type: meta
title: "Hot Cache"
created: 2026-04-23
updated: 2026-04-27
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
2026-04-27: **[[Board 풀스택 학습 가이드]]** 저장 — `/save` synthesis. server-board(NestJS+Prisma) ↔ board(Next.js 16+React 19) 두 저장소를 처음 읽는 사람을 위한 통합 가이드. 연결 한 점은 `client.ts BASE` ↔ `main.ts enableCors`, 타입 동기화 수동(`cardSummarySchema` ↔ `Ticket`). 백엔드는 schema → main → modules → workflows → sprints → cards, 프론트는 types → api → page → AppShell → 합성 → backlog 순. 첫 프론트엔드 관련 페이지.
2026-04-27: **[[Prisma Client 재생성 누락 시 P2022]]** 저장 — `/save` concept. [[server-board]]에서 Epic 모델 제거 후 `prisma db seed`가 `P2022 ColumnNotFound`로 실패. `prisma migrate status`는 "up to date"였지만 `generated/prisma/`가 옛 스키마 기준이었음. 해결: `npx prisma generate`. 다른 ORM에도 적용되는 패턴이라 개념 페이지화.
2026-04-26: **[[esm-import-rule]]** 인제스트 — D-personal 모드 첫 사용. Notion에서 export한 ESM 학습 노트. [[esm-modules]] [[commonjs]] 개념 + [[Node.js]] 엔티티 생성. [[server-board]]와 직결.
2026-04-23: **[[server-board]]** 인제스트 — B-github 모드 첫 사용. NestJS 11 + Prisma 7 + PostgreSQL 백엔드. [[NestJS]] [[Prisma]] [[PostgreSQL]] [[TypeScript]] 엔티티 생성.
2026-04-23: **[[위키-능동-사용법]]** 저장 — `/save` 첫 사용. 입력→트리거→회수 3박자 + 4가지 루틴 정리.
2026-04-23: **wiki/ 게으른 확장 규칙** 도입 ([[wiki-운영-매뉴얼]] v3). 모드 첫 사용 시 wiki/ 폴더 자동 신설.
2026-04-23: **`.raw/` 6모드 폴더로 재구성** (A/B/C/D/E/F). 모드 템플릿 6개 추가. [[wiki-운영-매뉴얼]] v2 갱신.
2026-04-23: [[wiki-운영-매뉴얼]] 작성. 첫 논문 인제스트 [[cao-2017-openpose-paf]].

## 볼트 상태
- **위치**: `/Users/admin/wiki/`
- **모드**: B + E + C + F 결합 (B-github + D-personal 활성화됨)
- **도메인**: 풀스택 개발, AI 논문 리서치, 취업시장 분석, 개발 노트 ← **방금 첫 학습 노트 추가**
- **인제스트된 소스**: 3 (논문 1 + 저장소 1 + 학습노트 1)
- **위키 페이지**: 30 + 4 도메인 허브 + 시드
- **운영 매뉴얼**: [[wiki-운영-매뉴얼]] (필독)

## 최근 인제스트

### [[esm-import-rule]] (학습 노트, 2026-04-26)
- **한 줄**: ESM에서 import 경로에 `.js`를 쓰는 이유 — "실행될 때 존재할 파일을 적어라"
- **모드**: D-personal (첫 사용) — Notion 학습 노트를 PDF로 export
- **개념**: [[esm-modules]], [[commonjs]] / **엔티티**: [[Node.js]]
- **적용 대상**: [[server-board]] (NestJS 11 + TS 5.7 ESM 모드)
- **원본**: `.raw/D-personal/notion-study/ESM.pdf` (2026-04-21 작성)

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

### 1. 풀스택 개발 ([[fullstack-dev]])
- 프로젝트 1개: [[server-board]]
- synthesis 1개: [[Board 풀스택 학습 가이드]] (백엔드+프론트 학습 흐름)
- 다음 후보: 프론트 저장소 `board` 의 별도 프로젝트 페이지 신설, ADR 정리(`key` 컬럼 제거 / Memo 제거 / Epic 모델 제거), Prisma 스키마 진화 로그

### 2. AI 논문 리서치 ([[ai-research]])
- 논문 1개: [[cao-2017-openpose-paf]]
- 다음 후보: 후속 OpenPose 확장 논문, bottom-up vs top-down 비교

### 3. 취업시장 분석 ([[job-market]])
- 아직 인제스트 없음

### 4. 개발 노트 ([[dev-notes]])
- 학습 노트 1개: [[esm-import-rule]]
- 다음 후보: `tsconfig` 모듈 옵션 정리, ts-node/tsx 로더 비교, Notion 추가 노트 인제스트

## 다음 액션

1. **server-board 갱신 루틴**: 큰 변화 시 `.raw/B-github/server-board/`의 README/git-log/tree 재수집 → [[server-board]] 페이지 갱신
2. **ADR 후보 (decisions)**: `key` 컬럼 제거 결정, `baseUrl` 제거, Memo/Upload 제거 — `/save decision [이름]`로 기록 가능
3. **모듈별 분해 (선택)**: cards / sprints / workflows / epics 각각 `wiki/components/` (B 확장) 페이지로 파면 상세 추적
4. **크로스 연결 기회**: 포즈 추정 논문 개념이 서비스에 쓰일 여지 있는지 ([[fullstack-dev]] ↔ [[ai-research]])
5. **ESM 후속 학습 (D)**: `tsconfig` `module`/`moduleResolution`, `tsconfig-paths` + ESM, `tsx`/`ts-node` ESM 로더, Node `--experimental-strip-types`

## 열린 질문
- server-board 프론트엔드 통합이 끝나면 어떤 배포 전략을 쓸지 (AWS/Vercel/자체)
- `VEASLY-###` 이슈 시스템과 실제 Jira 연동을 할 것인지
- 소프트 삭제(`deletedAt`) 패턴을 어디까지 유지할지 (복구 필요한지)

## 스타일 선호
- 한국어로 작성 (본문, 섹션 제목, 안내)
- frontmatter 키와 위키링크 타깃은 영어 유지
