# Tech Stack Decision

## Decision

AgencyOS should move from the current Vite/localStorage prototype to a **boring full-stack TypeScript modular monolith**:

- **Framework:** Next.js App Router
- **Language:** TypeScript
- **Database:** PostgreSQL
- **ORM:** Prisma first
- **Auth:** Auth.js
- **UI:** Tailwind CSS + small internal component system, optionally shadcn-style primitives later
- **Validation:** Zod
- **Testing:** Vitest for domain/unit tests, Playwright for happy-path E2E
- **Deployment:** Vercel for public demo, Docker Compose for self-hosted local/open-source deployments
- **Package structure:** start as one app; split into packages only when needed

## Why this stack

### Next.js

AgencyOS is no longer just a static SPA. It needs routes, auth, server actions/API routes, database-backed pages, deployment previews, and eventually a public demo. Next.js is a React full-stack framework, so it lets us keep React while adding backend capability without creating a separate API service too early.

### PostgreSQL

AgencyOS has relational data: workspaces, users, customers, projects, project members, tickets, time entries, comments, invoices, and reports. PostgreSQL is the right default for this.

### Prisma

Prisma is the recommended first ORM because it gives:

- clear schema file
- generated type-safe client
- migrations
- easy seed scripts
- Prisma Studio for contributors
- broad familiarity among open-source contributors

Drizzle remains a good alternative if we later want more SQL-like control and lighter abstractions.

### Auth.js

Auth.js supports common auth methods such as OAuth, email magic links, credentials, and passkeys, and has database adapter support. It is a practical fit for GitHub/Google login and workspace membership.

### Tailwind + internal components

The current custom CSS is fine for prototype speed, but product velocity will improve if we move toward reusable components. Tailwind keeps styling close to components, and an internal component layer avoids importing a large design system too early.

## Alternatives considered

### Keep Vite SPA + separate API

Pros:
- small frontend
- flexible backend choice

Cons:
- more moving parts early
- separate routing/auth/API coordination
- worse for open-source one-command setup

Decision: defer. Use Next.js first.

### Drizzle instead of Prisma

Pros:
- lightweight
- SQL-like
- great performance/control

Cons:
- Prisma is more approachable for many contributors
- Prisma Studio/migration workflow is friendlier for first public version

Decision: Prisma first, Drizzle remains viable later.

### Supabase as primary backend

Pros:
- fast auth/database/storage setup
- generous hosted path

Cons:
- self-hosting story becomes more specific
- open-source contributors need Supabase knowledge

Decision: use plain Postgres-compatible architecture. Supabase can be a deployment option, not the core assumption.

### Microservices

Rejected. AgencyOS is too early. A modular monolith is simpler and safer.

## Target app structure

Initial Next.js structure:

```text
agencyos/
  app/
    (app)/
      dashboard/
      projects/
      projects/[projectId]/
      tickets/
      time/
      reports/
      customers/
      team/
      settings/
    api/
      export/
    layout.tsx
    page.tsx
  src/
    features/
      customers/
      projects/
      tickets/
      time/
      reports/
      team/
    components/
      ui/
      layout/
    lib/
      auth.ts
      db.ts
      validation.ts
    domain/
      calculations.ts
      permissions.ts
  prisma/
    schema.prisma
    seed.ts
  tests/
    e2e/
  docs/
```

Future monorepo split only if needed:

```text
apps/web
packages/domain
packages/db
packages/ui
```

## Initial database model

Core entities:

- Workspace
- User
- WorkspaceMember
- Customer
- Project
- ProjectMember
- Ticket
- TimeEntry
- Comment
- Attachment later
- ReportPreset later

## Migration plan from current prototype

### Phase 1: Stabilize current prototype

- Add edit/delete/detail drawers.
- Add report filters and date ranges.
- Add happy-path E2E test.
- Split `src/App.tsx` by feature so migration is easier.

### Phase 2: Create Next.js shell

- Create Next.js app structure.
- Port current UI into route-based pages.
- Keep local seed data temporarily.
- Preserve current visual style.

### Phase 3: Add database

- Add PostgreSQL + Prisma.
- Add schema and seed data.
- Replace localStorage reads/writes with server actions/API calls.
- Add `.env.example` and Docker Compose.

### Phase 4: Add auth/workspaces

- Add Auth.js.
- Add workspace membership roles.
- Add basic permissions:
  - owner/admin
  - project manager
  - member
  - viewer

### Phase 5: Productionize open-source setup

- Add GitHub Actions CI.
- Add Playwright E2E.
- Add contribution guide.
- Add demo seed/reset path.
- Add deployment docs for Vercel and Docker.

## Non-goals for now

- Microservices
- Complex role permission DSL
- Queue workers
- AI features
- Mobile app
- Full invoicing/accounting
- Heavy CRM pipeline
