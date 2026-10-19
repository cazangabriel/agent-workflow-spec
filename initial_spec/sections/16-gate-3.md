## 16. Gate 3 — industry comparison

Written 2026-10-19 after Gate 1 acceptance. This is a comparison, not a rewrite.

### 16.1 What we compared against

- Classic software teams: architect + delivery lead + developer, kanban, human test.
- Domain-driven design: ubiquitous language, bounded contexts, context maps.
- Spec-driven development for coding agents: GitHub Spec Kit (specify → plan → tasks → implement), AWS Kiro (requirements / design / tasks), Giant Swarm-style review-the-plan-not-the-diff.
- Multi-agent coding practice in 2026: Hermes durable kanban with parallel profile workers; Atlassian assigning Jira items to coding agents; “AI software factory” write-ups that keep planning and review with the human and execution with the agent.
- Fully autonomous “dark factory” setups, which we already rejected.

### 16.2 Recommendation

**Keep this workflow.** Do not replace it with Spec Kit, Kiro, Hermes kanban, or a Jira-assigned-agent setup.

Those tools solve a related problem (give an agent a structured artifact before code). They do not solve the problems this spec was written for: one human, scarce tokens, no unsupervised work, a living design that must stay consistent across a warm architect and cold workers, and a board that is the filesystem.

Borrow the one idea they share with us: the written artifact is what you review, not a surprise diff. We already do that (design spec, then ticket approach, then code).

### 16.3 Where this spec is stronger

- **Role boundaries are explicit.** Industry multi-agent write-ups keep rediscovering that the hard part is “who may change what, and when must they stop.” This spec already answers that for three product roles plus a workflow owner.
- **One shared tree.** Parallel-agent tools (Hermes, Agentic Kanban, Grok worktrees) isolate checkouts so agents do not clobber each other. That is correct for many writers. It is wrong for one writer plus a board the human must see. We rejected worktrees for that reason.
- **Human test is required.** Factory-model write-ups and Spec Kit both leave verification with the human, because agent-driven testing is still expensive and unreliable. That matches the user’s constraint. We did not pretend an agent QA role was affordable.
- **Living-knowledge is designed contents, not a generic memory bucket.** Grok Build Memory, and most agent memory features, are containers. We specified headers, abbreviation lists, and who may write.
- **Workflow changes sit above the product.** Spec Kit and Kiro let the same agent edit process and product. We split that into WorkflowSteward-warm so a product architect cannot rewrite its own rules.
- **No epics.** Spec Kit and Kiro add a task layer under a feature spec. We already decided a ticket is one testable outcome plus one approach. An epic layer would cost tokens and add a document nobody asked for.

### 16.4 Where this spec is weaker

- **Unproven.** Spec Kit and Kiro are in use. This workflow has not been run on a product yet. Gate 4 is the trial.
- **No machine check against the spec.** Spec-driven verification is an industry gap too, but teams that can afford agent tests have a loop we do not. Human test is the right call for cost; it is also the slowest feedback.
- **Two writers of living-knowledge can drift.** DesignArchitect-warm and KanbanMaster-cold both may touch it. Industry context maps exist for this; we have a header convention, not a merge ritual.
- **Warm session can still bloat.** Resuming DesignArchitect-warm is the right trade against cold-start ignorance. It is also the quadratic-cost risk we already named.
- **Markdown board has no claim/lock.** Fine while one Worker-cold writes. It would break if we later allowed parallel workers. Hermes uses a database for that. We should not copy it until we need it.
- **Init files are not written.** Gate 4. Until they exist, the human guides describe a role the platform cannot yet load by name.

### 16.5 Per-area verdict

| Area | Verdict |
|---|---|
| Phase 1 design spec before tickets | Keep. Matches spec-driven practice. |
| Three product roles + steward | Keep. Clearer than a single super-agent. |
| Column folders + generated kanban.md | Keep. Cheaper than a tool board for one human. |
| Human approve-then-test | Keep. Matches factory model, not dark factory. |
| No worktrees | Keep, for this team size. |
| Memory disabled | Keep. Living-knowledge is the designed store. |
| Friction as a fail counter | Keep. Better than a guessed 1–5 score. |
| NEXT file | Keep. Cheaper than scanning filenames. |
| Epics / user stories | Do not add. |
| Parallel workers | Do not add. |
| Agent-driven testing | Do not add, until token cost changes. |

### 16.6 Suggested improvements (not applied)

None of these are worth a spec edit unless the human asks:

1. After the first real product, add one sentence to living-knowledge authorship: if both writers touch the same file in one week, the human picks a single owner for that file until the next ceremony.
2. Gate 4 should write the init files by pointing each role at its human guide plus the sections it must obey, so a cold spawn has a reading list.
3. Do not adopt Spec Kit or Kiro as the process. If a later project wants their templates, that is a product choice, not a workflow replacement.

### 16.7 Status

Accepted by the human on 2026-10-19. Recommendation stands: keep this workflow. No Gate 3 edits were applied to the rest of the spec.
