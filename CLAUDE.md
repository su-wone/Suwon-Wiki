# 개인 위키 볼트

Mode: B + E + C + F (풀스택 개발, AI 리서치, 취업시장, 개발 노트)
Purpose: 풀스택 개발, AI 논문 리서치, 취업시장 분석, 개발 노트를 위한 영속적 지식 베이스
Owner: admin (sw.kang@newndy.com)
Created: 2026-04-23

## 구조

```
.raw/           원시 소스 (불변, Claude는 읽기만)
  articles/     기사, 블로그 포스트
  papers/       AI 논문 PDF
  transcripts/  강의/회의 트랜스크립트
  jobs/         채용 공고, JD
  code/         코드 스니펫, 문서
  screenshots/

wiki/           Claude가 생성하는 지식 베이스
  index.md      마스터 카탈로그
  log.md        작업 로그 (추가 전용)
  hot.md        핫 캐시 (약 500단어)
  overview.md   위키 개요
  domains/      4개 도메인 허브
  projects/     본인 풀스택 프로젝트
  papers/       AI 논문 요약
  companies/    취업 시장 회사
  learning/     개발 학습 노트
  decisions/    기술 결정 (ADR)
  concepts/     공유 개념
  entities/     사람, 회사, 도구
  sources/      인제스트된 소스 요약
  comparisons/  기술/회사/논문 비교
  questions/    좋은 답변 정리
  meta/         대시보드, 린트 보고서

_templates/     노트 템플릿 (5종)
_attachments/   이미지, PDF
```

## 규칙

- 모든 노트는 YAML frontmatter 사용: type, status, created, updated, tags (최소)
- 위키링크는 `[[Note Name]]` 형식. 파일명은 고유해서 경로 불필요
- `.raw/`는 절대 수정 금지
- `wiki/index.md`는 마스터 카탈로그. 매 인제스트 시 갱신
- `wiki/log.md`는 추가 전용. 새 항목은 맨 위에. 과거 항목 수정 금지

## 작업

- **Ingest**: 소스를 `.raw/`에 떨어뜨리고 "ingest [파일명]" 입력
- **Query**: 아무 질문. Claude가 인덱스를 먼저 읽고 관련 페이지로 파고듦
- **Lint**: "lint the wiki"로 헬스 체크
- **Save**: `/save`로 현재 대화를 위키 노트로 정리
- **Autoresearch**: `/autoresearch [주제]`로 자율 웹 리서치

## 4개 도메인

1. **풀스택 개발** — `wiki/projects/`, `wiki/decisions/`
   - 본인 코드 프로젝트, 아키텍처 결정, 기술 스택

2. **AI 논문 리서치** — `wiki/papers/`
   - 논문 요약, 핵심 주장, 방법론 비교

3. **취업시장 분석** — `wiki/companies/`
   - 회사 분석, JD 패턴, 연봉, 스택, 경쟁 포지셔닝

4. **개발 노트** — `wiki/learning/`
   - 새로 배운 개념, 강의 노트, 튜토리얼 시사점

## 크로스 도메인 연결

개념은 `wiki/concepts/`에, 사람/도구/라이브러리는 `wiki/entities/`에 공유됩니다. 예를 들어 AI 논문에서 배운 개념이 본인 프로젝트에 적용되면 두 곳에서 같은 개념 페이지를 참조하게 됩니다. 이것이 위키가 복리로 쌓이는 방식입니다.
