## 5. Team model

### 5.1 Skill family and two images

**TechnicalSupervisor is not a spawned agent.** It was an earlier name for the shared skill family. You do **not** spawn a TechnicalSupervisor beside the others. Same high-expertise set (§6.6 + stack + ticket literacy). Two images of that family have different lifetime and write rights — ISP applied to sessions, because one session cannot be both “reset every board” and “warm on design.”

Spawned agent types (names include intended lifespan):

| Spawn name | Init file | Lifespan | Job |
|---|---|---|---|
| **DesignArchitect-warm** | `DesignArchitect-warm.md` | Project lifetime, occasional reset. After reset, reads design spec + living-knowledge file headers + open TA tickets to get warm again | Design spec, -ilities, TA tickets, design-level living-knowledge when a TA requires it |
| **KanbanMaster-cold** | `KanbanMaster-cold.md` | One ceremony / one `open`-cap slice (~10 tickets), then a new instance | Board, non-TA tickets, living-knowledge that is not the design spec, worker facilitation, TA *creation* (problem only) |
| **Worker-cold** | `Worker-cold.md` | One ticket, or a short run of related tickets (§5.11), then a new instance | Implementation after human-approved approach |

There is still no non-technical manager and no third ticket-cutter. Architect-first: when structure and board speed conflict, structure wins or the human is asked. Do not shrink a design-level coupling problem into a cute worker ticket.

Init files are listed in the table above. Commit messages use the spawn name + ticket code + human title, e.g. `KanbanMaster-cold F-10-DB add recipe table`. Do not put the init filename in the commit line.

**Worker-cold**

- Software development, propositional logic (internal), domain of this app.
- OOP, OOD, SOLID, temporal coupling, common pitfalls and anti-patterns. Code must follow those; names must be readable (no `x` for a business idea). The human reads the code.
- Bite-sized living-knowledge: ticket layers + pointed-at cross-cuts, until the stop-rule (§5.7).
- May not write living-knowledge or the design spec. May not write TA tickets.
- May comment on the non-TA ticket they are executing (even if they did not create it).
- May **not** create tickets. Extra work is a comment on the current ticket; the human spawns KanbanMaster-cold to cut the new ticket (§8.11).
- May not edit fields of a ticket they did not create, other than comments and allowed state moves on the ticket they are executing.
- Stop-rule: tell the human; comment addressed to the Kanban master if both agree to escalate.

**DesignArchitect-warm**

- Long-lived so design discussion can stay warm.
- Reads/writes `project-design-spec*.md` and TA tickets.
- May read/write living-knowledge when a design change requires it.
- Must **not** read the kanban board file, and must **not** read non-TA tickets, unless a TA ticket is so incomplete that there is no other way — that is a Kanban-master writing defect, not a habit.
- Must not pick up high-frequency ticket chatter. That is the point of the split.

**KanbanMaster-cold**

- Spawned fresh each ceremony so ticket/comment junk dies.
- Reads design spec (read-only). Does not edit design spec.
- Reads/writes non-TA tickets and living-knowledge (gotchas, conventions, module bodies — not the design spec). Regenerates `kanban.md`.
- Creates TA tickets that state the problem, impact, needs. Does **not** put the design solution in the TA ticket.
- Looks at `in-progress`, `blocked`, and other columns during a ceremony, not only `backlog`/`open`/`done`/`closed`.
- For blockers: if they can see solution option(s), present them to the human; if not, or a decision is required, escalate to the human; if the gap is design-level, write a TA ticket.

This split is **candidate-accepted for spec writing**, with defects recorded in §5.9. Not a claim that ISP on agents is free.

### 5.2 Cost model

Design conversation stays in the long-lived Design architect + the design spec on git. Board/ticket chatter dies with each Kanban master instance. Worker chatter dies with each worker. Mixing those three histories in one never-reset session is the 100k-token failure mode. §5.6 and §5.9.

### 5.3 Concurrency (decided)

One worker at a time for now. `open` cap is a sprint-size buffer, proposed 10, so nine tickets may sit idle on purpose.

Serialisation: KanbanMaster-cold fills `open` from design spec + `backlog` → human + worker pick one `open` ticket → approach approval → implement → human test → `done` → next ceremony `done → closed`.

### 5.4 Topology decision

**Decided:** no non-technical manager. Architect-first. Shared TechnicalSupervisor *skill family* (name only).

**Working topology:** DesignArchitect-warm + KanbanMaster-cold + Worker-cold. Defects of that split are in §5.9.

Holding constraints still in force: no god-agent; small channel count; restore from init files + git; workers transient; living-knowledge and design spec are files, not session memory.

### 5.5 Phase 1 and Phase 2 — definitions (locked)

These are staffing + artifact definitions, not a product waterfall. Implementation still happens as tickets. Phase 1 is not “code the whole system on paper.”

**Phase 1 — design spec accepted.**
The Design architect works with the human until `project-design-spec*.md` is good enough to drive ticket breakdown. That spec includes: bounded contexts and UL starter; **3–4 priority architectural characteristics** for this project (§5.5.1); module/layer map; chosen technologies and how they will be used; component-level implementation order (dependencies), **not** a ticket list. Ticket breakdown is Kanban master work and is not Phase 1’s definition of done.

**Phase 2 — ticketed delivery against that spec.**
Kanban master runs ceremonies, breaks the accepted spec into tickets, maintains living-knowledge (non-spec), watches the board including `in-progress` and `blocked`. Design architect stays long-lived but is invoked through TA tickets and spec edits, not through reading the board. Workers implement `open` tickets. Ceremonies cannot be skipped: without them there is no legal source of new `backlog` tickets.

**Mini Phase 1.** Meaning, spelled out: Phase 2 assumes the design spec’s skeleton (named contexts, module/layer map, chosen stack, component order) still describes the product. If you later add a **new bounded context** — a new product area with its own language and map, e.g. you ship a catalogue and then start a billing context that was not in the spec — that slice is not “just another `F-` ticket.” The human sends DesignArchitect-warm back into Phase 1 *for that slice*: update the spec, pick/confirm -ilities if they change, then KanbanMaster-cold breaks the new spec text into tickets. Routine new features inside an already-mapped module are not Mini Phase 1. The human declares phase changes.

#### 5.5.1 Architectural characteristics (the “-ilities”)

At the start of Phase 1 the Design architect must raise this with the human: pick a short priority list of architectural characteristics for *this* project (Mark Richards / Neal Ford style). Usually **3–4, not a catalogue**. The list is written into the design spec and is used when design options collide.

The user’s standing preference to **consider** (not auto-win on every project): **maintainability** — because it fights big-ball-of-mud, and mud is expensive in both human teams and tokens.

Other characteristics (availability, security, scalability, testability, simplicity, …) are candidates the Design architect must actually argue for against this product, not paste from a conference slide.

#### 5.5.2 Suggested Phase 1 exit tick-list

Tangible. Human gated: the Design architect must ask the human to approve Phase 1 done. None of these is “the model feels ready.”

1. Bounded contexts named; UL starter for each written.
2. 3–4 architectural characteristics ranked and written.
3. Module + layer map written.
4. Technology choices written, with “use with the stack, not against it.”
5. Component-level implementation order written (dependencies visible).
6. Design spec has no silent unknowns on those items; remaining unknowns are listed as unknowns.
7. Living-knowledge *skeletons* exist for the mapped modules (headers + empty-or-thin bodies are allowed; full gotchas come from delivery).
8. Human answers the explicit question: “Do you accept Phase 1 as done for this spec version?”

A first thin slice through the test loop is **useful** but is Phase 2 evidence, not a hidden extra Phase 1 gate, unless the human adds it.

### 5.6 Option 1 cons — token arithmetic (what Con 1 actually is)

Con 1 is not “one mixed ceremony costs 50k+50k=100k instead of two 50k ceremonies.” Two ceremonies **in the same standing session** still share one growing input context.

It is also not “triage × design = 100k from 10k×10k.” A single prompt’s cost is roughly **input tokens + output tokens**. Input is (system) + (skills files) + (living-knowledge loaded) + (tickets opened) + (conversation so far). Design material and triage material **add** inside that input.

What *does* blow up: after N turns with no reset, each new turn re-sends the accumulated history. Total cost across the session is roughly the **triangle number** of that growth. That is **quadratic in session length**. Writing the result into files and resetting makes cost additive-per-ceremony instead of quadratic-per-week.

The two-image split attacks this: ticket junk dies with KanbanMaster-cold; design talk stays with DesignArchitect-warm. It does **not** make long-lived design free if that session never writes-to-spec-and-resets.

### 5.7 Stop-rule and who gets spawned

The worker may open another **already existing** living-knowledge file only when the current ticket or an already-loaded file **points at it**.

The worker **stops** when: they are not sure how to approach the ticket; they hit a boundary living-knowledge does not cover; the next needed fact is not written; or the work looks like a design-level coupling change.

Then: comment (self-contained; address Kanban master by name if escalating), tell the human, increment friction (§8.9). Do not create a TA ticket. Do not edit the design spec.

Escalation ladder (human still drives spawns):

1. Worker ↔ human (always first).
2. If both agree it needs the board role: human spawns **KanbanMaster-cold**.
3. If that is not enough or it is design-level: Kanban master writes a **TA ticket** (problem only). Human spawns **DesignArchitect-warm** on that TA ticket only.
4. Worker-spawns-Design-architect is **forbidden** until the human separately approves that evolution.

### 5.8 Planning token budget (user estimate, not a vendor fact)

User planning assumption: on the order of **6 million tokens per week**. “Expensive” is judged against that until replaced.

Proposed escalate band (not frozen): if one ticket or design change looks like 15–20% of 6M (~0.9–1.2M tokens), ask the human first.

### 5.9 Defects of the two-image split

- Extra hop Worker → Kanban master → TA → Design architect. Worth it only if TA tickets stay self-contained.
- Two writers of living-knowledge can drift. Design-shaped changes go through the spec or a TA; Kanban master writes gotchas; if unsure, ask the human.
- `kanban.md` is generated from ticket files. Human does not edit it by hand.
- Workers do not create tickets (§8.11).
- Phase 1 exit is *sufficient spec to break tickets*, not every class named.
- Cold Kanban master ticket quality = design spec quality. Intended.
- A bad TA ticket yields a bad design if the Design architect must not read non-TA tickets.

### 5.10 Init files versus commit names

Init file: `DesignArchitect-warm.md`, `KanbanMaster-cold.md`, `Worker-cold.md`.
Commit line: `KanbanMaster-cold F-10-DB add recipe table`. Do not put the init filename in the commit line.

### 5.11 Reuse the same Worker-cold on related tickets

Use the **same** Worker-cold instance when the next ticket depends on the one just finished, or both sit in the same module (layer set may grow or shrink).

Start a **new** Worker-cold when the next ticket is a different module with no dependency on what this instance just loaded.

Still one worker at a time. Do not keep one instance for a whole 10-ticket `open` slice if later tickets are unrelated.

KanbanMaster-cold should **cluster** `backlog → open` pulls toward related tickets so this reuse is possible on purpose.
