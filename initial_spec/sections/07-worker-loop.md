## 7. Mandatory worker initialisation and ticket loop

This sequence is mandatory. It must appear as a list of steps every worker always completes.

**Step 1 — Init.**
The human initialises a new worker by pointing a non-initialised agent at `Worker-cold.md`. That file instructs the worker to acquire generic skills needed:

- software engineering expertise, plus
- general expertise in the business domain of that project, plus
- anything technical or topical that is specific for that project,

but **not** yet all expertise in all project-layer areas (not yet UX, DB, SQL, etc. as a full set).

Also load §7.1 (OOP/OOD/SOLID, pitfalls, readable names). If the ticket needs git, containers, or deploy, load those skills before touching those artifacts (§4.6).

**Step 2 — Show the board, then propose.**
The worker’s first visible act is to show the human the current `/app-name-1/kanban.md` board (generated from ticket files). Then the worker may suggest 4–5 next tasks from `open` (and mention `blocked` if relevant). KanbanMaster-cold built that list in the last ceremony.

**Step 3 — Human picks.**
The human picks 1 ticket from those 4–5 proposals.

**Step 4 — Approach discussion (mandatory).**
Worker proposes one or more approaches. Prefer simplest approaches first that still produce maintainable professional code (see §4.4–§4.5). Human chooses an approach as-is or alters it.

**Step 5 — Implement.**
Only after agreement does the worker implement as discussed.

**Step 6 — Commit and push.**
Worker commits and pushes to `dev`, either as direct commit(s) or via a feature branch when appropriate, then merge into `dev`. Commit line: `Worker-cold F-10-DB add recipe table`.

**Step 7 — Human test (required step; human may skip the actual exercise).**
Human pulls latest `dev`. The workflow asks the human to test the ticket in the app (start containers etc. as needed) and confirm to the worker whether it works. The human may make a gut decision not to run the test that time. That does not remove the step from the workflow and does not authorise an agent-tester substitute.

**Step 8 — Mark done or continue. Do not mark closed.**
If the human confirms it works as expected, the worker (or the human) moves the ticket file into `tickets/done/` and updates the state field inside the file. **Closed is a KanbanMaster-cold ceremony** (§8.8), not a worker action. If it does not work, work continues until it does.

If during Steps 4–8 the worker hits the §5.7 stop-rule, they comment the ticket, tell the human, and do not invent the missing rule.

Related checklist item for every worker session: do not forget commit + push (see §4.3).

Same Worker-cold instance may continue onto a related next ticket (§5.11).

### 7.1 Worker construction skills (decided)

When writing application code the worker must know and use:

- OOP and OOD
- SOLID
- Temporal coupling (and that it is a coupling)
- Common software-development pitfalls and anti-patterns — avoid them
- Readability: method and variable names that a human can read; no throwaway names like `x` for a domain idea

These are standing worker skills, not optional style.
