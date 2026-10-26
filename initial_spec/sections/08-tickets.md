## 8. Tickets

### 8.1 Store

One markdown file per ticket under `/app-name-1/tickets/<column>/`. Column = folder. `/app-name-1/` is repo-relative. Comments live in the ticket file body.

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

Violation: ticket state stored only inside the file, with every ticket in one folder.
Example: AM-10 in `open` is the file `/app-name-1/tickets/open/AM-10-F-DB-add-recipe-table.md`. It is not a row in a single `tickets.md`.

### 8.2 Kind, increment, module, layers

Kind = class of work, which implies the default reader.

- `F` — feature / implementation. Default reader: Worker-cold. Unabbreviated: `Feature`.
- `TA` — technical-architecture problem. Default reader: DesignArchitect-warm. Created only by KanbanMaster-cold. Unabbreviated: `TechnicalArchitecture`.

KanbanMaster-cold has no kind prefix. It cuts tickets; it is not an inbox.

`F` and `TA` share one increment across the whole repo. No zero-padding. The next number lives in `/app-name-1/tickets/NEXT` (§8.15). Do not scan filenames to find it.

Module abbreviations and layer abbreviations are two separate per-project lists in living-knowledge, both written by DesignArchitect-warm in Phase 1 (§6.8). Each row has `Token`, `Unabbreviated`, and `Meaning`.

- Module = product area in this repo (e.g. `AM` = `AmazonManager`). Not a second git repo.
- Layer = technical slice (`DM` = `DataModel`, `DB` = `Database`, `UI` = `UserInterface`).
- Cross-module work uses a reserved module abbrev that DesignArchitect-warm defines (suggested default: `XCUT` = `CrossCut`), not a silent second name.

KanbanMaster-cold may only use abbreviations from those lists. It builds `Unabbreviated ID` by lookup. It does not invent a word.
Violation: a filename `AM-010-F-DB-....md` (zero-padded), or a module token not in `abbreviations.md`.
Example: `NEXT` contains `14`. The next file is `AM-14-F-DB-add-recipe-table.md`, then `NEXT` becomes `15` in the same commit.

### 8.3 Filename

State is the folder, not the filename. There is no cluster token in the filename.

Pattern: `<module>-<n>-<kind>-<layers>-<slug>.md`

```text
/app-name-1/tickets/open/AM-10-F-DB-add-recipe-table.md
/app-name-1/tickets/open/CHANADV-11-F-API-sync-listing.md
/app-name-1/tickets/open/XCUT-13-TA-name-billing-boundary.md
```

Module comes first so a folder listing groups the same area together.

Rules:

- Slug is a short ASCII hyphenated form of the human title. It must be readable on its own (`add-recipe-table`, not `fix`, `update`, `misc`, `wip`). A lazy slug is a KanbanMaster-cold writing defect.
- Full title still lives in the file body and is authoritative.
- Whole filename including `.md` ≤ 80 characters.
- No punctuation except hyphen. No spaces.
- Do not put friction or state in the filename.
- Moving column = move the file and update the state field inside it. A mismatch is a defect.

Who may move a ticket file:

- Worker-cold: on tickets the human has assigned — `open` → `in-progress` → `in-testing` or `blocked`.
- Human: any move, especially `in-testing` → `done` (pass) or `in-testing` → `in-progress` (fail).
- KanbanMaster-cold: `backlog` → `open`, `done` → `closed`, `closed-wont-do`, ceremony and blocker moves.

Violation: a filename `AM-10-F-DB-C3-add-recipe-table.md` (cluster token), or `AM-10-F-DB-open.md` (state in the name), or a slug `fix`.
Example: the file is `AM-10-F-DB-add-recipe-table.md` in the `open` folder. Moving it to `in-progress` changes the folder and the `State:` field. The filename stays the same.

### 8.4 Board states

| Token / folder | Who may move into this folder | Cap |
|---|---|---|
| `backlog` | Only KanbanMaster-cold | any |
| `open` | Only KanbanMaster-cold via `backlog` → `open` | target ~10; hard stop 15 |
| `in-progress` | Worker-cold when the human starts that ticket | any (human-driven) |
| `in-testing` | Worker-cold after commit+push | any (human-driven) |
| `blocked` | Worker-cold or KanbanMaster-cold | any |
| `done` | Human after test pass or gut-skip accept | any |
| `closed` | Only KanbanMaster-cold | any |
| `closed-wont-do` | KanbanMaster-cold or human | any |

No rule that only one ticket may be `in-progress` or `in-testing`. No rule that the worker must wait for `in-testing` to clear before pushing another ticket. The human picks the next ticket.
Violation: the worker moving a file to `done`, or KanbanMaster pulling 16 into `open`.
Example: AM-10 can sit in `in-testing` while the human starts AM-11. That is allowed. The worker does not move AM-10 to `done`.

### 8.8 Kanban ceremony

Fresh KanbanMaster-cold. New `backlog` tickets come from the design spec.

`open` target ~10, hard stop 15. Prefer pulling tickets that share a module or a written `Depends on`, when that is straightforward. Do not invent a named cluster id. Do not stretch unrelated tickets into a fake group.

Also: review other columns, blockers, friction; write self-contained TA tickets (never "see AM-33"); close accepted `done`; regenerate `kanban.md`; do not edit the design spec.
Violation: a pull that puts `open` at 16, a cluster id, or a TA body that says only "see AM-33".
Example: pull AM-8 and AM-9 because AM-9 `Depends on` AM-8. Do not invent `cluster: C3`.

### 8.9 Friction counter

Integer field on the ticket body, named `friction`. Starts at 0. No upper limit. Not a difficulty score. Not a 1–5 opinion. Shown on the `kanban.md` row. Not in the filename. A high number means the ticket has bounced: repeated test fails, stop-rule hits, or human send-backs.

Who sets it. KanbanMaster-cold writes `friction: 0` when it cuts the ticket. Worker-cold is the only role that increments it. Worker-cold adds 1 only for one of these three events. This is the closed set. Nothing else counts.

1. Test fail. The human tested the ticket and said it failed, or moved it back from `in-testing`.
2. Stop-rule hit. The worker stopped because it was unsure, living-knowledge did not cover the fact, or the work looked like a design change.
3. Human send-back. The human rejected the work and sent it back, other than a test fail already counted in 1.

Each event is +1, once, in the same edit as one comment that names the event and why. A ticket that fails testing three times and is sent back twice can reach 5. That is correct.

Worker-cold does not invent a number. It does not write `friction: 3` because the ticket felt hard. It does not write `friction: 1` because it expects the ticket to be hard.
Violation: `friction: 3` with no fail event, or a +1 with no comment.
Example: the human says the test failed. The worker sets `friction: 1` and comments "friction +1 — test failed: migration does not create index."

### 8.10 Kanban board file (generated)

Path: `/app-name-1/kanban.md`. The human does not hand-edit it. Generated by the agent that last moved or edited a ticket. Worker-cold shows this board first on spawn.

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
- Empty columns may be omitted or shown as "(none)".
- Ticket column is the code without the slug (`AM-10-F-DB`), so the row stays short.
- Title is the full sentence from the ticket body, not the filename slug.
- Header records who generated it and the commit, so the human can see whether the pulled file matches the board.

Violation: a hand-edited `kanban.md` that disagrees with the folders, or a row that uses the slug instead of the body title.
Example: the row title is "Add the recipe table and migration." not "add-recipe-table".

### 8.11 No worker-created tickets

The worker comments on the current ticket. The human spawns KanbanMaster-cold. That agent cuts the new ticket.
Violation: the worker creating `AM-15-F-DB-....md`.
Example: the worker finds a missing index. It comments on AM-10. It does not create a file.

### 8.12 Ticket body template (locked)

```markdown
# AM-7-F-DM add recipe table

- Created by: KanbanMaster-cold
- DateTimeCreated: 2026-10-19T14:00:00Z
- Module: AM
- TicketType: DataModel
- Unabbreviated ID: AmazonManager-7-Feature-DataModel
- friction: 0
- Depends on: AM-6-F-DM
- State: open

## Title

Add the recipe table and migration.

## Body

One human-testable outcome. Enough context that a cold Worker-cold can propose an approach without guessing. Point at the living-knowledge files that apply. Do not paste the design solution if this is a TA ticket.

## Comments

- 2026-10-19T15:10:00Z Worker-cold: friction +1 — test failed: migration does not create index. Addressed to KanbanMaster-cold.
```

`Unabbreviated ID` is the ticket code with each token expanded. Pattern: `<module unabbreviated>-<n>-<kind unabbreviated>-<layer unabbreviated>`.

Source of each word:

- Module and layer expansions come only from `/app-name-1/living-knowledge/abbreviations.md`, column `Unabbreviated` (§6.8). KanbanMaster-cold reads that file. It does not invent or approximate the word.
- Kind expansions are fixed. `F` is `Feature`. `TA` is `TechnicalArchitecture`. These are not in `abbreviations.md`.
- The number is copied. It is not expanded.

Example: `abbreviations.md` has `AM` = `AmazonManager` and `DM` = `DataModel`. The code `AM-7-F-DM` becomes `AmazonManager-7-Feature-DataModel`. It does not become `Amazon manager-7-Feature-data model`.

If a token has no `Unabbreviated` cell, KanbanMaster-cold stops and reports the missing cell. It does not guess a word.
Violation: an `Unabbreviated ID` that is not a lookup, or a ticket body with no `Unabbreviated ID`.

Other rules: filename is the identity. The H1 is the human title. `TicketType` is the unabbreviated layer name, the same cell used in `Unabbreviated ID`. `State` must match the folder. Comments are append-only, timestamped, and say who wrote them. A comment addressed to the Kanban master must say so. TA tickets use the same shape. The Body is the problem, impact, and needs — no recommended solution.
Violation: a TA body that says "see AM-33", or a `State: open` on a file in the `in-progress` folder.

### 8.13 TA tickets

Same filename pattern. Example: `XCUT-13-TA-name-billing-boundary.md`. Self-contained problem, impact, needs. No recommended design solution. No "see AM-10" as the body. `Unabbreviated ID` uses `TechnicalArchitecture` for `TA`.
Violation: a TA body that contains the solution.
Example: "living-knowledge does not state whether `cook_time` is minutes or seconds. Impact: the migration cannot be written. Need: a unit." Not "use minutes."

### 8.14 Ticket granularity

One human-testable outcome plus one approach discussion. Too small = no decision. Too big = cannot test as one idea, or would load half the app. Split when the approach forks. Reuse the same Worker-cold on the next ticket when it is the same module or a declared dependency — that is a spawn choice, not a filename token.
No epic or user-story layer. Traceability: design-spec section → ticket (module + number).
Violation: a ticket whose approach discussion has two independent outcomes.
Example: "add the table and also restyle the login page" is two tickets. Split it.

### 8.15 Next ticket number

Path: `/app-name-1/tickets/NEXT`. Contents are one integer — the next number to use. No markdown, no header.

```text
14
```

Rules: only KanbanMaster-cold reads and increments it. When it cuts a ticket it uses the current value, then writes `value + 1` in the same commit as the new ticket file. Workers and DesignArchitect-warm never touch it. If the file is missing, that is a defect. Do not fall back to scanning filenames.
Violation: scanning ticket filenames to compute the next number, or a worker writing `NEXT`.
Example: `NEXT` is `14`. KanbanMaster cuts `AM-14-...`, writes `15` into `NEXT`, and commits both in the same commit.
