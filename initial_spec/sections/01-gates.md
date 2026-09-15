## 1. Conversation protocol (gated; do not skip gates)

Work proceeds in this order. Do not collapse gates.

### Gate 1 — Understand, then rewrite the user brief

1. Read the user prompt (this source brief) and apply its rules and skills.
2. Rewrite that prompt into an organised specification with proper sections.
3. Preserve logic and relevant spec details. Do not lose them for tidiness.
4. Ask questions rather than inventing wild assumptions. The user prefers time spent on clarification over a high-quality-looking document that means something else.
5. This rewrite is **not** yet the design of the agent interaction workflow.

**Current position:** Gate 1 deliverable exists and is being refined in place as section files. Spawned roles are DesignArchitect-warm, KanbanMaster-cold, Worker-cold. Gate 3 (industry comparison as a separate exercise) has not been run as a full survey. Implementation is still Gate 4.

### Gate 2 — User accepts the rewrite

Only after the user is happy with this organised rewrite do we continue.

### Gate 3 — Discussion, comparison, refinement

Discuss:

- Which ideas are good, bad, or need refinement.
- Comparison with equivalent workflows in research / theory and with workflows already used in practice by companies.
- Whether this spec is weak enough that an existing workflow should be used instead.
- Whether this spec has merits or strong points over what exists.
- Recommended approach overall, and per subsection.
- Weaknesses and suggested improvements.

Only after that discussion do we decide what changes to make to the spec that resulted from the brief.

The original brief also listed these questions under “What I need.” They belong at Gate 3, not Gate 1:

- Assess the idea against industry standards. What is better or worse?
- Recommend this approach or another, as a whole or for any subsection.
- Name weaknesses and improvements.

### Gate 4 — Implement the workflow

When the user explicitly confirms readiness, implement the workflow / spec for agent interaction (Grok Build).
