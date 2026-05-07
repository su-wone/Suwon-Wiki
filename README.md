# Wiki Starter

Claude Code + Obsidian 기반 영속적 위키 볼트의 골격(skeleton)입니다. 풀스택 개발 / AI 리서치 / 취업시장 / 개발 노트 4개 도메인을 통합하는 지식 베이스를 빠르게 시작할 수 있도록 폴더 구조와 템플릿만 포함되어 있습니다.

## 시작하기

```bash
git clone --depth 1 -b template https://github.com/su-wone/Suwon-Wiki.git my-wiki
cd my-wiki
rm -rf .git
git init -b main
git add -A
git commit -m "init from wiki template"
git remote add origin <본인-repo-URL>
git push -u origin main
```

본인 GitHub repo 없이 로컬만 쓸 거면 마지막 두 줄은 생략.

## 폴더 구조

```
.raw/                원시 소스 (불변, Claude는 읽기만) — 6모드 폴더
  A-website/         사이트 크롤, 웹페이지 dump
  B-github/          본인 코드, README, git log
  C-business/        회사 자료, 회의록, JD
  D-personal/        일기, 음성 트랜스크립트, AI 대화 로그
  E-research/        논문 PDF, 웹 리서치 클립
  F-books/           책/강의 노트

wiki/                Claude가 생성하는 지식 베이스
  index.md           마스터 카탈로그
  log.md             작업 로그 (추가 전용)
  hot.md             핫 캐시
  overview.md        위키 개요
  domains/           4개 도메인 허브
  projects/          본인 프로젝트
  papers/            AI 논문 요약
  companies/         취업 시장 회사
  learning/          개발 학습 노트
  decisions/         기술 결정 / ADR
  concepts/          공유 개념
  entities/          사람, 회사, 도구
  sources/           인제스트된 소스 요약
  comparisons/       기술/회사/논문 비교
  questions/         좋은 답변 정리
  meta/              운영 매뉴얼, 대시보드

_templates/          노트 템플릿 (모드별 + 공유)
_attachments/        이미지, PDF
```

## 기본 작업 흐름

1. **Ingest**: 자료를 받으면 모드 판단 → `.raw/<모드폴더>/`에 떨어뜨림 → Claude Code에서 `ingest <파일명>`
2. **Query**: 그냥 질문 입력. Claude가 `index.md` → 관련 페이지 순으로 회수
3. **Lint**: `lint the wiki`로 헬스 체크 (orphan, 끊어진 링크 검사)
4. **Save**: `/save`로 현재 대화를 위키 노트로 정리
5. **Autoresearch**: `/autoresearch <주제>`로 자율 웹 리서치

## 더 읽을거리

- [CLAUDE.md](./CLAUDE.md) — 운영 룰, 모드 결정 규칙, lazy expansion 규칙
- [WIKI.md](./WIKI.md) — LLM 위키 스키마 레퍼런스 (Karpathy 패턴 기반)

## 요구사항

- [Obsidian](https://obsidian.md/)
- [Claude Code](https://claude.ai/code)
- (권장) claude-obsidian 플러그인
