## 13. Decision log and remaining questions

### 13.1 Resolved

1. **Living-knowledge path.** Repo-relative `/app-name-1/living-knowledge/`.
2. **Ticket store.** One markdown file per ticket under `/app-name-1/tickets/<column>/`. Filename is identity only (`F-10-DB.md`). Column folder is state. `kanban.md` is generated. `open` cap 10. Only KanbanMaster-cold may `backlog → open` and `done → closed`. Workers do not create tickets.
3. **Concurrency.** One worker at a time for now. Same Worker-cold may continue on related tickets (§5.11).
4. **Human approve-then-test loop.** Required step. Human may gut-skip a given test. Agent-tester rejected. Pass → `done`, not `closed`.
5. **Spec format.** Markdown only. No `.docx`. Spec now lives as section files under `initial_spec/sections/`.
6. **Document versions.** Do not increment until the user says to.
7. **Questions and statements to the human.** Must carry their own context (§0.7).
8. **Shared vocabulary.** A + B + DDD extra (§2.4–§2.5).
9. **Living-knowledge author skill set.** Full §6.6 must + should list.
10. **Team topology.** Spawn DesignArchitect-warm, KanbanMaster-cold, Worker-cold. TechnicalSupervisor is the skill family name only.
11. **Semantic-defect pass.** Editors run §0.8 after material changes.
12. **Living-knowledge file header.** Mandatory short header. Not called “module card.”
13. **Friction counter.** Integer on ticket, +1 per event.
14. **Phase 1 / 2 definitions and exit tick-list.** §5.5. User agreed the definitions.
15. **Commit line.** `KanbanMaster-cold F-10-DB add recipe table`.

### 13.2 Still open

1. Grok Build platform constraints (session spawn, shared working tree).
2. “Feature branch when appropriate.”
3. Ticket type abbreviations: fixed list vs per-project list in living-knowledge.
4. Ticket folder name `/app-name-1/tickets/` is a proposal.
5. Kind prefix `F` only, or other families later?
6. Zero-padding width for the shared ticket increment.
7. `open` cap of 10 is a proposal.
8. Escalate threshold in §5.8 (~15–20% of the 6M weekly planning assumption).
9. Weekly token budget is a user planning assumption (~6M), not a vendor fact.

### 13.3 Suggested next step

Continue Gate 1 refinement on the section files. No implementation until the user explicitly says so.
