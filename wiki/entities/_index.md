---
type: meta
title: "엔티티 인덱스"
created: 2026-04-23
updated: 2026-04-26
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

모든 도메인이 공유하는 엔티티. 사람, 조직, 제품, 도구, 라이브러리, 데이터셋, 모델.

## 폴더 구조

엔티티는 `entity_type`별로 하위 폴더에 정리됩니다 (basename은 고유하므로 위키링크는 `[[Name]]` 형식 유지):

```
wiki/entities/
  people/         — entity_type: person
  organizations/  — entity_type: organization
  tools/          — entity_type: tool / product
  datasets/       — entity_type: dataset
  models/         — entity_type: model
```

## 사람 (`people/`)

### 연구자 / 저자
- [[Zhe-Cao]] — OpenPose/PAF 1저자
- [[Tomas-Simon]] — OpenPose/PAF 공저자
- [[Shih-En-Wei]] — OpenPose/PAF 공저자, CPM 1저자
- [[Yaser-Sheikh]] — CMU 교수, 포즈/휴먼 캡처 그룹 리더

### 강사 / 멘토
<!-- 유튜브 채널, 코스 강사 -->

### 채용 담당자 / 동료
<!-- 회사 contact -->

## 조직 (`organizations/`)

### 연구소 / 학회
- [[CMU-Robotics-Institute]] — OpenPose 본거지

### 회사 (고용주/후보)
<!-- wiki/companies/ 로 연결 -->

## 제품 / 도구 (`tools/`)

### 개발 도구
<!-- VSCode, Git, Docker -->

### 런타임
- [[Node.js]] — JavaScript 런타임 (server-board 백엔드 실행 환경)

### 라이브러리 / 프레임워크
- [[NestJS]] — TypeScript Node.js 서버 프레임워크 (server-board)
- [[Prisma]] — TypeScript ORM (server-board)
- [[OpenPose]] — 실시간 멀티 퍼슨 키포인트 라이브러리

### 테스트 / 빌드
- [[Jest]] — JS/TS 테스트 프레임워크 (server-board)
- [[tsx]] — TypeScript 직접 실행 (server-board seed)

### 이슈 트래커 / PM
- [[Jira]] — Atlassian 이슈 트래커 (server-board 모델 차용)

### 언어
- [[TypeScript]] — server-board 메인 언어

### 데이터베이스
- [[PostgreSQL]] — server-board 저장소 DB

### 서비스 / 플랫폼
<!-- AWS, Vercel, Supabase -->

## 데이터셋 (`datasets/`)

- [[COCO-Dataset]] — 객체/키포인트 벤치마크
- [[MPII-Dataset]] — 인간 포즈 벤치마크

## 모델 (`models/`)

- [[VGG-19]] — 백본 CNN

## 저장소 (Repositories)

<!-- 참조하는 오픈소스 저장소. entity_type: repository → tools/ 또는 별도 repositories/ 추가 가능 -->

---

## 엔티티 페이지 만들기

인제스트 시 Claude가 자동 생성하면서 entity_type에 맞는 하위 폴더에 배치. 처음 언급되면 stub 페이지가 생기고, 재언급되면 내용이 쌓입니다.

### 폴더 배치 규칙

| `entity_type` | 폴더 |
|---|---|
| `person` | `people/` |
| `organization` | `organizations/` |
| `tool`, `product` | `tools/` |
| `dataset` | `datasets/` |
| `model` | `models/` |

### 권장 frontmatter

```yaml
---
type: entity
title: "엔티티 이름"
entity_type: person  # person|organization|tool|dataset|model|repository
role: "역할 한 문장"
first_mentioned: "[[첫 소스]]"
status: developing
---
```
