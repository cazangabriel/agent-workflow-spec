## 1. Conversation protocol (gated; do not skip gates)

A gate is a stage that must finish before the next stage starts. Skipping a gate means doing a later stage while an earlier stage is not accepted.

Required: work proceeds in this order. Do not collapse gates.
Violation: starting Gate 4 implementation while Gate 3 is not accepted.
Example: the human has not accepted Gate 3. You do not write `.grok/agents/` files and call it implementation.

### Gate 1 — Understand, then rewrite the user brief

1. Read the user prompt and apply its rules and skills.
2. Rewrite that prompt into an organised specification with proper sections.
3. Preserve logic and relevant spec details. Do not lose them for tidiness.
4. Ask questions rather than inventing assumptions. The user prefers time spent on clarification over a high-quality-looking document that means something else.
5. This rewrite is not yet the design of the agent interaction workflow.

Status: Accepted. User accepted the rewrite on 2026-10-19. The remaining init-file on-disk shape is Gate 4, not a Gate 1 blocker.
Violation: a Gate 1 rewrite that drops a constraint because it looks costly.
Example: the brief says "no feature branch." The rewrite keeps that sentence. It does not replace it with "feature branch when appropriate."

### Gate 2 — User accepts the rewrite

Status: Done. User accepted Gate 1 on 2026-10-19.
Violation: treating Gate 1 as accepted because the rewrite exists, before the user says so.

### Gate 3 — Discussion, comparison, refinement

Discuss: which ideas are good, bad, or need refinement; comparison with research and with company practice; whether this spec is weak enough that an existing workflow should be used instead; merits over what exists; recommended approach; weaknesses.
Only after that discussion do we decide what changes to make.
Status: Accepted 2026-10-19. Comparison is §16. Recommendation was to keep this workflow. No further Gate 3 edits unless the user reopens them.
Violation: editing the spec from a Gate 3 opinion before the user accepts the comparison.

### Gate 4 — Implement the workflow

When the user explicitly confirms readiness, implement the workflow for agent interaction (Grok Build).
Violation: writing product-repo init files before that confirmation.
Example: the user has not said Gate 4 is open. You do not copy `.grok/agents/` into a product repo.
