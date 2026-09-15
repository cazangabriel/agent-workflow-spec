## 8. Tickets

### 8.1 Store

One markdown file per ticket under `/app-name-1/tickets/<column>/`.

```text
/app-name-1/tickets/backlog/
/app-name-1/tickets/open/
/app-name-1/tickets/in-progress/
/app-name-1/tickets/in-testing/
/app-name-1/tickets/blocked/
/app-name-1/tickets/done/
/app-name-1/tickets/closed/
/app-name-1/tickets/closed-wont-do/
```

Column = folder. `/app-name-1/` is repo-relative. Do not use GitHub Issues unless the user later says to. Comments live in the ticket file body.

### 8.2 Kind prefix and shared increment

Kind prefix says **what class of work** the ticket is, which also implies the default reader:

- `F` — feature / implementation work. Default reader: Worker-cold.
- `TA` — technical-architecture problem. Default reader: DesignArchitect-warm. Created only by KanbanMaster-cold.

KanbanMaster-cold has no kind prefix. That role *cuts* tickets; it is not a ticket inbox.

`F` and `TA` share one increment. No zero-padding (`F-1`, `F-10`, not `F-001`). Agents must sort by the integer, not lexicographically as strings (`F-9` before `F-10`).

More kind prefixes may be added later if a new inbox is actually needed. Do not invent them in advance.

Type abbreviations after the increment (`DB`, `UI`, …) are **layer/area tags**, not agent-class names. The list is per-project, written by DesignArchitect-warm into living-knowledge (§6.8). KanbanMaster-cold uses only that list when cutting tickets.

Examples:

- `F-7-DB` — database work for the worker
- `F-8-DM-BL` — data model + business layer
- `TA-13` — design problem for the Design architect

### 8.3 Filename

State is **not** in the filename. Title slug and optional cluster id **are**, so a folder listing plus `kanban.md` can draw a board without opening bodies.

Pattern:

```text
F-<n>[-C<cluster>]-<types>-<slug>.md
TA-<n>[-C<cluster>]-<slug>.md
```

Example: `/app-name-1/tickets/open/F-10-C3-DB-add-recipe-table.md`

Rules:

- Slug is a short ASCII hyphenated form of the human title. Full title still lives in the file body.
- Whole filename (including `.md`) ≤ **80 characters**, so Windows path limits stay safe under a normal repo path.
- No punctuation except hyphen. No spaces.
- Do **not** put friction or state in the filename (those change often; renaming would be noise).
- Cluster token `C<n>` is assigned by KanbanMaster-cold when related tickets should be done by the same Worker-cold instance. Omit `C<n>` if the ticket is a singleton.
- Moving column = move the file between folders and update the state field inside the file. Mismatch is a defect.

Who may move a ticket file between column folders:

- Worker-cold: `open` → `in-progress` → `in-testing` or `blocked` (only the ticket they are executing).
- Human: any move they need, especially `in-testing` → `done` (pass) or `in-testing` → `in-progress` (fail).
- KanbanMaster-cold: `backlog` → `open`, `done` → `closed`, `closed-wont-do`, and blocker/ceremony moves.

### 8.4 Board states

| Token / folder | Who may move *into* this folder | Cap |
|---|---|---|
| `backlog` | **Only** KanbanMaster-cold (from design spec, during ceremony) | any |
| `open` | **Only** KanbanMaster-cold via `backlog → open` | target ~10; hard stop 15 (§8.8) |
| `in-progress` | Worker-cold when they start the approved ticket | typically 1 |
| `in-testing` | Worker-cold after commit+push, before the human test | typically 1 |
| `blocked` | Worker-cold or KanbanMaster-cold | any |
| `done` | **Human** after test pass or gut-skip accept | any |
| `closed` | **Only** KanbanMaster-cold, `done → closed` | any |
| `closed-wont-do` | KanbanMaster-cold or human | any |

`done` is not terminal. Do not collapse `done` into `closed`.

Blocked tickets are first-class. The human’s standing preference: do **not** park a blocker and pick an unrelated `open` ticket. Spawn KanbanMaster-cold on the blocker first.

### 8.5 Human-readable title

Full sentence title lives in the file body. Filename slug is the short form (§8.3).

### 8.6 Discovery comments

Workers write: extra skills; extra living-knowledge files loaded; extra work discovered; stop-rule events (§5.7); each friction event (`friction += 1` plus a concise comment).

Comments addressed to the Kanban master must say so.

### 8.7 Cheap board index

Listing `/tickets/<column>/` shows code + cluster + types + slug. Friction, depends-on, and comments stay in the body. `kanban.md` is generated and is what a newly spawned worker shows first (§8.10).

### 8.8 Kanban ceremony (cannot be skipped)

A ceremony is a fresh KanbanMaster-cold instance. New `backlog` tickets come from reading the design spec. If `backlog` and `open` run dry, work stops until a ceremony runs.

`open` target is ~10 so ceremonies happen often enough. It is **guidance**, not a brick wall: pulling a coherent cluster that lands on 12–13 is allowed. Hard stop **15** so `open` cannot become a junk drawer. Do not stretch unrelated tickets into a fake cluster just to fill the column.

When moving `backlog → open`, prefer tickets that already share a dependency or the same module+layer, and stamp them with the same `C<n>`.

The Kanban master also reviews `in-progress` / `in-testing` / `blocked` / `done` / comments / friction; presents blocker options or escalates; writes TA tickets for design gaps (problem only, never “see F-33”); closes accepted `done` tickets; regenerates `kanban.md`; does not edit the design spec.

### 8.9 Friction counter

Integer field on the ticket, default 0, no cap. Not a 1–5 opinion score. Worker increments by 1 per fail event and leaves one concise comment. Shown on the `kanban.md` row. Not in the filename.

### 8.10 Kanban board file (generated)

Path: `/app-name-1/kanban.md`. Human does not hand-maintain it. Generated by the agent that last moved or edited a ticket. Worker-cold shows this board first on spawn.

Rows: ticket code, cluster id, slug/title, folder/state, friction, created-by.

### 8.11 Extra work mid-ticket (no worker-created tickets)

Workers do **not** create tickets. Comment on the current ticket → human spawns KanbanMaster-cold → that agent creates the new ticket. Do not shrink a design-level coupling problem into a cute ticket.

### 8.12 Ticket body fields

`Created by`, `DateTimeCreated`, `TicketType` (unabbreviated), `Cluster:` (`C<n>` or none), `friction` (int, default 0), `Depends on`, state matching the folder, full human title, comments log.

### 8.13 TA tickets

Prefix `TA`, same increment, same folders. Example: `/app-name-1/tickets/open/TA-13-name-the-billing-boundary.md`.

Only KanbanMaster-cold creates them. Content is the problem, impact, needs — self-contained. Forbidden: “see F-33” as the body. DesignArchitect-warm must not follow a non-TA dependency unless the TA is incomplete (then that is a writing defect). Design architect marks TA `done` with a concise solution comment; Kanban master closes it after any spec change is already written.

### 8.14 Ticket granularity (working rule)

Do not size tickets to “how many minutes an agent needs.” Size them to **one human-testable outcome** plus **one approach discussion**.

Working bounds:

- Too small: a ticket whose only work is mechanical and has no decision, if it would be cheaper as a step inside a cluster neighbour.
- Too big: a ticket the human cannot test as one idea, or that would force the worker to load half the app.
- Aim: one outcome a senior engineer could implement in a focused sitting and the human can accept or reject in one test pass.
- Split when the approach discussion forks, or when two test passes would be clearer than one.
- Cluster (`C<n>`) is how several small-but-related outcomes share one Worker-cold instance. Clustering is not an excuse to write a mega-ticket.

No Jira-style epic / user-story layer. Phase 1 already produces the design spec with component order. Traceability is: design-spec section → backlog tickets → cluster id. Adding epics would be a third board.
