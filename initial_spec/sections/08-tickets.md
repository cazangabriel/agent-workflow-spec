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

Column = folder. `/app-name-1/` is repo-relative. Comments live in the ticket file body.

### 8.2 Kind, increment, module, layers

Kind = class of work, which implies the default reader:

- `F` — feature / implementation. Default reader: Worker-cold.
- `TA` — technical-architecture problem. Default reader: DesignArchitect-warm. Created only by KanbanMaster-cold.

KanbanMaster-cold has no kind prefix. It cuts tickets; it is not an inbox.

`F` and `TA` share one increment across the whole repo. No zero-padding.

**Module abbreviations** and **layer/type abbreviations** are two separate per-project lists in living-knowledge, both written by DesignArchitect-warm in Phase 1 (§6.8).

- Module = product area in *this* repo (e.g. `AM`, `CHANADV`, `RECIPES`). Not a second git repo.
- Layer/type = technical slice (`DB`, `UI`, `API`, …).
- Cross-module work uses a reserved module abbrev that the Design architect defines (suggested default: `XCUT`), not a silent second name.

KanbanMaster-cold may only use abbreviations from those lists.

### 8.3 Filename

State is the folder, not the filename. There is **no cluster token** in the filename.

Pattern:

```text
<module>-<n>-<kind>-<layers>-<slug>.md
```

Examples:

```text
/app-name-1/tickets/open/AM-10-F-DB-add-recipe-table.md
/app-name-1/tickets/open/CHANADV-11-F-API-sync-listing.md
/app-name-1/tickets/open/XCUT-13-TA-name-billing-boundary.md
```

Module comes first so a folder listing groups the same area together (the job named clusters were trying to do).

Rules:

- Slug is a short ASCII hyphenated form of the human title. It must be readable on its own (`add-recipe-table`, not `fix`, `update`, `misc`, `wip`). Lazy slugs are a KanbanMaster-cold writing defect.
- Full title still lives in the file body and is authoritative.
- Whole filename including `.md` ≤ **80 characters**.
- No punctuation except hyphen. No spaces.
- Do not put friction or state in the filename.
- Moving column = move the file and update the state field inside it. Mismatch is a defect.

Who may move a ticket file:

- Worker-cold: on tickets the human has assigned — `open` → `in-progress` → `in-testing` or `blocked`. Several tickets may sit in `in-progress` or `in-testing` at once; the human drives that.
- Human: any move, especially `in-testing` → `done` (pass) or `in-testing` → `in-progress` (fail).
- KanbanMaster-cold: `backlog` → `open`, `done` → `closed`, `closed-wont-do`, ceremony/blocker moves.

### 8.4 Board states

| Token / folder | Who may move *into* this folder | Cap |
|---|---|---|
| `backlog` | **Only** KanbanMaster-cold | any |
| `open` | **Only** KanbanMaster-cold via `backlog → open` | target ~10; hard stop 15 |
| `in-progress` | Worker-cold when the human starts that ticket | any (human-driven) |
| `in-testing` | Worker-cold after commit+push | any (human-driven) |
| `blocked` | Worker-cold or KanbanMaster-cold | any |
| `done` | **Human** after test pass or gut-skip accept | any |
| `closed` | **Only** KanbanMaster-cold | any |
| `closed-wont-do` | KanbanMaster-cold or human | any |

No rule that only one ticket may be `in-progress` or `in-testing`. No rule that the worker must wait for `in-testing` to clear before pushing another ticket. The human picks the next ticket.

### 8.5 Title and slug quality

Body title = full sentence. Filename slug = short form that still names the outcome. KanbanMaster-cold must not write lazy slugs or lazy body titles.

### 8.6 Discovery comments

Workers write extra skills, extra living-knowledge loaded, extra work, stop-rule events, friction events (`friction += 1` + comment). Comments to the Kanban master must say so.

### 8.7 Cheap board index

A folder listing already shows module + number + kind + layers + slug. That is enough to pick work without opening every body. Friction, depends-on, and comments stay in the body. `kanban.md` is generated (§8.10).

### 8.8 Kanban ceremony

Fresh KanbanMaster-cold. New `backlog` tickets come from the design spec.

`open` target ~10, hard stop 15. Prefer pulling tickets that share a module or a written `Depends on`, when that is straightforward. Do **not** invent a named cluster id. Do not stretch unrelated tickets into a fake group.

Also: review other columns, blockers, friction; write self-contained TA tickets (never “see AM-33”); close accepted `done`; regenerate `kanban.md`; do not edit the design spec.

### 8.9 Friction counter

Integer on the ticket, default 0, no cap. +1 per fail + one comment. Shown on `kanban.md`, not in the filename.

### 8.10 Kanban board file (generated)

`/app-name-1/kanban.md`. Rows: module-number-kind, layers, slug/title, folder, friction, created-by.

### 8.11 No worker-created tickets

Comment on the current ticket → human spawns KanbanMaster-cold → that agent cuts the new ticket.

### 8.12 Ticket body fields

`Created by`, `DateTimeCreated`, `Module`, `TicketType` (unabbreviated layers), `friction`, `Depends on`, state matching the folder, full human title, comments log. No `Cluster` field.

### 8.13 TA tickets

Same filename pattern. Example: `XCUT-13-TA-name-billing-boundary.md`. Self-contained problem, impact, needs. No recommended design solution. No “see AM-10” as the body.

### 8.14 Ticket granularity

One human-testable outcome + one approach discussion. Too small = no decision. Too big = cannot test as one idea, or would load half the app. Split when the approach forks. Reuse the same Worker-cold on the next ticket when it is the same module or a declared dependency — that is a spawn choice, not a filename token.

No epic / user-story layer. Traceability: design-spec section → ticket (module + number).
