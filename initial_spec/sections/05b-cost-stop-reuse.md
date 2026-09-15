## 5 continued — cost arithmetic, stop-rule, worker reuse

Read after [05-team-model.md](05-team-model.md).

### 5.6 Option 1 cons — token arithmetic (what Con 1 actually is)

Con 1 is not “one mixed ceremony costs 50k+50k=100k instead of two 50k ceremonies.” Two ceremonies **in the same standing session** still share one growing input context.

It is also not “triage × design = 100k from 10k×10k.” A single prompt’s cost is roughly **input tokens + output tokens**. Input is (system) + (skills files) + (living-knowledge loaded) + (tickets opened) + (conversation so far). Design material and triage material **add** inside that input. They do not multiply as 10k×10k in one shot.

What *does* blow up:

- After N turns with no reset, each new turn re-sends the accumulated history. Total cost across the session is roughly the **triangle number** of that growth: turn 1 sends H, turn 2 sends H+Δ, … turn N sends ~NΔ. That is **quadratic in session length**, not linear in “number of concerns.”
- If both triage chatter and design chatter stay in that history, H grows faster. Two concerns in one uncompacted session therefore cost more *over a week* than two short sessions that each load only the files they need and then die.
- Writing the result into living-knowledge and resetting is what makes the cost additive-per-ceremony instead of quadratic-per-week.

So: separate ceremonies help **only if** the session does not keep both histories warm. Files are the memory. The 30M-token recipes-site stretch is the quadratic pattern, not proof that architecture and triage are each 15M.

The two-image split attacks this: ticket junk dies with KanbanMaster-cold; design talk stays with DesignArchitect-warm. It does **not** make long-lived design free. Design conversation still grows quadratically inside the Design architect session if that session never writes-to-spec-and-resets.

### 5.7 Stop-rule and who gets spawned

**Stop-rule (the condition that was missing before, now explicit):**

The worker may open another **already existing** living-knowledge file only when the current ticket or an already-loaded file **points at it** (Relies-on / dependency / living-knowledge file header).

The worker **stops** — no more inventing, no more “I will just assume” — when any of these is true:

- they are not sure how to approach the ticket;
- they hit a boundary, unknown, or unexpected chunk that living-knowledge does not clearly cover;
- the next fact they need is not written;
- the work looks like a design-level coupling / structure change.

Then: comment on the ticket (self-contained; if escalating, address the Kanban master by name in the comment), tell the human, increment friction (§8.9). Do not create a TA ticket. Do not edit the design spec.

Escalation ladder (human still drives spawns):

1. Worker ↔ human (always first).
2. If both agree it needs the board role: human spawns **KanbanMaster-cold**; worker’s comment is waiting.
3. If the Kanban master cannot solve it, or it is design-level: Kanban master writes a **TA ticket** (problem only). Human spawns **DesignArchitect-warm** on that TA ticket only.
4. Worker-spawns-Design-architect is **forbidden** until the human separately approves that evolution. Reliability signal for that future decision: friction counters + fail comments on closed tickets, aggregated by the Kanban master. No automatic unlock.

**KanbanMaster-cold spawn triggers:** ceremony time; human asks; worker comment addressed to them; board is dry (`backlog`+`open` emptying); blocker review.

**DesignArchitect-warm spawn / attention triggers:** Phase 1 work; a TA ticket in `open`; human asks a structural question; mini Phase 1.

### 5.8 Planning token budget (user estimate, not a vendor fact)

Grok does not publish SuperGrok weekly token quotas in this spec’s sources. The following is the **user’s planning assumption**, recorded so “cheap / expensive” has a denominator:

- User observed ~30M+ tokens over ~2 weeks on a Grok Build recipes site (the failure mode this workflow is trying not to repeat).
- User’s working budget for *this* kind of work: on the order of **6 million tokens per week**, leaving headroom inside a guessed SuperGrok week, with leftover quota usable if it exists.
- That number is an estimate. It may be wrong. It is still the figure “expensive” is judged against until the user replaces it.

What “15–20% escalate band” meant (proposal only, **not frozen**): if an agent believes a single ticket or design change would burn on the order of 15–20% of the 6M weekly planning assumption (roughly 0.9–1.2M tokens), they must ask the human before doing it, with the estimate and the uncertainty. It is a “this might eat the week” tripwire, not a measured law. Ignore or replace the percentage; keep the idea that expensive is relative to §5.8.

### 5.9 Defects and weak points of the two-image split

- Extra hop: Worker-cold comment → human spawns KanbanMaster-cold → TA ticket → human spawns DesignArchitect-warm. Worth it only if TA tickets stay self-contained (the Design architect can solve them without opening a non-TA ticket).
- Two writers of living-knowledge can drift. Example: KanbanMaster-cold writes “cancel by setting status=void”; DesignArchitect-warm later writes “cancellation is a new CancelledOrder record.” Two sources of truth. Rule: design-shaped changes go through the spec or a TA; Kanban master writes gotchas/conventions; if unsure which file, ask the human rather than both writing.
- `kanban.md` must stay generated from ticket files and column folders. The human does not edit it by hand. §8.10.
- Workers do not create tickets (§8.11). Extra work is a comment; KanbanMaster-cold cuts the ticket.
- “Finished design spec then tickets” can slide into waterfall over-design. Phase 1 exit is *sufficient spec to break tickets*, not *every class named*. Spec remains editable via TA in Phase 2.
- Cold Kanban master ticket quality = design spec quality. Intended. If the spec is vague, tickets will be vague; fix the spec, do not make the Kanban master long-lived “to remember what we meant.”
- Design architect forbidden to read non-TA tickets means a bad TA ticket yields a bad design. Feature if TA writing is enforced; defect if it is not.

### 5.10 Init files versus commit names

An **init file** is the skills-and-permissions document an image reads at spawn (`DesignArchitect-warm.md`, `KanbanMaster-cold.md`, `Worker-cold.md`).

A **commit agent name** is the spawn name in the git message, e.g. `KanbanMaster-cold F-10-DB add recipe table`. Same image, two strings. Do not put the init filename in the commit line.

### 5.11 Reuse the same Worker-cold on related tickets

Re-initialising a worker for every ticket re-pays context cost. Use the **same** Worker-cold instance when:

- the next ticket depends on the one just finished, even if the layers differ; or
- both tickets sit in the same module, even if the layer set grows or shrinks (module M1 layers A–B, then M1 A–B–C or only B).

Start a **new** Worker-cold when the next ticket is a different module with no dependency on what this instance just loaded.

Still one worker at a time. Do not keep one instance for a whole 10-ticket `open` slice if later tickets are unrelated — that recreates the junk pile the warm/cold split exists to avoid.

KanbanMaster-cold should **cluster** `backlog → open` pulls toward related tickets so this reuse is possible on purpose.
