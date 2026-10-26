## 13. Decision log and remaining questions

### 13.1 Resolved

1. Living-knowledge path: `/app-name-1/living-knowledge/`.
2. Tickets: `/app-name-1/tickets/<column>/` including `in-testing`.
3. Filename: `<module>-<n>-<kind>-<layers>-<slug>.md`.
4. Abbreviation lists: Phase 1 artifact `/app-name-1/living-knowledge/abbreviations.md`, written by DesignArchitect-warm, accepted by the human (§6.8). Not an open workflow-spec item.
5. Kind prefixes: `F` and `TA` for now.
6. Shared increment, no zero-padding. Agents sort by integer. Next number is `/app-name-1/tickets/NEXT` (one integer; only KanbanMaster-cold writes it). Do not scan filenames.
7. Named clusters dropped.
8. `open` target ~10, hard stop 15.
9. Workers do not create tickets. Human-driven WIP.
10. Human marks `done`. No feature branches. Push `dev`; human releases to `main`.
11. Escalate band: DA-warm / KM-cold only. The ~6M figure is a user planning heuristic only; xAI does not publish a precise weekly quota. The human may form an informal estimate over time from observed burn. The band stays a rough “ask first” signal the human can adjust.
12. No epics. Granularity = one testable outcome + one approach discussion.
13. Grok Build mapping locked in §14: human-spawned roles; no write-capable worker subagents; shared tree on `dev` (no worktrees for this team); DA-warm resumes; KM/Worker are fresh sessions.
14. **Grok Build Memory is disabled.** It is a simpler automatic cache, not living-knowledge. Start sessions with `grok --no-memory` or `GROK_MEMORY=0`. Living-knowledge and tickets remain the only source of truth.
15. Shared-tree guards (§14.6): session-start worktree check, commit+push proof, session rename, no subagent worktree isolation.
16. Human-guide files (§14.7): under `/app-name-1/human-guides/`, for the human only. Canonical copies live in this repo under `new-project-root/app-name-1/human-guides/`: the four role manuals, `glossary.md`, and `project-flow.md`.
17. Ticket body template and `kanban.md` layout locked in §8.12 and §8.10.
18. Spec lives as `initial_spec/sections/`.
19. **WorkflowSteward-warm owns this workflow spec** (§15). Product roles must not edit it and must not spawn the steward. It lives in the workflow-spec repo. A lost conversation is recovered by spawning it there and pointing it at the README and the decision log. Section files are the knowledge.
20. Gate 1 accepted 2026-10-19. Gate 2 done. Gate 3 accepted 2026-10-19. Comparison is §16. Recommendation: keep this workflow.
21. Init-file on-disk shape is locked. The product root is the folder `new-project-root/`. Copy its contents into a new product repo. It holds the product `AGENTS.md`, the three product init files (`.grok/agents/design-architect-warm.md`, `kanban-master-cold.md`, `worker-cold.md`), the human guides, empty living-knowledge, the ticket column folders, and `tickets/NEXT` containing `1`. Rename `app-name-1` to the real app name. The steward init file and the root `AGENTS.md` of this repo stay here. Do not copy them into a product repo.

### 13.2 Still open

None. The workflow design is locked. Starting a product repo is use, not an open design item.

### 13.3 Next

Start a product repo. Copy the contents of `new-project-root/` into it. Work on `dev`. Start DesignArchitect-warm for Phase 1. Do not start KanbanMaster-cold or Worker-cold before Phase 1 is accepted.

A human-only starter prompt lives at `for-human-only/precision-starter.md`. It is not part of this workflow. Agents must not treat it as spec.
