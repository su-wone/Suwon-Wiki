---
type: meta
title: "작업 로그"
created: 2026-04-23
updated: 2026-04-23
tags:
  - meta
  - log
status: evergreen
related:
  - "[[index]]"
  - "[[hot]]"
  - "[[overview]]"
---

# 작업 로그

내비게이션: [[index]] | [[hot]] | [[overview]]

추가 전용. 새 항목은 맨 위에 추가합니다. 과거 항목은 절대 수정하지 마세요.

항목 형식: `## [YYYY-MM-DD] operation | Title`

최근 항목 파싱: `grep "^## \[" wiki/log.md | head -10`

---

## [2026-04-23] ingest | server-board (B-github 모드 첫 인제스트)
- Source: `.raw/B-github/server-board/` (README, package.json, git-log, remotes, branches, tree)
- Origin: https://github.com/su-wone/Board-Server.git (branch: feat/frontend-integration)
- Created: [[server-board]] (project hub), [[server-board-repo]] (source), [[NestJS]] [[Prisma]] [[PostgreSQL]] [[TypeScript]] (entities)
- Updated: [[index]], [[projects/_index]], [[hot]]
- Stack: NestJS 11 + Prisma 7 + PostgreSQL, ESM, TypeScript 5.7
- 도메인: Users / Sprints / Workflows / Cards / Epics / Labels (VEASLY 이슈 트래커)

## [2026-04-23] save | 위키 능동적 사용법
- Type: synthesis (question)
- Location: wiki/questions/위키-능동-사용법.md
- From: 사용자 질문 "내가 이걸 능동적으로 사용하려면 어떻게 해야하나요?"에 대한 답변 정리
- 핵심: 입력(.raw 떨구기) → 트리거(슬래시) → 회수(질문) 3박자, 4가지 루틴, 실패 모드
- 첫 [[wiki/questions/]] 페이지

## [2026-04-23] policy | wiki/ 게으른 확장 규칙 도입
- 동기: `.raw/`는 6모드 받지만 `wiki/`는 활성 4모드(B+E+C+D)에만 최적화 → A/F가 헐거움
- 결정: 빈 폴더 미리 만들지 않음. 모드 첫 사용 시 자동 신설.
- 갱신: CLAUDE.md (게으른 확장 규칙 섹션), [[wiki-운영-매뉴얼]] v3 (섹션 6)
- 효과: 결정 부하 추가 없이 6모드 전부 수용 가능. 데이터가 폴더를 요구할 때만 생성됨.

## [2026-04-23] redesign | .raw 6모드 폴더 + 모드 템플릿 6개
- 동기: 자료별 폴더 분류(articles/papers/jobs/...)가 결정 부하를 줘서, WIKI.md의 6모드 이름으로 단순화
- `.raw/` 변경:
  - 신설: A-website/, B-github/, C-business/, D-personal/, E-research/, F-books/
  - 폐지: articles/, code/, jobs/, papers/, screenshots/, transcripts/
  - 이동: `.raw/papers/Realtime ... PAF.pdf` → `.raw/E-research/`
- `_templates/` 신설 (6개): A-website.md, B-module.md, C-decision.md, D-personal.md, E-paper.md, F-book.md
- `_templates/` 유지 (5개, cross-cutting): concept.md, entity.md, source.md, comparison.md, question.md
- 갱신: CLAUDE.md (구조 + 의사결정 룰), [[wiki-운영-매뉴얼]] (v2: 6모드 표), .raw/.manifest.json (PDF 새 경로), [[cao-2017-openpose-paf-source]], [[cao-2017-openpose-paf]] (raw 경로 갱신)

## [2026-04-23] save | wiki 운영 매뉴얼
- 타입: session
- 위치: `wiki/meta/wiki-운영-매뉴얼.md`
- 출처: 첫 인제스트 직후 대화 — `.raw/` 폴더 운영 원칙, `/save` vs `ingest` 차이, 두뇌 채우기 4가지 길, 첫 2주 플랜 정리
- 핵심: `.raw/`는 외부 불변 원본 전용. Claude 대화 내용은 `/save`로 직접 `wiki/`에 작성

## [2026-04-23] ingest | Cao 2017 — Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields
- 소스: `.raw/papers/Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields.pdf`
- 요약: [[cao-2017-openpose-paf-source]]
- 논문 페이지: [[cao-2017-openpose-paf]]
- 페이지 생성 (16개):
  - 소스: [[cao-2017-openpose-paf-source]]
  - 논문: [[cao-2017-openpose-paf]]
  - 개념: [[part-affinity-fields]], [[bottom-up-pose-estimation]], [[confidence-map]], [[bipartite-matching]], [[multi-person-pose-estimation]]
  - 사람: [[Zhe-Cao]], [[Tomas-Simon]], [[Shih-En-Wei]], [[Yaser-Sheikh]]
  - 조직/도구/데이터셋/모델: [[CMU-Robotics-Institute]], [[OpenPose]], [[COCO-Dataset]], [[MPII-Dataset]], [[VGG-19]]
- 페이지 갱신 (6개): [[index]], [[hot]], [[ai-research]], [[papers/_index]], [[sources/_index]], [[log]]
- 핵심 인사이트: 림을 "방향이 있는 2D 벡터 필드(PAF)"로 표현하면, bottom-up 접근이면서도 사람 수에 무관한 실시간 성능 + top-down 수준 정확도 동시 달성. COCO 2016 keypoints 챌린지 1위.

## [2026-04-23] setup | 볼트 초기화
- 위치: `/Users/admin/wiki/`
- 모드: B + E + C + F 결합 (풀스택 + AI 리서치 + 취업시장 + 개발 노트)
- 생성된 도메인 허브: [[fullstack-dev]], [[ai-research]], [[job-market]], [[dev-notes]]
- 시드 파일: [[index]], [[hot]], [[overview]], [[log]]
- 템플릿 5종 (concept, entity, source, question, comparison) 생성
- 다음 단계: 첫 소스를 `.raw/`에 떨어뜨리고 "ingest [파일명]"
