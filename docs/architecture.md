# AgencyOS Architecture

## Architecture decision

AgencyOS will evolve into a **boring full-stack TypeScript modular monolith**.

Recommended target stack:

- **Framework:** Next.js App Router + TypeScript
- **Database:** PostgreSQL
- **ORM:** Prisma
- **Auth:** Auth.js / NextAuth with Prisma adapter
- **UI:** Tailwind CSS + shadcn/Radix-style primitives
- **Forms/validation:** React Hook Form + Zod
- **Testing:** Vitest, React Testing Library, Playwright
- **Deployment:** Vercel for hosted demo, Docker Compose for self-hosting
- **Storage later:** S3-compatible object storage for attachments/files/invoices

## Principle

Build one boring modular monolith first. AgencyOS should be easy for open-source contributors to run, understand, and extend.

Avoid microservices, custom auth, complex permission engines, and heavy automation until the core agency workflow is proven.

## Current prototype

- React + Vite
- Browser localStorage persistence
- Domain logic in `src/domain.ts`
- UI in `src/App.tsx`
- No backend yet

This is intentionally fast for product discovery, not the final architecture.

## Why Next.js

AgencyOS now needs route-based product screens, auth, workspaces, server-side permissions, database writes, reporting queries, invite flows, and shareable URLs. Next.js gives us React plus backend capability without introducing a separate API service too early.

## Why PostgreSQL

The domain is relational:

- users
- workspaces
- customers
- projects
- project members
- tickets
- time entries
- budgets
- invoices later
- permissions and memberships

PostgreSQL is the safest default for this kind of product.

## Why Prisma first

Prisma is the first ORM choice because it offers:

- readable schema
- type-safe generated client
- migrations
- seed scripts
- Prisma Studio
- familiar contributor experience
- strong Auth.js examples

Drizzle remains a good alternative later if AgencyOS needs more SQL-like control or lighter abstractions.

## Target folder/module structure

```text
agencyos/
  app/
    (marketing)/
      page.tsx
    (app)/
      dashboard/
      customers/
      projects/
      projects/[projectId]/
      tickets/
      time/
      reports/
      team/
      settings/
    api/
      auth/[...nextauth]/route.ts
      export/
  components/
    ui/
    layout/
    forms/
    data-table/
    drawers/
  features/
    auth/
    workspace/
    customers/
      actions.ts
      queries.ts
      schema.ts
      components/
    projects/
    tickets/
    time-tracking/
    reports/
    team/
  lib/
    auth.ts
    db.ts
    permissions.ts
    validation.ts
    dates.ts
    money.ts
  prisma/
    schema.prisma
    migrations/
    seed.ts
  tests/
    unit/
    e2e/
    fixtures/
  public/
  docker-compose.yml
  .env.example
```

## Core modules

### Workspace/Auth

Multi-tenant boundary. Every customer, project, ticket, and time entry belongs to a workspace.

### Customers

CRM-lite account records. Customers own projects and reporting context.

### Projects

Anchor object. Contains customer link, owner/lead, budget, rate, status, dates, team, tickets, and time.

### Tickets

Trello-like execution layer. Tickets move through statuses and are assigned to colleagues.

### Time Tracking

Clockify-like time entries against projects and optional tickets.

### Reports

Aggregations over time, project, customer, team, billable status, and budget.

### Team

Users, roles, capacity, rates, project memberships, assignments, and workload.

## Initial database entities

- User
- Workspace
- Membership
- Customer
- Project
- ProjectMember
- Ticket
- TimeEntry
- Tag
- Invite
- ReportPreset later

Later:

- Comment
- Attachment
- Invoice
- Expense
- Retainer
- AuditLog
- API token / webhook

## Permission model v1

Keep it simple:

- **Owner/Admin:** manage workspace, billing/settings, all data
- **Manager:** manage projects, customers, team assignments, reports
- **Member:** manage assigned tickets and own time entries
- **Viewer:** read-only

Every query/mutation must be workspace-scoped.

## Reporting model v1

Start with Postgres aggregation queries:

- date range
- project
- customer
- user
- billable/internal
- budget used
- revenue estimate

Do not add analytics pipelines or materialized summaries until query performance demands it.

## Open-source requirements

- `.env.example`
- Docker Compose for app + Postgres
- seed script
- migrations
- contribution guide
- issue and PR templates
- CI for lint, test, build
- Playwright happy-path test
- public demo seed/reset path

## Migration plan

### Phase 1 — stabilize prototype

- Add edit/delete/detail drawers.
- Add report filters and date ranges.
- Add weekly time view.
- Add one Playwright happy-path E2E test.
- Split `src/App.tsx` by feature to make migration easier.

### Phase 2 — create Next.js shell

- Create Next.js App Router app.
- Port current product screens as routes.
- Add Tailwind and component primitives.
- Keep mock/local seed data temporarily.

### Phase 3 — add PostgreSQL + Prisma

- Define Prisma schema.
- Add migrations.
- Add seed data.
- Add Docker Compose and `.env.example`.
- Replace localStorage gradually with DB-backed queries/mutations.

### Phase 4 — add Auth.js + workspaces

- Add login.
- Add workspace creation.
- Add memberships and roles.
- Protect app routes.
- Enforce workspace scoping in data access.

### Phase 5 — productionize

- Add CI.
- Add Playwright.
- Add public demo mode.
- Add deployment docs.
- Add contribution guide.

## Risks and mitigations

- **Next.js complexity:** mitigate with clear feature folders and simple conventions.
- **Multi-tenancy bugs:** add workspace-scoped helpers early.
- **Prisma limitations:** use raw SQL for advanced reports if needed.
- **Auth config friction:** provide `.env.example`, seed docs, and local setup instructions.
- **Large component files:** split by feature before migration.
