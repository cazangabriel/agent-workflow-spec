## 7. Mandatory worker initialisation and ticket loop

This sequence is mandatory. A step does not start until the previous step has finished. "First", "wait", and "propose" mean the step number, not a mood.

**Step 1 — Init.** Before any other act.
The human points a non-initialised agent at `Worker-cold.md`. The agent loads software engineering, this app's domain, and the project standing skills. It does not load every layer. It loads §7.1. It loads git, containers, or deploy skills only if the ticket needs those artifacts (§4.6).
Violation: starting the ticket without having read `Worker-cold.md`.
Example: the human says "read `.grok/agents/worker-cold.md` and obey it." The agent reads that file before it shows the board.

**Step 2 — Show the board, then suggest.** After Step 1, before a ticket body is opened.
First visible act: show `/app-name-1/kanban.md`. Suggest next work from the board and filenames (module + number + layers + slug). Do not open every body just to pick. Mention `blocked` if that column is not empty.
Violation: opening a ticket body before the board is shown.
Example: the agent pastes the `kanban.md` table, notes that `blocked` has AM-7, and suggests AM-10 from the filename. It does not open AM-10's body in this step.

**Step 3 — Human picks.** After Step 2.
The human drives which ticket to work. Several tickets may already be `in-progress` or `in-testing`. The same Worker-cold instance may continue on the next ticket the human picks if it is the same module or a declared dependency (§5.11).
Violation: the agent picking the ticket itself and starting work.
Example: the human says "AM-10". The agent does not start AM-11 because it looked related.

**Step 4 — Approach discussion.** After the human names a ticket, before any code is written.
The worker proposes one approach. The human chooses or alters. A worker ticket that looks too big for §8.14 is a split request, not a 15–20% weekly-budget estimate. The 15–20% band belongs to DesignArchitect-warm and KanbanMaster-cold talking about design scope (§5.8). If a worker ticket would take on the order of a million tokens, the ticket is already wrong.
Violation: writing code in this step, or quoting a 15–20% figure.
Example: "Add column `cook_time integer not null default 0` in one migration." Then stop. Do not write the migration file.

**Step 5 — Implement.** Only after the human agrees to an approach, or alters one and the worker restates the altered version and the human agrees.
Implement only that agreed approach. Stop-rule (§5.7): if unsure, if living-knowledge does not cover it, or if it looks like a design change, comment on the ticket, tell the human, and do not invent the missing rule. Do not create a ticket.
Violation: inventing the missing rule, or creating a ticket.
Example: living-knowledge does not state the unit of `cook_time`. The worker comments "unit not stated. I am not inventing minutes." and stops.

**Step 6 — Commit and push to `dev`.** After Step 5, before the ticket is moved.
No feature branch. Commit line: `Worker-cold AM-10-F-DB add recipe table`.
Violation: a feature branch, or a commit line that only names an internal step (`wip`, `step 6 done`).

The human may start another ticket while one sits in `in-testing`. There is no wait-for-testing constraint.

**Step 7 — Move to `in-testing`.** After the push in Step 6.
The worker moves that file, regenerates `kanban.md`, and asks the human to pull `dev` and test or gut-skip. The same message names the absolute path, the commit, and that the human should see it after `git pull`.
Violation: saying "pushed" without the path and the commit.
Example: "Moved `/app-name-1/tickets/in-testing/AM-10-F-DB-add-recipe-table.md`. Commit `abc123` on `dev`. You should see it after `git pull`."

**Step 8 — Human signals, worker moves.** After Step 7.
Pass or gut-skip: the human signals the ticket is done. The signal is the human saying the test passed, gut-skipping, or saying to consider the ticket done. Silence is not a signal. Worker-cold then moves `in-testing` → `done`. Worker-cold does not move to `done` before that signal. Worker-cold does not move to `closed`.
Fail: the human says it failed, or moves it back. The worker increments `friction` by 1, writes one comment, and returns to Step 4 or Step 5 as the human says.
Violation: the worker moving the file to `done` before the signal, or the worker moving it to `closed`.
Example: the human says "AM-10 is done." The worker moves the file to `done`. The worker does not move it because the human went quiet.

### 7.1 Worker construction skills

The worker is required to apply these when writing code: OOP, OOD, SOLID, temporal coupling, known pitfalls and anti-patterns, readable names.
Violation: code that the worker cannot explain against one of these.
Example: a method that both reads the database and renders HTML is a SOLID violation. The worker does not ship it and call it fine.
