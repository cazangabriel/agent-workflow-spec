## 8 continued — ceremony, friction, generated board, TA tickets

Read after [08-tickets.md](08-tickets.md).

### 8.8 Kanban ceremony (cannot be skipped)

A ceremony is a fresh KanbanMaster-cold instance. It is not optional: new `backlog` tickets come from this role reading the design spec, not from magic. If `backlog` and `open` run dry, work stops until a ceremony runs.

When moving `backlog → open`, cluster related tickets: dependents together, or tickets that share a module (and nearby layers), so the same Worker-cold instance can be reused (§5.11).

The ceremony is more than two column moves. The Kanban master:

- Reads `project-design-spec*.md` (read-only) and breaks remaining work into `backlog` tickets (component-level order from the spec, not inventing a new architecture).
- Moves `backlog → open` up to the cap of 10.
- Reviews `in-progress`, `blocked`, `done`, and comments, including friction counters.
- For blockers: options to the human if known; else escalate; design-level gaps become TA tickets (problem only).
- Moves `done → closed` when implementation is accepted and living-knowledge (not the design spec) is updated if a recurring gotcha earned a sentence.
- Regenerates `/app-name-1/kanban.md` from ticket files (§8.10).
- Must not edit the design spec.

Ticket breakdown happens **after** the human accepts a Phase 1 design spec. Weak spec ⇒ weak tickets; fix the spec via TA.

### 8.9 Friction counter (on the ticket, not in living-knowledge)

`friction` is an integer field on the ticket, default **0**, no upper bound. It is **not** a 1–5 opinion score.

On each friction encounter the **worker increments by 1** and leaves one concise comment describing that fail (stop-rule, test fail, wrong implementation, user/architect had to intervene, hit-and-miss). Four fails ⇒ four comments and `friction += 4`.

The counter is shown on the kanban board row for that ticket.

The Kanban master **looks** at friction during ceremony. High friction + a pattern that will recur may justify a short living-knowledge gotcha. High friction on a one-off does not. The counter is not a compiler that writes files.

This counter is also the reliability signal if, later, the human considers letting workers spawn the Design architect. Not authorised today.

### 8.10 Kanban board file (generated)

Path: `/app-name-1/kanban.md`.

The human does not hand-maintain this file. **Source of truth is the ticket files** (folder = state; body = title, friction, created-by, depends-on). `kanban.md` is **generated** from that by the agent that last moved or edited a ticket. A newly spawned Worker-cold shows this generated board first.

Board rows show at least: ticket code, title, folder/state, friction, created-by.

### 8.11 Extra work discovered mid-ticket (no worker-created tickets)

Workers do **not** create tickets, including “obvious” follow-ons.

Path: Worker-cold writes a self-contained comment on the **current** ticket describing the extra work → human spawns KanbanMaster-cold → KanbanMaster-cold creates the new ticket with board-level judgment (and refuses to shrink a design-level coupling problem into a cute ticket). Longer than letting the worker open a ticket. Safer.

### 8.12 Ticket body fields (decided)

Every ticket file includes:

- `Created by:` agent image name
- `DateTimeCreated:`
- `TicketType:` unabbreviated types, e.g. `Technical Architecture` or `Database, Data Model, UI`
- `friction:` integer, default 0
- `Depends on:` list of ticket code + title (may include `blocked by:`)
- State field matching the **folder**
- Human title and description
- Comments log

### 8.13 TA tickets (Design architect’s inbox)

Kind prefix `TA`. Same shared increment as `F-` tickets, same column-folder scheme. Example path: `/app-name-1/tickets/open/TA-13.md`.

- Only KanbanMaster-cold creates them.
- Worker never creates them. Worker blocks and comments; Kanban master decides whether a TA is warranted.
- Content: problem, impact, needs, enough context to solve **without** reading a non-TA ticket. No recommended design solution.
- May depend on a non-TA ticket; DesignArchitect-warm must not follow that link unless the TA is incomplete (then that is a Kanban-master writing defect).
- May depend on other TA tickets; Design architect may read those.
- Design architect marks the TA `done` and leaves a concise solution comment. Kanban master later closes it in ceremony after reflecting any spec change the Design architect already wrote.
