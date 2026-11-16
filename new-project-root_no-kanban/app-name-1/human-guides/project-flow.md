# Project flow — which agent, when

This file is for you. Agents do not treat it as living-knowledge.

## The three roles

- DesignArchitect-warm owns the product design. Warm: resume the same session in Phase 1.
- Worker-cold implements one agreed feature. Cold by default.
- WorkflowSteward-warm owns changes to the workflow specification. It runs in the agent-workflow-spec repo, not in the product repo.

## Start a product repo — before Phase 1

1. Create a private GitHub repo. Clone it. Create and check out a `dev` branch. All agent work happens on `dev`. No feature branches.
2. Copy the contents of `new-project-root_no-kanban/` into the new repo root. Rename `app-name-1` to the real app name.
3. Do not copy the steward init file, the steward AGENTS.md, or the agent-workflow-spec itself.
4. Commit and push to `dev`.

## Order of a project

1. Phase 1 — design, before any feature. Start DesignArchitect-warm. Work until the design spec has: bounded contexts and ubiquitous language, 3–4 architectural characteristics, module and layer map, the stack, a `.gitignore` that matches that stack, every Not yet named skill slot filled, component order, unknowns listed, living-knowledge skeletons, both abbreviation lists, and an explicit "accept Phase 1?". You accept Phase 1.
2. Worker-cold discusses a feature when you ask it to discuss. It does not start implementation in that reply. It implements only when you explicitly ask it to implement the feature. It commits and pushes to `dev`. You pull, test or gut-skip, and signal done. Worker-cold then marks the feature done in `features.md`.
3. Design gap. If the worker hits the stop-rule, it comments and stops. If the gap is design-level, you spawn DesignArchitect-warm. It writes the solution into the design spec or living-knowledge.
4. Workflow change. Start WorkflowSteward-warm in the agent-workflow-spec repo. Product agents do not edit the workflow specification.

## Frequency

| Role | How often | Resume? |
|------|-----------|---------|
| DesignArchitect-warm | Continuous through Phase 1. In Phase 2 only for a design gap. | Yes, same session. |
| Worker-cold | Every feature you want discussed or implemented. | Only for the same module or a dependent feature. |
| WorkflowSteward-warm | Rare. Only when the workflow itself changes. | Yes, in the workflow-spec repo. |

One Worker-cold at a time. Several features may sit in progress. You drive which feature is next.

## What you do not do

- Do not start Worker-cold before you accept Phase 1.
- Do not ask DesignArchitect-warm to implement a feature.
- Do not ask Worker-cold to start implementation when you only asked for a discussion.
- Do not ask Worker-cold to write living-knowledge or mark a feature done before you signal it.
- Do not use a worktree. Do not start with Grok Build Memory on.
- Do not copy the steward init file or the agent-workflow-spec into the product repo.
