## 13. Decision log and remaining questions

### 13.1 Resolved

1. Living-knowledge path: `/app-name-1/living-knowledge/`.
2. Ticket store: `/app-name-1/tickets/<column>/`. Columns include `in-testing`. Filename = kind + increment + optional `C<n>` + types + short slug (≤80 chars). State is the folder, not the filename. `kanban.md` generated.
3. Kind prefixes: `F` and `TA` for now. More only when a new inbox is needed.
4. No zero-padding. Sort by integer.
5. Type abbreviations: per-project list in living-knowledge, written by DesignArchitect-warm (§6.8).
6. `open` target ~10, hard stop 15. Cluster pull that lands on 12–13 is allowed.
7. Cluster id `C<n>` stamped by KanbanMaster-cold. Worker proposes clusters from the board, does not open every body to discover them.
8. Workers do not create tickets. Human and worker may move files between columns they are allowed to use (§8.3).
9. One worker at a time. Reuse the same instance on a cluster (§5.11).
10. Human approve-then-test via `in-testing`. Human marks `done`. Agent-tester rejected.
11. Blocked tickets outrank starting an unrelated `open` ticket.
12. No feature branches. Push `dev`. Human releases `dev` → `main`.
13. Escalate band: if one ticket/change looks like ~15–20% of the ~6M weekly planning assumption, ask before doing it (§5.8).
14. No epic / user-story layer. Design spec + tickets + clusters are the traceability.
15. Ticket granularity: one human-testable outcome + one approach discussion (§8.14).
16. Spec lives as section files under `initial_spec/sections/`.
17. Topology, UL package, living-knowledge header, friction counter, Phase 1/2, §0.7 statements rule: as already written.

### 13.2 Still open

1. Grok Build platform constraints (session spawn, shared working tree).
2. Weekly token budget remains a user planning assumption (~6M), not a vendor fact.
3. Exact first project type-abbreviation list — written per project in Phase 1, not in this workflow spec.

### 13.3 Suggested next step

Continue Gate 1 refinement. No implementation until the user explicitly says so.
