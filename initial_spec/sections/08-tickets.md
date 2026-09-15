## 8. Tickets

### 8.1 Store

Tickets live in the git repo as markdown files, one file per ticket, in repo-relative column folders:

```text
/app-name-1/tickets/backlog/
/app-name-1/tickets/open/
/app-name-1/tickets/in-progress/
/app-name-1/tickets/blocked/
/app-name-1/tickets/done/
/app-name-1/tickets/closed/
/app-name-1/tickets/closed-wont-do/
```

Same meaning of `/app-name-1/` as in §6.2: repo-relative project directory, not a host-absolute path.

Do not use GitHub Issues or an external tracker unless the user later says to. Ticket comments are body text inside the ticket file (and therefore in git).

### 8.2 Shared increment, typed prefix

Ticket numbers use prefix codes. The increment variable is shared among all ticket types.

Examples:

- `F-07-DB` — a database ticket
- `F-08-DM-BL` — cross-cutting data model and business layer / services
- `F-09-CTL-UI-JS` — cross-cutting controllers, UI, and JavaScript
- `TA-13` — technical-architecture ticket for DesignArchitect-warm (§8.13)

`F` and `TA` share one increment. There is not a separate TA sequence.

### 8.3 Ticket files and column folders

Board column is **which folder the ticket file lives in**, not a suffix on the filename. Listing a folder is the cheap column view (no need to open file bodies to learn state).

Filename keeps identity only, e.g. `F-10-DB.md` or `TA-13.md`. **Do not also put state in the filename** (`F-10-DB-open.md` is retired). Folder + suffix would be two copies of the same fact.

Moving a ticket between columns = moving the file between folders, and updating the state field inside the file to match. Mismatch is a defect.

Filename parts, in order (no state suffix):

1. Kind prefix `F` or `TA`.
2. Shared increment, zero-padded in examples (`07`, `09`, `10`). Padding width is not locked; pick one width per project and keep it stable.
3. One or more type abbreviations for `F-` tickets, separated by `-`.
4. Suffix `.md`.

Example path: `/app-name-1/tickets/open/F-10-DB.md`.

### 8.4 Board states (kanban)

The column formerly called `todo` is named **`open`**.

| Token / folder | Column | Who may move *into* this folder | Cap |
|---|---|---|---|
| `backlog` | Backlog | **Only** KanbanMaster-cold creates here (from design spec, during ceremony) | any number |
| `open` | Open | **Only** KanbanMaster-cold via `backlog → open` | fixed; **10** |
| `in-progress` | In progress | Worker-cold when they start the human-approved ticket | 1 worker ⇒ typically 1 |
| `blocked` | Blocked | Worker-cold or KanbanMaster-cold | any |
| `done` | Done | Worker-cold or human after test step passes (or human gut-skips and accepts) | any |
| `closed` | Closed | **Only** KanbanMaster-cold, via `done → closed` | any |
| `closed-wont-do` | Closed (rejected) | KanbanMaster-cold or human | any |

`done` is not terminal. `closed` and `closed-wont-do` are terminal. Do not collapse `done` into `closed`.

### 8.5 Human-readable title

Every ticket also needs a human-friendly title / description of what the ticket is doing. That lives **inside** the file. The filename is the index; the title is not required in the filename.

### 8.6 Discovery comments on tickets

Workers write into the ticket file body:

- extra transient skills added for the ticket;
- extra living-knowledge files loaded;
- extra work discovered mid-ticket;
- stop-rule events (§5.7), written so a later KanbanMaster-cold can act without the worker’s session;
- each friction event: increment the ticket `friction` field by 1 and leave a concise comment (§8.9).

Comments the worker addresses to the Kanban master must say so in the comment.

### 8.7 Why column folders exist (cost)

Opening every ticket body to build a board would grow context with ticket history the agent does not need. Listing `/tickets/<column>/` is the cheap board index. Titles, friction, and depends-on still live in the ticket file; `kanban.md` is generated from folders + those fields (§8.10).
