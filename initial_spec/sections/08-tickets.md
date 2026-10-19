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

`F` and `TA` share one increment across the whole repo. No zero-padding. The next number lives in `/app-name-1/tickets/NEXT` (§8.15). Do not scan filenames to find it.

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

Module comes first so a folder listing groups the same area together.

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

Integer on the ticket, default 0, no cap. Not a 1–5 opinion score. Worker increments by 1 each time something fails (test fail, stop-rule hit, human send-back) and leaves one concise comment. Shown on the `kanban.md` row, not in the filename. A high number means the ticket has bounced a lot; it is a signal for the human and the Kanban master, not a difficulty guess. Plain-language definition lives in the glossary (§14.8).

### 8.10 Kanban board file (generated)

Path: `/app-name-1/kanban.md`. Human does not hand-edit it. Generated by the agent that last moved or edited a ticket. Worker-cold shows this board first on spawn.

Layout (locked):

```markdown
# Kanban

Generated from ticket files. Do not hand-edit.
Last generated: 2026-10-19T15:12:00Z by KanbanMaster-cold
Commit: abc1234

## open

| Ticket | Layers | Title | Friction | Created by |
|---|---|---|---|---|
| AM-10-F-DB | DB | Add the recipe table and migration. | 0 | KanbanMaster-cold |

## in-progress

| Ticket | Layers | Title | Friction | Created by |
|---|---|---|---|---|
| AM-9-F-UI | UI | Recipe form validation. | 0 | KanbanMaster-cold |

## in-testing

## blocked

## done

## backlog

## closed

## closed-wont-do
```

Rules:

- One section per column, in this order: open, in-progress, in-testing, blocked, done, backlog, closed, closed-wont-do.
- Empty columns may be omitted or shown as “(none)”.
- Ticket column is the code without the slug (`AM-10-F-DB`), so the row stays short.
- Title is the full sentence from the ticket body, not the filename slug.
- Header records who generated it and the commit, so the human can see whether the pulled file matches the board.

### 8.11 No worker-created tickets

Comment on the current ticket → human spawns KanbanMaster-cold → that agent cuts the new ticket.

### 8.12 Ticket body template (locked)

```markdown
# AM-10-F-DB add recipe table

- Created by: KanbanMaster-cold
- DateTimeCreated: 2026-10-19T14:00:00Z
- Module: AM
- TicketType: Database
- friction: 0
- Depends on: AM-7-F-DM
- State: open

## Title

Add the recipe table and migration.

## Body

One human-testable outcome. Enough context that a cold Worker-cold can propose an approach without guessing. Point at the living-knowledge files that apply. Do not paste the design solution if this is a TA ticket.

## Comments

- 2026-10-19T15:10:00Z Worker-cold: friction +1 — test failed: migration does not create index. Addressed to KanbanMaster-cold.
```

Rules:

- Filename is the identity. The H1 is the human title.
- `TicketType` is the unabbreviated layer name(s), matching the abbreviation list.
- `State` must match the folder the file is in.
- Comments are append-only, timestamped, and say who wrote them. A comment addressed to the Kanban master must say so.
- TA tickets use the same shape. The Body is the problem, impact, and needs — no recommended solution.

### 8.13 TA tickets

Same filename pattern. Example: `XCUT-13-TA-name-billing-boundary.md`. Self-contained problem, impact, needs. No recommended design solution. No “see AM-10” as the body.

### 8.14 Ticket granularity

One human-testable outcome + one approach discussion. Too small = no decision. Too big = cannot test as one idea, or would load half the app. Split when the approach forks. Reuse the same Worker-cold on the next ticket when it is the same module or a declared dependency — that is a spawn choice, not a filename token.

No epic / user-story layer. Traceability: design-spec section → ticket (module + number).

### 8.15 Next ticket number

Path: `/app-name-1/tickets/NEXT`

Contents are one integer — the next number to use. No markdown, no header.

```text
14
```

Rules:

- Only KanbanMaster-cold reads and increments it.
- When it cuts a ticket it uses the current value, then writes `value + 1` in the same commit as the new ticket file.
- It is committed and pushed with the ticket, same as any other board change.
- Workers and DesignArchitect-warm never touch it.
- If the file is missing, that is a defect. Do not fall back to scanning filenames.
