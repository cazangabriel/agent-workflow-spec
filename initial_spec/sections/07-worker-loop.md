## 7. Mandatory worker initialisation and ticket loop

This sequence is mandatory.

**Step 1 — Init.**
Human points a non-initialised agent at `Worker-cold.md`. Load software engineering, this app’s domain, project standing skills — not every layer. Load §7.1. Load git / containers / deploy only if the ticket needs those artifacts (§4.6).

**Step 2 — Show the board, then suggest.**
First visible act: show `/app-name-1/kanban.md`. Suggest next work from the board and filenames (module + number + layers + slug). Do not open every body just to pick. Mention `blocked` if any exist.

**Step 3 — Human picks.**
The human drives which ticket to work. Several tickets may already be `in-progress` or `in-testing`. Same Worker-cold instance may continue on the next ticket the human picks if it is the same module or a declared dependency (§5.11).

**Step 4 — Approach discussion.**
Worker proposes approaches. Human chooses or alters.

A worker ticket that looks too big for §8.14 is a **split request**, not a 15–20% weekly-budget estimate. The 15–20% band belongs to DesignArchitect-warm and KanbanMaster-cold talking about design/architecture scope (§5.8). If a worker ticket would take on the order of a million tokens, the ticket is already wrong.

**Step 5 — Implement** after agreement.

**Step 6 — Commit and push to `dev`.**
No feature branch. Commit line: `Worker-cold AM-10-F-DB add recipe table`.

The human may start another ticket while one sits in `in-testing`. No wait-for-testing constraint.

**Step 7 — Move to `in-testing`.**
Worker moves that file, regenerates `kanban.md`, asks the human to pull `dev` and test (or gut-skip).

**Step 8 — Human moves it on.**
Pass / gut-skip: human moves `in-testing` → `done`.
Fail: human moves it back; worker increments friction and continues.
Worker does not mark `done` or `closed`.

Stop-rule (§5.7): comment, tell the human, do not invent the missing rule.

### 7.1 Worker construction skills

OOP, OOD, SOLID, temporal coupling, pitfalls / anti-patterns, readable names.
