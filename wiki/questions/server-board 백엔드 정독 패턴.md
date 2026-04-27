---
type: synthesis
title: "server-board 백엔드 정독 패턴"
created: 2026-04-27
updated: 2026-04-27
tags:
  - synthesis
  - learning
  - nestjs
  - prisma
  - postgresql
  - patterns
status: developing
question: "server-board의 schema → main → modules → workflows → sprints 4단계 정독에서 코드만 읽어선 알기 어려운 비명시적 패턴(non-obvious) 은 무엇인가?"
answer_quality: solid
related:
  - "[[server-board]]"
  - "[[Board 풀스택 학습 가이드]]"
  - "[[NestJS]]"
  - "[[Prisma]]"
  - "[[PostgreSQL]]"
  - "[[esm-import-rule]]"
sources:
  - "[[server-board-repo]]"
---

# server-board 백엔드 정독 패턴

[[Board 풀스택 학습 가이드]]가 학습 순서를 안내한다면, 이 페이지는 그 순서를 따라 실제 정독하다가 코드만 읽어선 발견하기 어려운 **비명시적 결정·트레이드오프**를 정리한다. 동일한 디자인이 다음 도메인에 또 등장하므로, 한 번 정리해두면 압축 해제처럼 빠르게 읽힌다.

---

## 1. PostgreSQL 시퀀스와 표시 순서는 분리된다

### 관찰
`Workflows.findAll()`이 반환하는 응답에서 `id`가 5,9,11,10,6,7,8 처럼 흩어져 보인다. `orderBy: { order: 'asc' }`인데 왜 id가 순서대로가 아닌가.

### 핵심
**정렬 기준은 `id`가 아니라 보이지 않는 `order` 컬럼이다.** `select: { id, status }`만 했기에 `order` 필드가 응답에 노출되지 않을 뿐, DB에서는 `order = 0,1,2,3,4,5,6`으로 깔끔히 정렬되어 나온다.

### id가 흩어진 진짜 원인
`prisma/seed.ts`가 워크플로우는 TRUNCATE하지 않고 **upsert + deleteMany** 정책을 쓴다.

- 같은 `status`가 있으면 → UPDATE (id 유지)
- 없으면 → CREATE (시퀀스가 새 id 발급)
- 목록에 없는 status → DELETE

PostgreSQL 시퀀스는 **단방향**이라 DELETE해도 되돌아가지 않는다. 한때 만들어졌다 사라진 id 1~4의 자취가 영구 결손으로 남은 것.

### 왜 굳이 `id`와 `order`를 분리했는가
- `id`는 **행의 정체성** — 외래키(`Cards.workflowId`)가 가리키는 좌표.
- `order`는 **표시 순서** — 사용자가 컬럼을 드래그로 재정렬 가능.

만약 id로 순서를 표현했다면 컬럼 재배치마다 PK를 바꿔야 하고, 모든 외래키가 깨진다. id 흩어짐은 비용이 아니라 분리 설계의 정상 결과.

### 일반화된 룰
- **Surrogate Key (`id`)**: DB의 일. 비어 있어도, 흩어져도 신경 쓰지 않는다.
- **Natural Key (`status`, `key`)**: 사람이 읽는 값. 깔끔하게 관리한다.
- **표시 순서 (`order`)**: 별도 정수 컬럼.

`Cards`도 같은 패턴 — id는 시퀀스, `key`는 `VEASLY-1` 사람용, `order`는 컬럼 내부 위치.

---

## 2. ValidationPipe 3옵션의 의존성

### 관찰
`main.ts`가 `transform: true, whitelist: true, forbidNonWhitelisted: true` 셋을 모두 켠다. `forbidNonWhitelisted`만으로 충분해 보이는데 왜 `whitelist`도 켜는가.

### 핵심
**`forbidNonWhitelisted`는 `whitelist: true`가 없으면 동작하지 않는다.** `whitelist`가 "DTO에 없는 속성을 식별"하는 검사이고, `forbidNonWhitelisted`는 그 검사 결과의 **처벌 강도** 옵션(제거 vs 거부).

| `whitelist` | `forbidNonWhitelisted` | 결과 |
|---|---|---|
| false | (무관) | 통과 — 추가 필드 그대로 들어옴 |
| true | false | 추가 필드 **조용히 제거** |
| true | true | 추가 필드 시 **400 에러** |

### 정책 의미
"조용히 제거"는 클라이언트가 자기 실수를 모른다(오타가 사라지는데 디버깅 어려움). 백엔드/프론트가 같은 팀일 때는 **거부**가 더 안전 — 즉시 발견되어 고친다.

### `transform` 시너지
`transform: true`는 들어온 JSON을 DTO 클래스 인스턴스로 만들고 타입 변환(`?id=5` → `id: 5`). 그 후에야 class-validator 데코레이터들이 동작. 셋이 합쳐 **DTO가 곧 API 계약, 어긋나면 즉시 거부**.

---

## 3. `@Global()` PrismaModule — DI의 양면

### 관찰
`WorkflowsModule`은 `imports: [PrismaModule]`을 적지 않았는데도 `WorkflowsService`가 `PrismaService`를 주입받는다.

### 메커니즘
`PrismaModule`의 `@Global()` + `exports: [PrismaService]` 조합 → AppModule에 한 번만 등록하면 **하위 트리 어디서든** 자동 주입 가능. 보일러플레이트 N개 제거.

### 트레이드오프
- **장점**: 진짜 전역 자원(DB, 로거, 설정)에서 반복 import 제거.
- **단점**: 어느 모듈이 진짜 의존하는지 코드만으로 보이지 않음. 모듈 경계가 흐려짐.

### 실무 룰
DB 클라이언트, 로거, 설정처럼 **앱 전체가 공유하는 자원**에만 `@Global()`. 일반 도메인 서비스에는 쓰지 않는다.

---

## 4. DTO는 자료 검증, Service는 도메인 검증

### 관찰
`StartSprintDto`는 `@IsISO8601()`로 날짜 포맷만 검증한다. "endDate는 startDate 이후여야 한다"는 규칙은 `SprintsService.start()` 안에 있다.

### 분업 원칙
| 검증 종류 | 어디에 | 예 |
|---|---|---|
| 자료 검증 | DTO | 타입, 길이, 포맷, 배열 원소 형태 |
| 의미 검증 (필드 간 관계) | Service | endDate > startDate, parent와 type의 호환성 |
| 도메인 규칙 | Service | "동시에 IN_PROGRESS 스프린트는 1개" |

### 왜 분리하는가
DTO는 "JSON 형식이 맞는가"만 본다. 두 필드 관계나 DB 상태에 의존하는 검증은 **도메인 지식이 필요**해서 서비스 책임. 검증 코드의 위치가 곧 책임의 위치.

---

## 5. 두 형태의 `$transaction` — 분기점

### 인터랙티브 형태 — 검증·분기가 필요할 때
```ts
return this.prisma.$transaction(async (tx) => {
    const cards = await tx.cards.findMany(...);
    if (cards.length !== dto.cardIds.length) throw new NotFoundException(...);
    const alreadyAssigned = cards.filter(c => c.sprintId !== null);
    if (alreadyAssigned.length > 0) throw new BadRequestException(...);
    // 조건 통과 후에만 쓰기
    const sprint = await tx.sprints.create(...);
    await tx.cards.updateMany(...);
    return ...;
});
```
- 콜백 throw → 자동 ROLLBACK
- 정상 종료 → COMMIT
- **모든 쿼리는 `tx`로 호출**(`this.prisma`로 부르면 격리 깨짐)

### 배열 형태 — 단순 다중 쓰기를 원자적으로
```ts
await this.prisma.$transaction([
    this.prisma.cards.updateMany(...),
    this.prisma.sprints.update(...),
]);
```
- 검증은 트랜잭션 밖에서 끝낸 뒤
- 안에서는 단순히 여러 쓰기를 묶는다

### 선택 기준
- 중간에 조건 분기/예외 → **인터랙티브**
- 단순 N개 쓰기 묶음 → **배열형**

---

## 6. 소프트 삭제 + 수동 cascade

### 관찰
`SprintsService.softDelete()`가 두 가지를 한다.
1. `cards.updateMany({ where: { sprintId: id }, data: { sprintId: null } })` — 카드를 백로그로
2. `sprints.update({ where: { id }, data: { deletedAt: now } })` — 스프린트는 deletedAt만 찍기

### 왜 수동인가
[schema.prisma](server-board/prisma/schema.prisma)의 `onDelete: SetNull`은 **하드 DELETE**일 때만 트리거된다. 소프트 삭제(`UPDATE deletedAt`)에는 작동하지 않으므로 **개발자가 동일한 일을 손으로** 한다.

### 비용
- 모든 조회에 `where: { deletedAt: null }`을 매번 적어야 함 (Prisma는 자동 필터 안 해줌)
- 삭제 의미를 가진 모든 작업에 `_count`도 `where: { deletedAt: null }` 옵션 추가
- 빠뜨리면 삭제된 행이 응답에 섞임 — 가장 흔한 버그

### 보상
- 복구 가능
- 감사 로그 자동 유지(`deletedAt`이 시각)

---

## 7. 응답 모양 정리 — `select` 상수 + flatten 후처리

### 패턴
```ts
const SPRINT_SELECT = {
    id: true, name: true, status: true, startDate: true, endDate: true,
    _count: { select: { cards: { where: { deletedAt: null } } } },
} as const;

function flattenCount<T extends { _count: { cards: number } }>({ _count, ...rest }: T) {
    return { ...rest, cardCount: _count.cards };
}
```

### 의도
1. **`_count`로 별도 쿼리 없이 관계 집계** — `findMany` + `cards.length`보다 효율적 (SQL `COUNT(*)`)
2. **응답 형태를 일관되게** — 클라이언트에 `{ ..., _count: { cards: 12 } }` 대신 `{ ..., cardCount: 12 }`
3. **`select` 상수화** — 같은 모양을 4개 엔드포인트가 공유 시 단일 진실
4. **`as const`** — Prisma 타입 추론 정확도 ↑, 런타임 변형 차단

### 노출 통제
`select`에 `deletedAt`, `createdAt`, `updatedAt`을 안 넣으면 클라이언트에 **내부 필드 노출 차단**. 조회 의도와 무관한 컬럼은 응답에서 빠진다.

---

## 8. NestJS 표준 예외와 HTTP 상태의 매핑

| 예외 | HTTP | 언제 |
|---|---|---|
| `BadRequestException` | 400 | 입력 형식은 맞지만 의미가 잘못됨 (endDate ≤ startDate) |
| `NotFoundException` | 404 | 리소스 없음 |
| `ConflictException` | 409 | 현재 상태와 충돌 (PLANNED 아님 / 이미 IN_PROGRESS 있음) |
| `ForbiddenException` | 403 | 권한 부족 |
| `UnauthorizedException` | 401 | 인증 실패 |

`throw` 한 번이면 NestJS가 자동으로 JSON 응답 변환:
```json
{ "statusCode": 409, "message": "...", "error": "Conflict" }
```

---

## 9. 작은 문법 메모

- `import 'dotenv/config'`는 **side-effect import** — 변수에 안 담고 부수효과만 실행. `process.env` 읽는 코드보다 위에 있어야 한다.
- `from './x.js'`는 ESM 모드(`"type": "module"`) 때문에 소스에 `.js`로 적는다. 자세한 건 [[esm-import-rule]].
- `private readonly prisma: PrismaService` 단축 생성자 매개변수 — 클래스 필드 선언 + DI 요청을 한 줄에. `private`/`readonly` 빠지면 단순 매개변수가 됨.
- `void bootstrap()` — async 함수 호출 결과 Promise를 의도적으로 await하지 않는다는 표시. floating promise 경고 회피.

---

## 다음 단계

- Step 5: `cards/` — 가장 복잡한 도메인. 자기참조(parent/children), 다중 필터(`cards-filter.dto.ts`), `key` 자동 생성 패턴.
- 그 후 프론트엔드 `src/lib/api/*.ts` ↔ 백엔드 컨트롤러 매핑을 직접 비교.

---

## 출처

- 정독 대상: [[server-board]] (`feat/frontend-integration` 브랜치, 2026-04-27 시점)
- 학습 흐름: [[Board 풀스택 학습 가이드]]
