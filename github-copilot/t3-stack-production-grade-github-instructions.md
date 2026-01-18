# The T3 Stack — Team Instructions

**“The Modern Standard”**

This document defines **how we build, review, and maintain** applications using **The T3 Stack**.
These rules are mandatory for all contributors unless explicitly approved otherwise.

---

## 1. What We Mean by “The T3 Stack”

Our standard stack consists of:

* **Next.js (App Router preferred)**
* **TypeScript (strict)**
* **tRPC** for type-safe APIs
* **Prisma** for database access
* **NextAuth (Auth.js)** for authentication
* **Tailwind CSS** for styling
* **Zod** for validation

> Principle: **End-to-end type safety over everything else**

---

## 2. Core Engineering Principles (Non-Negotiable)

* Type safety is not optional
* Validation happens at boundaries
* Business logic is not in UI components
* APIs must be predictable and documented via types
* Security defaults must be explicit
* Small, readable PRs over “big bang” changes
* Production correctness > developer convenience

---

## 3. Project Structure (Required)

We follow **structure by responsibility**, not randomness.

```txt
src/
  app/                # Next.js App Router
  server/
    api/
      routers/        # tRPC routers
      root.ts
      trpc.ts
    auth.ts           # NextAuth config
    db.ts             # Prisma client
  components/         # Reusable UI components
  hooks/              # Reusable React hooks
  lib/                # Utilities, helpers
  styles/             # Global styles
  types/              # Shared TS types
prisma/
  schema.prisma
```

Rules:

* No business logic inside `app/` pages
* No Prisma calls inside React components
* No cross-layer imports (UI → DB is forbidden)

---

## 4. TypeScript Rules (Strict Mode)

* `strict: true` is required
* No `any` (ever)
* Use `unknown` + narrowing if needed
* Public functions must have explicit return types
* Avoid complex generics unless necessary

❌ Bad

```ts
function handle(data: any) {}
```

✅ Good

```ts
function handle(data: unknown): Result {
  if (!isValid(data)) throw new Error("Invalid input");
  return transform(data);
}
```

---

## 5. API Layer (tRPC Rules)

### Router Design

* Routers represent **domains**, not pages
* Procedures are **thin**
* Business logic belongs in services (or router helpers)

```ts
export const userRouter = createTRPCRouter({
  getById: protectedProcedure
    .input(z.object({ id: z.string() }))
    .query(({ input, ctx }) => {
      return userService.getById(input.id, ctx.session.user.id);
    }),
});
```

### tRPC Rules

* All inputs validated with **Zod**
* Use `protectedProcedure` for authenticated routes
* Never expose Prisma models directly
* Never trust client input

---

## 6. Validation (Zod Everywhere)

Validation must occur at:

* API boundaries (tRPC inputs)
* Forms (client-side)
* Critical business logic (server-side)

Rules:

* No unvalidated input reaches Prisma
* No duplicate schemas — share Zod schemas when possible
* Prefer `z.enum` and `z.literal` over free strings

---

## 7. Database & Prisma Rules

### Schema Rules

* Use explicit relations
* Avoid nullable fields unless required
* Index fields used for filtering/sorting
* Add comments for non-obvious fields

### Access Rules

* Prisma is server-only
* No Prisma in UI components
* No raw SQL unless approved

### Migrations

* Every schema change requires a migration
* Never edit production data manually
* Migrations must be reviewed carefully

---

## 8. Authentication & Authorization

### Authentication

* Use **NextAuth/Auth.js**
* JWT/session handling must be explicit
* Session user object must be typed

### Authorization (Very Important)

* Auth ≠ Authorization
* Authorization checks happen **in tRPC procedures or services**
* Never trust client role flags

❌ Bad

```ts
if (user.role === "admin") {}
```

✅ Good

```ts
assertCanManageUser(ctx.session.user, targetUserId);
```

---

## 9. Frontend (React + Next.js)

### Component Rules

* Functional components only
* Components do **one thing**
* Max ~200 lines per component
* No API logic in components

### Data Fetching

* Use tRPC hooks
* Handle loading/error/empty states explicitly
* No side effects during render

### State Management

* Local state first
* Context only for cross-cutting concerns
* No Redux unless justified

---

## 10. Styling (Tailwind)

* Tailwind is the default
* No inline styles unless trivial
* Extract repeated class patterns into components
* Avoid overly complex class chains

Accessibility is required:

* Semantic HTML
* Labels for inputs
* Keyboard navigability

---

## 11. Error Handling

### Server

* Throw typed errors
* Do not leak internal messages
* Map domain errors to safe client messages

### Client

* Show user-friendly errors
* Log detailed errors only in dev

Never:

* Swallow errors
* Console.log sensitive data

---

## 12. Performance & Scalability

* Avoid unnecessary re-renders
* Memoize only when measured
* Paginate all list endpoints
* Add caching only with correct invalidation
* No premature optimization

---

## 13. Testing Expectations

Minimum:

* Unit tests for business logic
* API tests for critical tRPC procedures

Rules:

* Tests must be deterministic
* Mock external dependencies
* Bug fix = regression test

We prefer:

* `vitest` / `jest`
* Component tests for critical flows

---

## 14. Environment & Secrets

* Use `.env` files locally
* Never commit secrets
* Validate required env vars at startup
* Different envs = different configs

---

## 15. Git & PR Rules

### Branching

* `main` is protected
* Feature branches only

### PRs

* Small and focused
* Clear description
* Screenshots for UI changes
* Tests included when applicable

### Code Review Checklist

* Types correct?
* Validation present?
* Security impact?
* Prisma usage safe?
* No architectural violations?

---

## 16. What We Explicitly Avoid

* REST APIs alongside tRPC (unless approved)
* `any`
* Business logic in UI
* Unvalidated input
* Random libraries “just because”
* Silent failures
* God components
