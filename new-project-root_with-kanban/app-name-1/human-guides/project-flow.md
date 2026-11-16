# Project flow — which agent, when

This file is for you. Agents do not treat it as living-knowledge.

## The four roles

- DesignArchitect-warm owns the product design. Warm: resume the same session in Phase 1.
- KanbanMaster-cold runs the board. Cold: a new session for each ceremony.
- Worker-cold implements one agreed ticket. Cold by default.
- WorkflowSteward-warm owns changes to the workflow specification. It runs in the agent-workflow-spec repo, not in the product repo.

## Start a product repo — before Phase 1

1. Create a private GitHub repo. Clone it. Create and check out a `dev` branch. All agent work happens on `dev`. No feature branches.
2. Copy the contents of `new-project-root_with-kanban/` into the new repo root. Rename `app-name-1` to the real app name.
3. Do not copy the steward init file, the steward AGENTS.md, or the agent-workflow-spec itself.
4. Commit and push to `dev`.

## Order of a project

1. Phase 1 — design, before any feature ticket. Start DesignArchitect-warm. Work until the design spec has: bounded contexts and ubiquitous language, 3–4 architectural characteristics, module and layer map, the stack, a `.gitignore` that matches that stack, every Not yet named skill slot filled, component order, unknowns listed, living-knowledge skeletons, both abbreviation lists, and an explicit "accept Phase 1?". You accept Phase 1.
2. KanbanMaster-cold ceremony. It pulls tickets from backlog into open, reviews blockers and friction, closes accepted done tickets, and regenerates `kanban.md`.
3. Worker-cold implements an assigned ticket. It commits and pushes to `dev`, then moves the ticket to `in-testing`. You pull, test or gut-skip, and signal done. Worker-cold then moves it to `done`. It does not move to `closed`.
4. Design gap. If the worker hits the stop-rule, it comments and stops. You spawn KanbanMaster-cold. If the gap is design-level, it writes a TA ticket. You then spawn DesignArchitect-warm on that TA.
5. Workflow change. Start WorkflowSteward-warm in the agent-workflow-spec repo. Product agents do not edit the workflow specification.

## Frequency

| Role | How often | Resume? |
|------|-----------|---------|
| DesignArchitect-warm | Continuous through Phase 1. In Phase 2 only for a TA or Mini Phase 1. | Yes, same session. |
| KanbanMaster-cold | One ceremony when the board needs a pull, a close, or a TA. | No. New session. |
| Worker-cold | Every ticket you want implemented. | Only for the same module or a Depends-on. |
| WorkflowSteward-warm | Rare. Only when the workflow itself changes. | Yes, in the workflow-spec repo. |

One Worker-cold at a time. Several tickets may sit in `in-progress` or `in-testing`. You drive the board.

## What you do not do

- Do not start Worker-cold before you accept Phase 1.
- Do not ask DesignArchitect-warm to implement a ticket or to read the board.
- Do not ask Worker-cold to cut a ticket, write living-knowledge, or mark done or closed.
- Do not use a worktree. Do not start with Grok Build Memory on.
- Do not copy the steward init file or the agent-workflow-spec into the product repo.
