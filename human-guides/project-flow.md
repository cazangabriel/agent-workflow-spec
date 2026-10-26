# Project flow — which agent, when

This file is for you. Agents do not treat it as living-knowledge. The per-role files in this folder say how to start each agent. This file says the order and the frequency.

## The four roles

- **DesignArchitect-warm** owns the product design. Warm: resume the same session in Phase 1.
- **KanbanMaster-cold** runs the board. Cold: a new session for each ceremony.
- **Worker-cold** implements one agreed ticket. Cold by default. Reuse the same session only for a related next ticket.
- **WorkflowSteward-warm** owns changes to this workflow. It runs in the `agent-workflow-spec` repo, not in the product repo. You start it only when a workflow rule needs to change.

Product roles run in the product repo, on the shared checkout, with `grok --no-memory`. Do not use a worktree. Do not share one terminal between roles. One Worker-cold at a time.

## Start a product repo — before Phase 1

Do this once, before you start any product agent. `app-name-1` in the spec is a placeholder. Use your real app name for the folder. Tell DesignArchitect-warm that name.

**1. Create the repo.**
New private GitHub repo. Clone it. Create a `dev` branch and check it out. All agent work happens on `dev`. You release to `main` yourself. No feature branches.

**2. Copy these files from `agent-workflow-spec`.**

From `templates/product/`:

- `AGENTS.md` → the product repo root as `AGENTS.md`

From `.grok/agents/`:

- `design-architect-warm.md`
- `kanban-master-cold.md`
- `worker-cold.md`

Put those three at `.grok/agents/` in the product repo.

From `human-guides/`:

- every file in that folder, into `<app-name>/human-guides/`

**3. Do not copy these.**

- `.grok/agents/workflow-steward-warm.md` — stays in the workflow-spec repo
- `initial_spec/` — the workflow spec, not the product
- `for-human-only/` — prompts for you, not for product agents
- the root `AGENTS.md` of the workflow-spec repo — that one is for the steward

**4. Create the empty product structure.**

```text
<app-name>/living-knowledge/
<app-name>/tickets/NEXT          # file contains the single integer 1
<app-name>/tickets/backlog/
<app-name>/tickets/open/
<app-name>/tickets/in-progress/
<app-name>/tickets/in-testing/
<app-name>/tickets/blocked/
<app-name>/tickets/done/
<app-name>/tickets/closed/
<app-name>/tickets/closed-wont-do/
```

Do not hand-write `kanban.md`. The first KanbanMaster-cold ceremony generates it. Do not invent tickets. Phase 1 has not been accepted.

**5. Commit and push to `dev`.**
Then start Phase 1 below. Add the product repo to the Grok GitHub permissions list if Grok cannot see it.

## Order of a project

**1. Phase 1 — design, before any feature ticket.**
Start DesignArchitect-warm. Work with it until the design spec has: bounded contexts and ubiquitous language, 3–4 architectural characteristics, the module and layer map, the stack, the component order, the unknowns listed, living-knowledge skeletons, and both abbreviation lists. You explicitly accept Phase 1. Do not start KanbanMaster-cold or Worker-cold for product work before that accept.

**2. First ceremony — cut the first tickets.**
Start a new KanbanMaster-cold session. It reads the accepted design spec, cuts backlog tickets, and proposes a pull into `open` (target about 10, hard stop 15). You agree before it writes. It regenerates `kanban.md`.

**3. Phase 2 — the delivery loop.**
Start Worker-cold. It shows `kanban.md`. You pick a ticket. It proposes an approach. You agree. It implements, commits to `dev`, and moves the ticket to `in-testing`. You pull, test or gut-skip, and move it to `done` on pass.

Repeat Worker-cold on the next ticket. Reuse the same Worker session only if the next ticket is the same module, or depends on the one just finished. Start a new Worker session when the next ticket is a different module with no dependency.

**4. Ceremony again — when the board needs it.**
Start a new KanbanMaster-cold (do not resume the old ceremony) when:
- `open` is thin and you want more tickets from backlog,
- `done` tickets are waiting to be closed,
- a worker is blocked and you want the board role to look,
- you want a ceremony even if nothing is blocked.

Ceremonies cannot be skipped. Without one, no new backlog tickets can legally appear. KanbanMaster-cold prefers to pull same-module or dependent tickets into `open` together. That is a pull habit, not a cluster id on the file.

**5. Design gap — only then the architect again.**
If Worker-cold hits the stop-rule (unsure, living-knowledge does not cover it, or it looks like a design change), it comments and stops. You spawn KanbanMaster-cold. If the gap is design-level, KanbanMaster-cold writes a TA ticket (the problem only, no solution). You then spawn DesignArchitect-warm on that TA. It writes the solution into the design spec or living-knowledge and comments on the TA. Then the worker can continue.

A Mini Phase 1 is a new product area the current design spec does not cover. You declare it and send DesignArchitect-warm back to update that slice. A normal feature inside an already-mapped module is not Mini Phase 1.

**6. Workflow change — only the steward.**
If a rule of this workflow needs to change, start WorkflowSteward-warm in the `agent-workflow-spec` repo. Product agents do not edit the workflow and do not start the steward.

## Frequency, short

| Role | How often | Resume? |
| --- | --- | --- |
| DesignArchitect-warm | Continuous through Phase 1. In Phase 2 only for a TA or a Mini Phase 1. | Yes, same session. |
| KanbanMaster-cold | One ceremony when the board needs a pull, a close, or a TA. Not every ticket. | No. New session. |
| Worker-cold | Every ticket you want implemented. | Only for the same module or a Depends-on. |
| WorkflowSteward-warm | Rare. Only when the workflow itself changes. | Yes, in the workflow-spec repo. |

You drive the board. Several tickets may sit in `in-progress` or `in-testing` at once. There is no rule that the worker must wait for a test to finish before you pick another ticket. There is a rule against two Worker-cold sessions writing the same repo at the same time.

## What you do not do

- Do not start Worker-cold before you accept Phase 1.
- Do not ask DesignArchitect-warm to implement a ticket or to read the board.
- Do not ask Worker-cold to cut a ticket, write living-knowledge, or mark `done` / `closed`.
- Do not resume a month-old KanbanMaster session. Start a new one.
- Do not start a second Worker while one is writing.
- Do not use a worktree. Do not start with Grok Build Memory on.
- Do not copy the steward init file or `initial_spec/` into the product repo.

How to start each role, the first message, and what each may write is in the matching file in this folder.
