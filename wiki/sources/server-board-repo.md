---
type: source
title: "server-board 저장소 스냅샷"
source_type: github-repo
source_path: ".raw/B-github/server-board/"
source_origin: https://github.com/su-wone/Board-Server.git
captured: 2026-04-23
branch: feat/frontend-integration
created: 2026-04-23
updated: 2026-04-23
tags:
  - source
  - mode-b
  - repo-snapshot
status: developing
related:
  - "[[server-board]]"
---

# server-board 저장소 스냅샷

## 원본

- 경로: `.raw/B-github/server-board/`
- 수집일: 2026-04-23
- 브랜치: `feat/frontend-integration`
- 커밋 기준: `fe1c627` (docs(swagger): flesh out response schemas...)

## 포함 파일

- `README.md` — NestJS 스타터 디폴트 README (프로젝트 커스텀 없음)
- `package.json` — 의존성 (NestJS 11, Prisma 7, AWS SDK S3, Jest 30)
- `git-log.txt` — 최근 50 커밋
- `remotes.txt` — origin: `su-wone/Board-Server`
- `branches.txt` — 브랜치 목록
- `tree.txt` — `src/`, `prisma/` 구조 (depth 3)
- `prisma-ls.txt` — prisma 폴더 목록

## 요약

- **앱 이름**: server-board (package.json), 저장소 이름은 `Board-Server`
- **용도**: 보드 백엔드 API (Jira 스타일, VEASLY- prefix)
- **마이그레이션 6건** (2026-03-25 init → 2026-04-22 add_epic)
- **도메인**: cards, epics, sprints, workflows (src/board/ 하위)

## 갱신 방법

큰 변화(모듈 추가, 마이그레이션, 스택 변경) 있을 때 같은 경로에 덮어쓴 뒤 [[server-board]]를 갱신.

## 관련

- [[server-board]] — 프로젝트 허브
