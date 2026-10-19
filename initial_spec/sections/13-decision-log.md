## 13. Decision log and remaining questions

### 13.1 Resolved

1. Living-knowledge path: `/app-name-1/living-knowledge/`.
2. Tickets: `/app-name-1/tickets/<column>/` including `in-testing`.
3. Filename: `<module>-<n>-<kind>-<layers>-<slug>.md`.
4. Abbreviation lists: Phase 1 artifact `/app-name-1/living-knowledge/abbreviations.md`, written by DesignArchitect-warm, accepted by the human (§6.8). Not an open workflow-spec item.
5. Kind prefixes: `F` and `TA` for now.
6. Shared increment, no zero-padding. Agents sort by integer.
7. Named clusters dropped.
8. `open` target ~10, hard stop 15.
9. Workers do not create tickets. Human-driven WIP.
10. Human marks `done`. No feature branches. Push `dev`; human releases to `main`.
11. Escalate band: DA-warm / KM-cold only. The ~6M figure is a user planning heuristic only; xAI does not publish a precise weekly quota. The human may form an informal estimate over time from observed burn. The band stays a rough “ask first” signal the human can adjust.
12. No epics. Granularity = one testable outcome + one approach discussion.
13. Grok Build mapping locked in §14: human-spawned roles; no write-capable worker subagents; shared tree on `dev` (no worktrees for this team); DA-warm resumes; KM/Worker are fresh sessions.
14. **Grok Build Memory is disabled.** It is a simpler automatic cache, not living-knowledge. Start sessions with `grok --no-memory` or `GROK_MEMORY=0`. Living-knowledge and tickets remain the only source of truth.
15. Shared-tree guards (§14.6): session-start worktree check, commit+push proof, session rename, no subagent worktree isolation.
16. Human-guide files (§14.7): three `how-to-for-human` markdown files under `/app-name-1/human-guides/`, generated once at project start, for the human only.
17. Spec lives as `initial_spec/sections/`.

### 13.2 Still open

1. Exact on-disk shape of `.grok/agents/` init files — Gate 4.
2. Who owns changes to *this* workflow once a product project is running (product architect vs a higher layer). See discussion; not yet locked.

### 13.3 Next

Gate 1 is ready for the user to accept or request a Gate 3 pass. No implementation until the user says so.
