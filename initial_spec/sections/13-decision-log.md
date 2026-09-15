## 13. Decision log and remaining questions

### 13.1 Resolved

1. Living-knowledge path: `/app-name-1/living-knowledge/`.
2. Tickets: `/app-name-1/tickets/<column>/` including `in-testing`.
3. Filename: `<module>-<n>-<kind>-<layers>-<slug>.md` e.g. `AM-10-F-DB-add-recipe-table.md`. ≤80 chars. No state, no friction, **no cluster token** in the name.
4. Two Phase-1 lists in living-knowledge, written by DesignArchitect-warm: module abbreviations + layer abbreviations. Reserved cross-cut module token (default `XCUT`).
5. Kind prefixes: `F` and `TA` only for now.
6. Shared increment, no zero-padding. Agents sort by the integer. Explorer text-sort will not put `AM-9` after `AM-10` — that is accepted.
7. Named ticket clusters (`C3` etc.) **dropped**. Same-module filenames + `Depends on` + human-driven next-ticket choice replace them.
8. `open` target ~10, hard stop 15.
9. Workers do not create tickets. Human drives WIP: several `in-progress` / `in-testing` allowed. No wait-for-testing-before-next-push rule.
10. Human marks `done`. Worker never marks `closed`.
11. No feature branches. Push `dev`. Human merges `dev` → `main`.
12. Escalate band 15–20% of ~6M: DesignArchitect-warm and KanbanMaster-cold only. Worker tickets that look that expensive are split instead.
13. No epics / user stories. Granularity = one testable outcome + one approach discussion.
14. KanbanMaster-cold must not write lazy slugs or lazy titles.
15. Spec lives as `initial_spec/sections/`.

### 13.2 Still open

1. Grok Build platform constraints.
2. Weekly token budget is still a user planning assumption (~6M).
3. Concrete module/layer abbrev lists — per project, Phase 1, not this spec.

### 13.3 Next

Continue Gate 1 refinement. No implementation until the user says so.
