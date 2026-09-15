## 7. Mandatory worker initialisation and ticket loop

This sequence is mandatory.

**Step 1 — Init.**
Human points a non-initialised agent at `Worker-cold.md`. Load software engineering, this app’s domain, project-specific standing skills — not every layer. Also load §7.1. Load git / containers / deploy only if the ticket needs those artifacts (§4.6).

**Step 2 — Show the board, then propose clusters.**
First visible act: show `/app-name-1/kanban.md`. Then suggest which `open` cluster (`C<n>`) or singleton to take next. Do this from the generated board and filenames (code + cluster + slug). Do not open every ticket body just to pick. Mention `blocked` if any exist — blocked work is higher priority than a new cluster (§8.4).

**Step 3 — Human picks.**
Human picks one ticket, or confirms the suggested cluster. Same Worker-cold instance may then walk the rest of that cluster (§5.11) after each ticket’s test step, unless the human says to stop.

**Step 4 — Approach discussion (mandatory).**
Worker proposes one or more approaches. Prefer simplest approaches that still produce maintainable professional code (§4.4–§4.5). Human chooses or alters.

If the worker believes the ticket would burn on the order of 15–20% of the weekly planning budget (§5.8), they say so **here**, with the estimate and the uncertainty, and wait. They do not start the expensive work.

**Step 5 — Implement.**
Only after agreement.

**Step 6 — Commit and push to `dev`.**
Direct commits on `dev`. No feature branch. Commit line: `Worker-cold F-10-DB add recipe table`.

Do not start the next ticket’s push until the current ticket has left `in-testing` (accepted or sent back), unless the human explicitly says “keep going on this cluster; I will test the bundle.”

**Step 7 — Move to `in-testing`.**
Worker moves the file `in-progress` → `in-testing`, regenerates `kanban.md`, and asks the human to pull `dev` and test (or gut-skip). Agent-tester is still rejected.

**Step 8 — Human moves it on.**
Pass or gut-skip accept: human moves `in-testing` → `done`.
Fail: human moves `in-testing` → `in-progress` (or `blocked`); worker increments friction and continues.
Worker does **not** mark `done`. Worker never marks `closed`.

If during Steps 4–8 the worker hits the §5.7 stop-rule, they comment, tell the human, and do not invent the missing rule.

Same Worker-cold instance may continue onto the next ticket in the same `C<n>` cluster.

### 7.1 Worker construction skills (decided)

OOP, OOD, SOLID, temporal coupling, common pitfalls and anti-patterns, readable names. Standing skills, not optional style.
