# AgencyOS UX Product Vision

AgencyOS should feel like a premium startup product, not an admin database. The product promise is:

> A calm, beautiful agency cockpit where projects, people, tickets, and time feel connected.

## UX bar

Every core workflow should feel:

- **Fast** — obvious next action, minimal typing, smart defaults.
- **Premium** — motion, hierarchy, polished empty states, no cramped forms.
- **Operational** — connects people, time, budget, and customer context.
- **Trustworthy** — clear data persistence, undo/confirm destructive actions, visible status.
- **Open-source friendly** — still simple enough to self-host and contribute to.

## Priority experience tracks

### 1. Premium project creation

Creating a project should feel like starting a new agency engagement, not filling a spreadsheet row.

Target flow:

1. Slide-over command panel opens from the right.
2. User chooses or creates a customer.
3. User enters project essentials: name, status, budget, hourly rate, dates.
4. User assigns lead and team with capacity/rate context.
5. User optionally creates starter tickets/phases.
6. User lands in the new project cockpit with next actions.

Premium details:

- animated slide-over instead of basic modal
- sticky summary panel showing budget, team capacity, and estimated hours
- smart defaults from customer/team rates
- inline validation
- `Create project` button becomes a progress state
- success state transitions directly into project cockpit

Acceptance criteria:

- [ ] Project creation uses a slide-over/drawer interaction.
- [ ] Team assignment is part of project creation.
- [ ] Budget/time summary updates while creating.
- [ ] New project opens immediately after creation.
- [ ] Mobile project creation remains usable.

### 2. Employee time and capacity overview

Team members should not be a static list. Each employee should show workload, capacity, and time.

Target surfaces:

- Team overview cards with capacity, assigned tickets, logged hours, billable ratio.
- Employee detail view with assigned projects, open tickets, weekly timesheet, utilization trend.
- Project cockpit team panel showing each member's planned vs logged time.

Acceptance criteria:

- [ ] Team page shows capacity and utilization at a glance.
- [ ] Project cockpit shows project members with logged hours and open work.
- [ ] Employee detail or drawer connects tickets, time, projects, and rate.
- [ ] Overloaded/underbooked states are visually clear.

### 3. Drag-and-drop ticket board

Tickets should feel like Trello-level direct manipulation, with agency-specific context.

Target behavior:

- drag ticket cards between Backlog, Todo, In Progress, Review, Done
- reorder within columns
- preserve keyboard/select fallback for accessibility
- update ticket status immediately with optimistic feedback
- show assignee, estimate, logged time, due date, priority

Acceptance criteria:

- [ ] Cards can be dragged between columns.
- [ ] Cards can be reordered within a column.
- [ ] Status changes persist.
- [ ] Keyboard/select fallback remains available.
- [ ] Mobile has a usable fallback if drag/drop is awkward.

### 4. Time logging logic

Time tracking is core product value, not an add-on.

Target workflows:

- One-click timer from a project or ticket.
- Manual time entry with project, ticket, employee, billable flag, note, duration/date.
- Weekly timesheet overview by person.
- Project time overview: logged vs estimated vs budget.
- Customer rollup: hours and revenue across projects.

Acceptance criteria:

- [ ] Timer can start/stop on project or ticket.
- [ ] Manual entries can be added, edited, deleted.
- [ ] Project cockpit shows logged vs estimated time.
- [ ] Team page shows weekly logged time by employee.
- [ ] Reports use real time entry data and filters.

### 5. Customer cockpit / CRM-lite

AgencyOS should learn from CRM products without becoming HubSpot-heavy.

Target surfaces:

- Customer detail with projects, active tickets, logged hours, account health, revenue target.
- Activity timeline for important project/customer events.
- Follow-up reminders later.

Acceptance criteria:

- [ ] Customer detail shows project and time rollups.
- [ ] Customer health is visible and editable.
- [ ] Customer view suggests next operational action.

## Reference product lessons

- MOCO: project/time/money should be connected; time tracking needs control views like remaining hours, target/actual, billable/non-billable, and person/project/client timesheets.
- Trello: cards and lists should support direct drag-and-drop movement and simple visual organization.
- Clockify: timer, manual entry, billable status, weekly timesheets, and reports are core expectations.
- HubSpot: records should centralize relationship context, activities, tasks, and pipeline/status information.

## Near-term implementation order

1. Project creation drawer + team assignment summary.
2. Employee/project time overview cards.
3. Drag/drop ticket board with accessible fallback.
4. Timer + manual time-entry CRUD.
5. Project/customer/employee detail drawers.

This should be built in small PRs with QA review after each slice.
