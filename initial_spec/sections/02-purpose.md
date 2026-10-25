### 2.2 What “optimal” means here

Design an optimal team workflow based on proven methods that give good or outstanding results in real-life human teams — then adapt those methods to agents, token cost, and context windows.
Violation: copying a human-team practice that ignores token cost, or inventing an agent practice with no human-team basis, without saying which.
Example: a human team uses a standup. This workflow does not add a daily standup agent, because the token cost has no matching benefit. That omission is stated, not silent.

### 2.3 Suggested targets, and what is now decided

Original brief treated these as starting targets, not hard constraints. User decisions since then:

- Decided: no TechnicalSupervisor instance. Spawned roles are DesignArchitect-warm, KanbanMaster-cold, and Worker-cold. No non-technical manager. No separate ticket-cutter besides KanbanMaster-cold.
- DesignArchitect-warm owns the design spec. KanbanMaster-cold cuts tickets and runs the board. Worker-cold implements.
- Knowledge that must survive a session lives in organised `.md` files on git alongside the project (living-knowledge).
- Worker agents are transient: spawned for one or a few tasks, so their context windows stay short.
- Decided: one worker at a time. Do not run parallel workers until the user reopens that constraint.
- Decided: human-approve-then-test loop is required on every ticket. The human may skip actually performing a test as a gut call, but the loop itself stays. Agent-driven testing is not viable yet because it would consume scarce tokens.
- Decided: two staffing phases (§5.5). Phase 1 = design spec accepted. Phase 2 = ticketed delivery. Mini Phase 1 when a new bounded context is added.
- Decided: workers do not write living-knowledge. They write ticket comments and raise unknowns to the human.

Violation: a later edit that reintroduces a non-technical manager, or that drops the human test loop because agent testing "would be faster."
Example: an editor does not add a manager role and call it a cleanup. The decision above forbids it.
