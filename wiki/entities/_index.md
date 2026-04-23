---
type: meta
title: "엔티티 인덱스"
created: 2026-04-23
updated: 2026-04-23
tags:
  - meta
  - index
  - entity
status: evergreen
related:
  - "[[index]]"
  - "[[concepts/_index]]"
---

# 엔티티 인덱스

모든 도메인이 공유하는 엔티티. 사람, 조직, 제품, 도구, 라이브러리, 저장소.

## 사람 (People)

### 연구자 / 저자
<!-- AI 논문 저자, 기술 리더 -->

### 강사 / 멘토
<!-- 유튜브 채널, 코스 강사 -->

### 채용 담당자 / 동료
<!-- 회사 contact -->

## 조직 (Organizations)

### 회사 (고용주/후보)
<!-- wiki/companies/ 로 연결 -->

### 연구소 / 학회
<!-- OpenAI, DeepMind, Anthropic, NeurIPS -->

## 제품 / 도구 (Products & Tools)

### 개발 도구
<!-- VSCode, Git, Docker -->

### 라이브러리 / 프레임워크
- [[NestJS]] — TypeScript Node.js 서버 프레임워크 (server-board)
- [[Prisma]] — TypeScript ORM (server-board)

### 언어
- [[TypeScript]] — server-board 메인 언어

### 데이터베이스
- [[PostgreSQL]] — server-board 저장소 DB

### 서비스 / 플랫폼
<!-- AWS, Vercel, Supabase -->

## 저장소 (Repositories)

<!-- 참조하는 오픈소스 저장소 -->

---

## 엔티티 페이지 만들기

인제스트 시 Claude가 자동 생성. 처음 언급되면 stub 페이지가 생기고, 재언급되면 내용이 쌓입니다.

### 권장 frontmatter

```yaml
---
type: entity
title: "엔티티 이름"
entity_type: person  # person|organization|product|repository|place
role: "역할 한 문장"
first_mentioned: "[[첫 소스]]"
status: developing
---
```
