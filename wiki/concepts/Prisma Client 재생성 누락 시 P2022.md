---
type: concept
title: "Prisma Client 재생성 누락 시 P2022"
created: 2026-04-27
updated: 2026-04-27
tags:
  - prisma
  - migration
  - troubleshooting
  - postgres
status: developing
related:
  - "[[Prisma]]"
  - "[[PostgreSQL]]"
  - "[[server-board]]"
---

# Prisma Client 재생성 누락 시 P2022

## 한 줄

[[Prisma]]에서 **수동으로 `migration.sql`을 작성하면** Prisma Client(`generated/prisma/`)가 자동 재생성되지 않아, 시드/런타임에서 `P2022 ColumnNotFound` ("The column `(not available)` does not exist in the current database") 에러가 발생한다. 해결: `npx prisma generate` 실행.

---

## 증상

```
PrismaClientKnownRequestError:
Invalid `prisma.cards.create()` invocation
The column `(not available)` does not exist in the current database.
  code: 'P2022'
```

특이점: `npx prisma migrate status`는 **"Database schema is up to date!"** 로 표시된다. 즉 마이그레이션은 DB에 정상 적용됐는데도 에러가 난다.

---

## 원인

Prisma는 다음 두 산출물을 별도로 관리한다.

1. **DB 스키마** — `prisma/migrations/*/migration.sql`로 변경, 적용 명령은 `migrate dev` / `migrate deploy`
2. **Prisma Client (생성된 TS 코드)** — `generated/prisma/` (또는 `node_modules/@prisma/client`)에 위치. 모델 타입과 쿼리 빌더가 들어 있다. 재생성 명령은 `prisma generate`

`prisma migrate dev`는 **마이그레이션 적용 + Client 재생성**을 함께 한다. 그러나 **수동으로 `migration.sql`을 작성하고 `migrate deploy`만 돌리거나, 마이그레이션 파일만 두고 따로 `db push`/`migrate dev`를 안 한 경우**, Client는 옛 스키마 기준으로 남는다.

이 상태에서 `prisma.cards.create({ data: { ... } })`를 호출하면, Client가 옛 스키마 기준으로 INSERT 쿼리를 빌드한다. 옛 스키마에 있던 컬럼이 DB에는 더 이상 없으므로 `ColumnNotFound`가 난다. (반대 방향, 즉 DB에는 있지만 Client에는 없는 컬럼도 동일한 패턴.)

`(not available)` 표기는 Prisma가 누락된 컬럼명을 메타데이터에서 못 찾을 때 출력하는 placeholder다.

---

## 진단 절차

마이그레이션 status는 정상인데 ColumnNotFound가 나는 경우:

```bash
# 1. 마이그레이션 자체는 적용됐는지 확인
cd /path/to/server
npx prisma migrate status   # "Database schema is up to date!"

# 2. 생성된 Client에 옛 컬럼이 남아 있는지 grep
grep -rl "epicId" generated/prisma/   # 또는 문제 컬럼명
# 결과가 나오면 Client가 옛 스키마 기준이라는 증거
```

`grep` 결과가 비어 있어야 정상. 옛 컬럼이 보이면 Client 미재생성이 확정.

---

## 해결

```bash
npx prisma generate
```

이 한 줄로 `generated/prisma/`가 현재 `schema.prisma` 기준으로 다시 만들어진다. 이후 `npx prisma db seed` 같은 명령이 정상 동작한다.

---

## 예방 수칙

| 상황 | 권장 명령 |
|---|---|
| 일반적인 스키마 변경 | `npx prisma migrate dev --name <name>` (자동으로 Client 재생성) |
| 수동 `migration.sql` 작성 | `npx prisma migrate deploy && npx prisma generate` |
| CI/배포 환경 | `npx prisma migrate deploy && npx prisma generate` (대부분의 가이드는 이 두 명령을 항상 짝으로 권장) |
| Client/스키마 동기화 의심 | 의심되면 무조건 `npx prisma generate` 한 번 더 |

핵심 멘탈 모델: **마이그레이션 적용과 Client 재생성은 별개의 작업**이다. `migrate dev`만 자동으로 둘을 묶어 준다.

---

## 관련 사례

- [[server-board]]에서 `Epic` 모델을 제거하는 마이그레이션을 수동 작성(`20260427060000_remove_epic`) 후 시드 실패. `migrate status`는 정상이었으나 `generated/prisma/models/Cards.ts`에 `epicId`가 남아 있었음. `prisma generate` 실행 후 시드 정상.
- 동일 함정이 컬럼 추가 직후에도 일어난다 (Client에는 새 컬럼 없음 → 새 컬럼에 값을 넣는 코드가 컴파일은 통과해도 런타임에서 실패).

---

## 추가 참고

- Prisma 문서: [Generating the Prisma Client](https://www.prisma.io/docs/concepts/components/prisma-client/working-with-prismaclient/generating-prisma-client)
- 동등한 함정: TypeORM `synchronize: false`에서 엔티티 메타데이터 캐시 미갱신, Drizzle `drizzle-kit push` 미실행 등 — ORM 전반에서 "DB는 최신, 클라이언트는 stale" 패턴은 공통이다.