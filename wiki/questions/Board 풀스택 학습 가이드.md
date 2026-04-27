---
type: synthesis
title: "Board 풀스택 학습 가이드"
created: 2026-04-27
updated: 2026-04-27
tags:
  - synthesis
  - learning
  - fullstack
  - nestjs
  - nextjs
  - prisma
status: developing
question: "백엔드(/Users/admin/server-board)와 프론트(/Users/admin/board)의 구조와 연결을 익히려면 어떤 순서로 폴더를 보고, 각 코드의 기능과 문법을 어떻게 이해해야 하는가?"
answer_quality: solid
related:
  - "[[server-board]]"
  - "[[fullstack-dev]]"
  - "[[NestJS]]"
  - "[[Prisma]]"
  - "[[PostgreSQL]]"
  - "[[TypeScript]]"
  - "[[esm-import-rule]]"
sources:
  - "[[server-board-repo]]"
---

# Board 풀스택 학습 가이드

[[server-board]] (백엔드)와 board (프론트엔드, Next.js 16) 두 저장소를 처음 읽는 사람을 위한 통합 안내. 폴더를 어떤 순서로 따라가야 머릿속에 그림이 그려지는지 + 자주 등장하는 문법의 의미.

## 1. 전체 그림 — 어떻게 연결되는가

```
[ Browser (React 19) ]
       │  fetch('/cards?sprintId=…')   ← src/lib/api/client.ts (Next)
       ▼
[ NestJS HTTP Server :4000 ]   ← server-board/src/main.ts
       ├─ Controller (라우팅)
       ├─ Service    (비즈니스 로직)
       └─ PrismaService (ORM)
       ▼
[ PostgreSQL ]
```

연결고리는 단 하나다.

- 프론트의 `src/lib/api/client.ts` 가 `process.env.NEXT_PUBLIC_API_BASE_URL` (기본 `http://localhost:4000`) 으로 `fetch` 호출.
- 백엔드의 `src/main.ts` 가 `enableCors()` 로 그 origin 을 허용.
- 양쪽 타입은 **수동 동기화**. 백엔드 Swagger `cardSummarySchema` ↔ 프론트 `src/types/board.ts` 의 `Ticket` 인터페이스.

타입 단일 소스가 없으므로 백엔드 응답 모양이 바뀌면 프론트 `types/board.ts`도 같이 손봐야 한다.

---

## 2. 백엔드 (server-board) — NestJS + Prisma

### 폴더 구조

```
server-board/
├── prisma/
│   ├── schema.prisma         ← DB 모델 (진실의 원천)
│   ├── seed.ts               ← 초기 데이터
│   ├── prisma.module.ts      ← Nest 전역 DB 모듈
│   └── prisma.service.ts     ← PrismaClient 래퍼 (DI용)
└── src/
    ├── main.ts               ← 부트스트랩 (CORS, ValidationPipe, Swagger)
    ├── app.module.ts         ← 루트 모듈
    └── board/
        ├── board.module.ts   ← 도메인 묶음
        ├── workflows/        ← 칼럼 (TO DO, IN PROGRESS …)
        ├── sprints/          ← 스프린트
        └── cards/            ← 이슈 카드
            ├── *.controller.ts  (HTTP 라우트)
            ├── *.service.ts     (DB 로직)
            ├── *.module.ts      (DI 묶음)
            └── dto/              (요청 검증 클래스)
```

### 학습 순서

요청이 흘러가는 방향 그대로.

| # | 파일 | 보는 이유 |
| --- | --- | --- |
| 1 | `prisma/schema.prisma` | DB 구조 = 도메인 모델. 모든 코드의 출발점. |
| 2 | `prisma/prisma.service.ts` | DB 연결이 어디서 만들어지는가. |
| 3 | `src/main.ts` | 서버가 어떻게 떠서 어디서 듣는가. |
| 4 | `src/app.module.ts` → `board/board.module.ts` | 모듈 트리. |
| 5 | `board/workflows/*` | GET-only — NestJS 패턴 첫 습득용. |
| 6 | `board/sprints/*` | 트랜잭션·검증·상태 전이가 들어간 중급. |
| 7 | `board/cards/*` | 필터·정렬·소프트 삭제·외래키 검증 종합편. |

### 핵심 문법 (NestJS)

**데코레이터** — "이건 어떤 역할이야"라고 마킹. 자바 어노테이션과 같은 개념.

```ts
@ApiTags('cards')          // Swagger 그룹명
@Controller('cards')       // "/cards" 라우트 묶음
export class CardsController {
  constructor(private cardsService: CardsService) {}   // DI: Nest가 자동 주입

  @Get()                                       // GET /cards
  findAll(@Query() query: CardsFilterDto) { ... }     // ?sprintId=… 자동 파싱+검증

  @Post()                                      // POST /cards
  create(@Body() dto: CreateCardDto) { ... }   // body → DTO 인스턴스로 변환+검증

  @Delete(':id')
  @HttpCode(204)
  async remove(@Param('id', ParseIntPipe) id: number) { ... }
  //                       ^ 문자열 → 숫자 변환 + 실패 시 400
}
```

**DTO + class-validator** — 요청을 클래스로 받아 자동 검증.

```ts
export class CreateCardDto {
  @IsString() @Length(1, 200)
  title!: string;                  // ! = "확실히 들어옴" 단언

  @IsInt()
  workflowId!: number;

  @ValidateIf((_, v) => v !== null)  // null이면 검증 스킵
  @IsInt()
  sprintId!: number | null;
}
```

전역 검증이 켜진 곳: `main.ts` 의 `app.useGlobalPipes(new ValidationPipe({ transform: true, whitelist: true }))`. `whitelist: true` = DTO에 없는 필드는 자동 제거.

**Service + Prisma**

```ts
@Injectable()
export class CardsService {
  constructor(private prisma: PrismaService) {}

  async findAll(filter: CardsFilterDto) {
    const where: Prisma.CardsWhereInput = { deletedAt: null };  // soft delete
    // ... 조건 분기
    const cards = await this.prisma.cards.findMany({
      where,
      orderBy: [{ workflowId: 'asc' }, { order: 'asc' }],
      select: CARD_SELECT,         // 응답 모양을 한 곳에서 고정
    });
    return cards.map(flattenWorkflow);   // { workflow:{status} } → { status }
  }
}
```

**트랜잭션** — `this.prisma.$transaction(async (tx) => { ... })`. 안에서 실패하면 모두 롤백. `sprints.service.ts` 의 `create`/`start` 가 대표 예시.

### Prisma 핵심 문법

```prisma
model Cards {
  id         Int       @id @default(autoincrement())
  deletedAt  DateTime? @db.Timestamptz(3)         // ? = nullable
  workflowId Int                                   // FK 컬럼
  workflow   Workflows @relation(fields:[workflowId], references:[id], onDelete: Restrict)
  // ↑ 관계 (실제 DB 컬럼 아님, JS 객체 탐색용)
  children   Cards[]   @relation("CardToCard")    // 자기 참조 1:N
  @@index([workflowId])                            // 쿼리 인덱스
}
```

`onDelete` 의미:
- `Restrict` — 자식 있으면 부모 삭제 불가
- `SetNull` — 부모 지우면 자식 FK는 NULL
- `Cascade` — 부모 지우면 자식 같이 삭제

소프트 삭제 패턴: `deletedAt` 컬럼만 채우고, 모든 조회에 `where: { deletedAt: null }` 을 넣는다 (현재 [[server-board]] 전반의 규칙).

ESM import 경로 규칙은 [[esm-import-rule]] 참조 — 이 프로젝트 모든 import 가 `.js` 확장자로 끝남.

---

## 3. 프론트엔드 (board) — Next.js 16 + React 19

### 폴더 구조

```
board/src/
├── app/                          ← Next.js App Router (파일 = 라우트)
│   ├── layout.tsx                ← 모든 페이지 공통 HTML 셸
│   ├── page.tsx                  ← "/" — 활성 스프린트 보드
│   └── backlog/page.tsx          ← "/backlog"
├── components/                   ← Atomic Design
│   ├── atoms/                    ← Avatar, PriorityDot, TypeIcon …
│   ├── molecules/                ← BacklogRow, FilterRow, TabBar
│   ├── organisms/                ← TicketCard, Column, Modal …
│   ├── templates/                ← AppShell, BoardView, BacklogPanel …
│   └── ui/                       ← shadcn/ui 원시 컴포넌트
├── lib/api/                      ← 백엔드 호출 (fetch 래퍼)
├── hooks/use-board-ui.ts         ← Context 소비 훅
└── types/board.ts                ← 도메인 타입 (백엔드 응답과 1:1)
```

### 학습 순서

| # | 파일 | 보는 이유 |
| --- | --- | --- |
| 1 | `src/types/board.ts` | 도메인 모델, 백엔드 응답과 매칭. |
| 2 | `src/lib/api/{client,cards,sprints,workflows}.ts` | "백엔드를 어떻게 부르는가". |
| 3 | `src/app/layout.tsx` | App Router 의 루트. |
| 4 | `src/app/page.tsx` | 서버 컴포넌트 데이터 페칭 진입점. |
| 5 | `templates/AppShell.tsx` | 클라이언트 셸 + Context 패턴. |
| 6 | `BoardView` → `Column` → `TicketCard` → atoms | 합성 흐름. |
| 7 | `app/backlog/page.tsx` → `templates/BacklogPanel.tsx` | 다중 fetch + 클라이언트 상태 + mutation. |
| 8 | atoms / molecules | 작은 조각의 재사용 패턴. |

### 핵심 문법 (Next.js / React)

**서버 컴포넌트 vs 클라이언트 컴포넌트**

`'use client'` 디렉티브가 없으면 그 파일은 서버에서 실행된다. 데이터 fetch·SEO·민감 코드 → 서버. `useState`/`onClick`/`useEffect` 가 필요 → `'use client'`.

```tsx
// app/page.tsx — 서버에서 실행
export default async function Page() {
  const [sprints, workflows] = await Promise.all([
    getSprints(), getWorkflows(),
  ]);
  return <AppShell ...><BoardView ... /></AppShell>;
}

// AppShell.tsx — 브라우저에서 실행
'use client';
import { useState, createContext } from 'react';
```

**Context 패턴** — 자식 어디서든 모달 띄우고 싶을 때 props 줄줄이 내려보내지 않고 사용.

```tsx
export const BoardUIContext = createContext<BoardUIContextValue | null>(null);

export function AppShell({ children }) {
  const value = { openCreate, openTicket, closeTicket };
  return <BoardUIContext.Provider value={value}>{children}</BoardUIContext.Provider>;
}

// hooks/use-board-ui.ts
export function useBoardUI() {
  const ctx = useContext(BoardUIContext);
  if (!ctx) throw new Error('useBoardUI must be inside <AppShell/>');
  return ctx;
}
```

**mutation 후 새로고침** — `router.refresh()` 가 서버 컴포넌트를 다시 페치한다 (페이지 깜빡임 없음).

```tsx
const router = useRouter();
const handleSubmit = async (name) => {
  await createSprint({ name, cardIds });
  router.refresh();
};
```

**fetch 래퍼** — 모든 API 호출이 한 군데에서 에러·204·JSON 처리.

```ts
export async function api<T>(path: string, init?: RequestInit): Promise<T> {
  const res = await fetch(`${BASE}${path}`, {
    cache: 'no-store',
    headers: { 'Content-Type': 'application/json', ...init?.headers },
    ...init,
  });
  if (!res.ok) throw new ApiError(res.status, ...);
  if (res.status === 204) return undefined as T;
  return res.json() as Promise<T>;
}
```

`<T>` = 제네릭. 호출 측에서 응답 타입 명시: `api<Ticket[]>(...)`.

**Atomic Design 흐름**

```
page.tsx ─ data fetch
  └─ AppShell (template, 클라이언트 셸·Context)
      └─ BoardView (template, 데이터 분배)
          └─ Column (organism, status별 카드 묶음)
              └─ TicketCard (organism, 한 카드)
                  ├─ TypeIcon (atom)
                  ├─ PriorityDot (atom)
                  └─ EstimateChip (atom)
```

---

## 4. 추천 학습 흐름 (양쪽 합쳐서)

**1주차 — 데이터 기반 잡기**
1. `schema.prisma` 정독 → ERD를 종이에 그려본다.
2. `seed.ts` 로 초기 데이터 확인.
3. 백엔드 `npm run start:dev` → `http://localhost:4000/docs` (Swagger) 에서 API 직접 호출.

**2주차 — 백엔드 한 모듈 끝까지**
1. workflows → sprints → cards 순.
2. 각 모듈마다 module → controller → service → dto 순서.
3. 흐름: Swagger 요청 → 데코레이터 → service → Prisma → 응답 매핑.

**3주차 — 프론트 데이터 라인**
1. `types/board.ts` ↔ 백엔드 응답 schema 비교.
2. `lib/api/*` 변환 흐름.
3. `app/page.tsx` 서버 페칭.

**4주차 — UI 합성**
1. `templates/AppShell.tsx` 의 Context/state.
2. `BoardView` → `Column` → `TicketCard` → atoms.
3. `BacklogPanel` 의 mutation + `router.refresh()`.

**자가 검증 과제**
- Swagger 에서 `POST /cards` → 보드 페이지 새로고침 → 카드 보이는지.
- 코드만 보고 "스프린트 생성 버튼을 누르면 어떤 함수가 어떤 순서로 호출되는가" 6단계로 적기 (BacklogPanel → createSprint → api → fetch → SprintsController → SprintsService).

---

## 5. 헷갈릴 만한 문법 빠른 참조

| 문법 | 의미 |
| --- | --- |
| `field?: T` | optional |
| `field!: T` | "확실히 있음" 단언 (DTO에서 흔함) |
| `as const` | 리터럴 타입 좁히기 |
| `Promise.all([...])` | 병렬 await |
| `Map`, `Set` | O(1) 조회 자료구조 |
| `cn(a, b)` | className 조건부 합치기 |
| `@Injectable()` | NestJS DI 가능 클래스 표시 |
| `@Module({...})` | Nest 의존성 묶음 단위 |
| `this.prisma.$transaction` | 여러 쿼리 원자적 실행 |
| `'use client'` | 그 파일은 브라우저에서 실행 |

## 관련

- [[server-board]] — 백엔드 프로젝트 허브
- [[fullstack-dev]] — 도메인 허브
- [[esm-import-rule]] — 백엔드에 적용된 ESM import 규칙
- [[Prisma Client 재생성 누락 시 P2022]] — 스키마 변경 시 주의점
