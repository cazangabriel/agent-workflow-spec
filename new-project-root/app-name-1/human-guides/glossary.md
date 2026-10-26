# Glossary

Plain-language definitions for this workflow. Read this instead of hunting an old conversation.

**Friction.** A number on a ticket that starts at 0. Each time something fails — a test fails, you send the ticket back, or the worker hits the stop-rule — the worker adds 1 and writes a short comment saying why. There is no upper limit and it is not a difficulty rating the agent invents. A high number means the ticket has bounced a lot. Look at it on `kanban.md` when you want to see which tickets have been painful.

**Living-knowledge.** The markdown files under `/app-name-1/living-knowledge/` that hold how this product is actually built: module map, conventions, gotchas, abbreviation lists. Agents read them so they do not guess. Workers do not write them. This is the source of truth, not Grok Build Memory and not session chat.

**Design spec.** The Phase 1 document that says what the product is, which modules exist, which technologies were chosen, and in what order components should be built. Only DesignArchitect-warm and you write it. KanbanMaster-cold reads it to cut tickets.

**`.gitignore`.** The file at the product repo root that tells git which paths not to commit. It is not living-knowledge. In Phase 1, after the stack is agreed, DesignArchitect-warm proposes one that matches that stack and writes it only after you agree. If the stack later changes, it proposes an update the same way.

**F ticket.** A feature / implementation ticket for Worker-cold. One thing you can test, plus one approach discussion.

**TA ticket.** A technical-architecture ticket for DesignArchitect-warm. It states a design problem (impact and what is needed). It does not contain the solution. Only KanbanMaster-cold creates them.

**Warm.** DesignArchitect-warm, and WorkflowSteward-warm. Resume the same session when you can. After a reset, re-read the files before advising.

**Cold.** KanbanMaster-cold and Worker-cold. Start a new session each ceremony or assignment. Do not resume an old thread as if it were still cheap. You may reuse the same Worker-cold session only for a related next ticket (same module or a declared dependency).

**Phase 1.** Design work until the design spec is good enough to cut tickets from. You explicitly accept Phase 1 done. Exit includes a `.gitignore` that matches the agreed stack.

**Phase 2.** Ticketed delivery against that accepted spec. Ceremonies cannot be skipped.

**Mini Phase 1.** A new product area with its own language was added that the current design spec does not cover. Send DesignArchitect-warm back to update the spec for that slice. A normal new feature inside an already-mapped module is not Mini Phase 1.

**Stop-rule.** The worker stops when it is not sure how to proceed, hits something living-knowledge does not cover, or the work looks like a design change. It comments, tells you, and does not invent the missing rule.

**Escalate band.** A rough “ask first” signal for DesignArchitect-warm and KanbanMaster-cold. If a design change looks like it would burn a large share of the week’s tokens, they stop and ask you before doing it. Worker tickets that look that large are split instead.

**Column folders.** Ticket state is the folder the file sits in (`open`, `in-progress`, `in-testing`, `blocked`, `done`, `closed`, `closed-wont-do`, `backlog`). The filename does not contain the state.

**NEXT.** The file `/app-name-1/tickets/NEXT` that holds the next ticket number as a single integer. Only KanbanMaster-cold updates it. Nobody scans hundreds of filenames to find the next number.

**Worktree.** An isolated second checkout Grok Build can create. This workflow does not use them. All roles work on the shared tree so you can see edits immediately.

**Grok Build Memory.** An optional automatic note-taking feature in Grok Build. Disabled for this workflow. Start every role with `grok --no-memory`. Living-knowledge (and, for the steward, the section files) is the designed memory.

**Kanban ceremony.** A fresh KanbanMaster-cold session that pulls tickets from backlog into open, reviews blockers and friction, closes accepted done tickets, and regenerates `kanban.md`. Without it, no new backlog tickets can legally appear.

**WorkflowSteward-warm.** The agent that owns changes to this workflow spec. It does not work inside a product repo. You spawn it in the workflow-spec repository when a rule needs to change, or if the original design conversation is lost. See `human-guides/WorkflowSteward-warm.md` for the exact recovery steps. Product agents must not edit the workflow and must not spawn this role.
