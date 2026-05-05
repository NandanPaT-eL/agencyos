# AgencyOS Implementation Roadmap

## Product north star

AgencyOS should become the simplest open-source operating system for small consulting agencies: one place to manage clients, projects, tickets, team capacity, time, and eventually billing — without enterprise bloat.

The next goal is not “more prototype features.” It is to turn the localStorage prototype into a credible multi-user agency workspace.

## Loop 1 — Foundation: auth, workspace, backend persistence

### Why

This is the credibility gap. Without auth, workspaces, and a real database, AgencyOS remains a demo.

### Scope

- Next.js app shell
- PostgreSQL + Prisma
- Auth.js login/logout
- Workspace model
- Workspace membership model
- Core schema:
  - users
  - workspaces
  - memberships
  - customers
  - projects
  - project members
  - tickets
  - time entries
- Seed/demo data
- Protected app routes
- Replace localStorage as source of truth

### Acceptance criteria

- User can sign in and sign out.
- User lands inside a workspace.
- Workspace-scoped data persists in database.
- Projects, customers, tickets, team, and time load from DB.
- Empty workspace state is handled gracefully.
- Seed data can create a useful demo workspace.
- LocalStorage is no longer the main data store.

## Loop 2 — Core CRUD and detail views

### Why

Lists are not enough. Real users need to inspect, edit, correct, and delete operational data.

### Scope

- Detail pages/drawers for:
  - projects
  - customers
  - tickets
  - team members
- Create/edit/delete flows
- Form validation
- Delete confirmations
- Assignment updates
- Relationship views:
  - customer → projects/tickets/time
  - project → tickets/time/team/customer
  - team member → assigned work/time

### Acceptance criteria

- User can create/edit/view/delete customers.
- User can create/edit/view/delete projects.
- User can create/edit/view/delete tickets.
- User can assign tickets/projects to team members.
- Forms validate required fields.
- Empty/error/loading states exist.

## Loop 3 — Time tracking and operational workflow

### Why

Consulting agencies live on time, delivery, and capacity. This loop makes AgencyOS agency-specific instead of generic project management.

### Scope

- Start/stop timer
- Manual time entry CRUD
- Weekly timesheet view
- Time entries linked to project/customer/ticket/user
- Daily/weekly totals
- Ticket activity notes/comments lightweight version
- Own time vs workspace time filters

### Acceptance criteria

- User can start/stop a timer on a project or ticket.
- User can manually add/edit/delete time entries.
- Time entries roll up to project, customer, team member, and week.
- Timesheet page shows daily and weekly totals.
- Reports use real time-entry data.
- Tickets support basic notes/activity.

## Loop 4 — Reporting, onboarding, and polish

### Why

After the core workflow works, AgencyOS needs to explain itself and surface decision-making value quickly.

### Scope

- Setup/onboarding wizard
- Better dashboard
- Report filters/date ranges
- Customer/project profitability-lite views
- Improved empty states
- Sample workspace option
- Navigation cleanup and visual polish

### Acceptance criteria

- New user can set up workspace, first customer, first project, and optional sample data.
- Dashboard shows meaningful real data.
- Reports filter by date range, customer, project, and team member.
- Time totals and project summaries are accurate.
- User understands next action within 2 minutes of signup.

## Deferred until after credible multi-user workflow

- Invoicing
- Retainers
- Payment integrations
- Advanced permissions
- Drag/drop boards
- File attachments
- Deep comments/activity streams
- Notifications
- Public API
- Complex utilization/profitability analytics
- Mobile app
- Marketplace/plugins

## Immediate next build task

Start Loop 1:

> Create a Next.js + PostgreSQL + Prisma + Auth.js foundation and migrate the current customers/projects/tickets/team/time prototype from localStorage into workspace-scoped database-backed data.

This unlocks every later loop and avoids wasting effort polishing temporary state management.
