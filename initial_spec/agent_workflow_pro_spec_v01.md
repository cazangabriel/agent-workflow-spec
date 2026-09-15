# Agent Team Workflow — Organised Specification (Rewrite of User Brief)

**Document status:** Organised rewrite of `agent_workflow_pro_draft_v01.txt`, plus resolved clarifications from the user.  
**This document is not yet the implemented agent-workflow spec.** Industry comparison, remaining design choices, and implementation stay gated.  
**Versioning:** Do not increment a document version number until the user says to. Edit this file in place.

**Source:** `/home/workdir/artifacts/agent_workflow_pro_draft_v01.txt`  
**Working file:** `/home/workdir/artifacts/agent_workflow_pro_spec_v01.md`  
**Audience / format:** The only user is a software engineer. Markdown only. Do not produce `.docx` copies.

**Date:** 2026-09-14

---

## 0. How to read and maintain this document

### 0.1 What this file is

The original user message mixed: conversation protocol, product intent, team design, knowledge architecture, ticket/git rules, and four “god-level / dogma” rules. This rewrite keeps those contents and groups them by cohesion so a later agent (or a different model) can use the file without private conversational context.

### 0.2 Binding rule for every later editor of this file (dogma)

This specification must make sense in its own universe. Do not keep key binding information only in an agent’s internal context. If a term, constraint, sequence, or detail is a component that joins other concepts together — or is itself a requirement — it belongs in this document.

Balance, not two extremes:

- Do not treat every word as sacred.
- Do not drop a requirement-shaped detail because it feels “already known” internally.

Worked warning from the brief (must stay in the spec): if the app must show butterflies, and the user later specifies they are blue, “blue” is part of the requirement. Losing “blue” in conversation, or deleting it later because “the model already knows the butterflies are blue,” is a defect.

### 0.3 Edit policy for this file and for later workflow/spec files (dogma)

Unsolicited compression is forbidden when it removes meaning, edges, or details.

Allowed:

- Inline edits that keep sections coherent.
- Merging duplicated material when all relevant scenario-bound details are preserved.
- Compression that does not lose important details.

Forbidden:

- Trigger-happy “improvements” of sentences the editor already rewrote many times, if the result only makes sense inside the editor’s private context.
- After N passes, a document whose original meaning is diluted.
- The opposite failure mode: refusing to edit existing text and only appending addenda / apply-diffs such as “v13.4.6: section 7 paragraph 3 — add the word backend.” That produces an unreadable spec.

High cohesion / low coupling applies to the spec itself:

- Topic X should not be scattered across seven unrelated places without a reason.
- A cross-cutting section (for example Security Considerations) does not require every topic section to duplicate a full security subsection. Put shared cross-cutting rules in the cross-cutting section; put topic-specific exceptions next to the topic.

### 0.4 Merge rule when two passages share a stem concept (dogma)

If one place has concept `A-B-x-y` and another has `A-B-t-w` (where `x,y` and `t,w` are details bound to `A-B` in different contexts):

- Do not merge `A-B` by keeping only `x-y` or only `t-w`.
- If both detail sets are relevant in different scenarios, the merged section must carry `A-B-x-y-t-w` (or an equivalent complete encoding of both scenarios).
- If `y` is the same as `t`, the merge may look like `A-B-x-y-w`. Use propositional logic; preserve relevant details.

### 0.5 Uncertainty rule for every agent that touches this work (dogma)

Unknowns and uncertainty are the enemies of correct estimation. Hidden unknowns make the human’s estimates wrong and later look like the agent “lied by being certain.”

Required behaviour:

- State uncertain or unknown variables explicitly.
- “I don’t know; I would suggest A, B, or C” is preferred to a confident answer that silently contains variables that could drop confidence (example given in the brief: claimed 100% when worst case is closer to 75%).
- When not sure, raise it with the user.
- Do not replace unknowns with assumptions and then implement against those assumptions.

This rule is first-class product behaviour, not style advice. Fake “yes-man” certainty on a critical personal project is treated as a reason to change model vendor.

### 0.6 Skills assumed of the editor of this rewrite

The brief assigned the rewriting agent these skills and required they be used:

- Up-to-date expert knowledge of AI agents (research, proven practice, community advances).
- Mathematics for precision of thought.
- Expert propositional logic, used to avoid inconsistencies and to make sound decisions — including the merge rule in §0.4. Logic is for internal consistency, not for dumping logic-jargon on the user.
- Expert software engineering (the domain being built for).
- Expert software architecture, used when later designing the workflow architecture.
- Exceptional abstract thinking.
- Expert project management, line management, and product management (real-life workflow failure modes).
- Expert specification drafting.

### 0.7 Questions to the human must carry their own context

An agent asking the human a question must make that question understandable **from the question itself**, without relying on the agent’s private memory of an earlier short label.

Required contents of a question:

- What is being asked, in full — not a two-word reminder of a previous thought.
- Why the answer matters (which decision or spec section it feeds).
- The options, if there are options, and what would change for each.
- Any quote or paraphrase of the source phrase being disambiguated.

Forbidden: “clarify X” when X is only a heading for a longer idea the human cannot see. That is the same class of defect as hiding binding information in internal context (§0.2).

### 0.8 Catching semantic defects without waiting for the human

The non-technical-manager hole was a defect in **this spec’s own language**. The brief used “manager” as a technical planner. The user’s later sentence used “manager” as the non-technical human-org stereotype. Both meanings sat in the document under one word. That is a Reading B failure applied to our roles, which §2.5 was supposed to prevent.

An editor of this spec must run the following pass after any material change, not only when the user points at a hole:

1. **Role × artifact matrix.** For every artifact that can be written (living-knowledge file, ticket body, ticket filename, commit, code), list who is allowed to write it and which §6.6 / worker skills that writer is required to have. If the writer lacks the skills the artifact needs, that assignment is a defect.
2. **Stereotype inversion.** For every job title, ask: “if this word means what it means in a typical company, does the assignment still hold?” If the brief-meaning and the street-meaning disagree, pick one meaning and write it down. Do not leave the word overloaded.
3. **Two-reader test.** Read each permission as if a later agent has only this file. Would they staff a non-technical writer onto a technical artifact?
4. **State-vocabulary test.** Every board column, filename token, and sentence that says “closed” / “done” / “open” must map to one state list. Two lists for the same board is a defect.
5. **Stop-rule test.** Whenever a worker is told both “load more context and continue” and “raise unknowns to the human,” write the stop condition that chooses between those two.
6. **Cost-relative test.** Words like cheap / expensive / too many tokens are meaningless without the planning budget they are relative to. If the spec uses those words, it must point at the budget assumption (§5.8).

This pass does not guarantee the editor will catch every hole. It makes the class of hole that slipped through (role word vs artifact skill) a checklist item rather than an accident.

---



## 1. Conversation protocol (gated; do not skip gates)

Work proceeds in this order. Do not collapse gates.

### Gate 1 — Understand, then rewrite the user brief

1. Read the user prompt (this source brief) and apply its rules and skills.
2. Rewrite that prompt into an organised specification with proper sections.
3. Preserve logic and relevant spec details. Do not lose them for tidiness.
4. Ask questions rather than inventing wild assumptions. The user prefers time spent on clarification over a high-quality-looking document that means something else.
5. This rewrite is **not** yet the design of the agent interaction workflow.

**Current position:** Gate 1 deliverable exists and is being refined in place. Topology is now Option 1 architect-primary (role name TechnicalSupervisor), two staffing phases, no non-technical manager agent. Gate 3 (industry comparison as a separate exercise) has not been run as a full survey; some industry patterns have already been used in discussion. Implementation is still Gate 4.

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

---

## 2. Purpose and problem

### 2.1 Purpose

Design the workflow for a team of agents running via Grok Build.

The team’s job is to build a software development project specified by the human user. Example project types (not an exhaustive closed list): public-facing website with a CMS; mobile store app; mobile game; Unity game; other software the user specifies.

### 2.2 What “optimal” means here

Design an optimal team workflow based on proven methods that give good or outstanding results in real-life human teams — then adapt those methods to agents, token cost, and context windows.

### 2.3 Suggested targets, and what is now decided

Original brief treated these as starting targets, not hard constraints. User decisions since then:

- **Decided:** one overview agent class, architect-primary, with thinner planning/ticket-cutting duties attached. Role name in this spec: **TechnicalSupervisor**. No separate non-technical manager agent. No separate technical ticket-cutter.
- That role takes requirements from the human, turns them into features with planning, cuts tickets, owns living-knowledge, and facilitates communication with the worker (reads ticket comments; does not wait for the worker to design the channel).
- Knowledge that must survive a session lives in organised `.md` files on git alongside the project (living-knowledge).
- Worker agents are transient: spawned for one or a few tasks / features, so their context windows stay short and they stay cheap in prompt-token cost.
- **Decided:** one worker at a time for now. Do not run parallel workers until the user reopens that constraint.
- **Decided:** human-approve-then-test loop is required on every ticket. The human may skip actually performing a test as a gut call, but the loop itself stays in the workflow. Reason: the user does not consider current AI reliable for unsupervised work; testing is how the user supervises. Agent-driven testing against requirements is **not viable yet** because it would consume scarce tokens.
- **Decided:** two staffing phases for the TechnicalSupervisor (§5.5). Phase 1 central and long-lived. Phase 2 event-driven plus mandatory board ceremonies. Same role, different presence.
- **Decided:** workers do not write living-knowledge. They write ticket comments and raise unknowns to the human.

### 2.4 Shared vocabulary between agents (original phrase: “language barriers”)

Original brief: “We must take into account potential language barriers between agents with different skillsets.”

That phrase is ambiguous. The two readings, and why the question was asked:

- **Reading A — natural language:** agents (or agent vs human) mixing English with another spoken/written language, or mixing dialects, so the same requirement is not stably worded.
- **Reading B — specialist vocabulary:** a worker loaded with DB skills and a worker (or manager) loaded with UX skills use different words for the same thing, or the same word for different things, or different precision about the same feature. Tickets and living-knowledge then fail to bind.

The surrounding sentence named **skillsets**, not human national languages, so Reading B is the stronger parse of the original brief. Reading A can still happen and is cheap to forbid.

**Decided package:** A + B + DDD extra (§2.5). Rules below are the operational form of that package.

1. All tickets, living-knowledge files, commit messages, and agent-to-human questions are written in one project language (the language of this spec: English), unless the user sets another project language in living-knowledge.
2. Layer jargon is allowed **inside** a layer’s detailed living-knowledge file.
3. Anything that crosses agents or roles — ticket titles, ticket type abbreviations, shared terms for modules and layers, status words in filenames — must use the shared vocabulary recorded in living-knowledge and in this spec. Do not invent a second name for an existing shared term.
4. If two skillsets would naturally use different words for the same concept, the first writer picks one term, writes a one-line alias if needed, and later agents reuse that term. Do not “translate” between agent jargons in flight.
5. Precision differences are not resolved by chat. If a ticket is too vague for a layer specialist to implement without guessing, the worker raises the unknown to the human instead of filling it with assumptions (§0.5).

### 2.5 Ubiquitous Language (decided: A + B + DDD extra)

The professional name for most of Reading B is **Ubiquitous Language** (domain-driven design): one language shared by the people (and here, agents) who talk about the domain, used in speech, tickets, living-knowledge, and code, with no private translation layer between “business words” and “implementation words.”

Overlap with A / B / A+B:

- **A (natural language)** is only a precondition. Ubiquitous Language can be English, Romanian, or anything else; it does not choose the vehicle language. Rule 1 in §2.4 (one project natural language) sits *under* UL, not inside it.
- **B (specialist vocabulary clash)** is what UL is for. DB, UX, domain, and management must not keep private synonyms for the same concept inside one bounded context.
- **A+B** is therefore: pick one vehicle language (A) + enforce UL inside each bounded context (B).

DDD nuance that must not be lost: **different bounded contexts may have different ubiquitous languages.** That is allowed. What is not allowed is an implicit, unwritten translation. If two contexts use the same English word for different things, living-knowledge at the boundary records the map. Layer specialists (DB vs UX) are **not** automatically different bounded contexts; often they are implementation dialects of one context and should collapse onto one term list.

Living-context is where the UL is written down so a later cold agent can speak it. Tickets and filenames must use that language, not a second dialect.

**Decided:** adopt the package **A + B + DDD extra**, not A alone, not B alone, not A+B with a single global glossary.

- A alone: everyone writes English and still says “order” three ways. Useless against the original skillset-barrier sentence.
- B alone: shared terms, but files may switch vehicle language mid-project. Cheap to prevent; not the core problem.
- A+B with one glossary for the whole app: smashes distinct bounded contexts into false unity, or produces an unusable mega-glossary.
- DDD extra without B: boxes on a diagram and informal talk inside each box.

Default vehicle language is English unless the user sets another in living-knowledge. Layer implementation dialect (SQL-ish vs CSS-ish) may appear inside a *detail* file when it names a technical mechanism; domain concepts in that same file still use the context’s UL. Status words in ticket filenames stay in the shared project vocabulary.

Status: **decided** by the user.

---

## 3. Scenarios to avoid (anti-patterns)

### 3.1 The expensive god-agent

Do not have one god agent that knows every project detail, with a huge and growing context that is extremely expensive to prompt.

Concrete failure from the user’s history: an agent that consumed about 100k tokens per prompt. That burns through Super Grok subscription credits quickly under Grok Build.

This is given as reductio ad absurdum. The usable rule: do not use expensive agents with massive context as the default working unit.

### 3.2 Too many specialists and a dense communication graph

Many specialised agents create many communication channels. That communication costs tokens for input, output, and logs.

Worst case: the communication graph among agents resembles a connected graph (every agent talks to every other). Avoid that.

Mitigations stated in the brief:

- Keep the number of agents limited so channel count stays organically small.
- Agents must not be “super chatty.” Stay on project / feature / workflow details. Not dinner conversation.
- Avoid endless discussion threads. If a thread tends to become long, an agent must be designated to escalate that thread to the user so the user decides what to do.
- Watch for loops in inter-agent discussion and break them by escalation rather than more talk.

---

## 4. Ideals that apply to the workflow and to every agent in it

### 4.1 High cohesion and low coupling

This principle must permeate:

- This specification’s structure.
- The later workflow design.
- The agents’ own work (tickets, code, living-knowledge files, communication).

### 4.2 Git competence is not assumed; it is trained

If an agent needs to work with git, that agent must specifically be trained with git skills.

Rules:

- One feature per commit.
- Do not mix unrelated changes.

### 4.3 Commit and push are not optional extras

If an agent works a ticket and is expected to commit and push, it is paramount that the agent does not forget.

Suggested control: every spawned worker gets a small session checklist that includes commit + push.

Why this exists (keep the failure mode in the spec): the agent reports a ticket implemented; the human expects to see it in the app; the change exists only in the agent’s local workspace and was never pushed; the human cannot pull and test. That situation is treated as a serious workflow defect.

### 4.4 Human-approved approach before implementation

Whenever an agent picks up a new task / ticket / feature:

1. Discussion first: agent proposes approach; human approves or changes it.
2. Only after the human approves the approach may implementation start.
3. Implementation follows the agreed approach.
4. Agent commits and pushes (direct commit(s) to `dev`, or a feature branch merged into `dev` when appropriate).
5. Human pulls latest `dev`, starts containers etc. as needed, and is asked to test in the app. The human may decide not to exercise a given ticket, but the workflow still presents the test step. This step exists because the user treats testing as supervision; current AI is not trusted to work unsupervised. Do not add an agent that tests against requirements as a substitute: that path was considered and rejected as too expensive in tokens.
6. If the change works, the ticket is marked `done` per §8.4. `closed` waits for the TechnicalSupervisor ceremony in §8.8. Otherwise work continues until it works and issues are fixed.

Aim of approaches: simplest approaches first, that still lean toward maintainable professional code. No quick-and-dirty code, no hacks, no bypasses, no lazy code. Code must suit current project size and remain maintainable.

### 4.5 Professional maintainable code

Restated because it is both an implementation standard and an approach-selection standard: professional, maintainable code for the current project size; not hacks.

### 4.6 Skills are per artifact, not per job title

Whoever writes artifact X must have the skills X needs. This is the operational form of §0.8 item 1. Skills are loaded from that agent type’s init file (§6.1, §5.10). Missing skill + write permission = defect.

| Artifact | Writer | Required skills (minimum) |
|---|---|---|
| Living-knowledge (gotchas, conventions, module body) | Kanban master; Design architect may also edit | Full §6.6 set + stack literacy for this project |
| Project design spec (`project-design-spec*.md`) | **Only** Design architect + human | §6.6 + architectural characteristics work (§5.5.1) + stack choice + component-level order |
| Kanban board file | Kanban master and worker (restricted moves) | Board vocabulary §8.4; filename+board atomic update §8.10 |
| New `backlog` ticket | **Only** Kanban master | Ticket-cutting, UL, dependency order, read design spec, must not invent structure the design spec does not support |
| New worker-spawned `open` ticket | Worker, only under §8.11 | Same as implementing that work; must be sure of the approach; must not shrink a design-level coupling problem into a cute ticket |
| TA ticket | **Only** Kanban master | Write a self-contained design *problem*; must not propose the design solution |
| Ticket comment | Worker on the ticket they are executing; Kanban master on any non-TA; Design architect on TA only | Clear, concise, self-contained; no novels |
| Application code in an area | Worker (after human-approved approach) | Software engineering + domain + that area’s living-knowledge + OOP/OOD/SOLID + pitfalls/anti-patterns + readable names (§7.1) + any transient extra skills noted on the ticket |
| Git commit / push | The agent who changed the tree | Git skill; one feature per commit; message = agent name + ticket code + human title (§9.2) |
| Containers / local run | Worker when the ticket needs it; human for test | Container skill for that stack |
| Deploy / host on a machine | Only an agent whose ticket says so, and only with deploy/host skills loaded | Those skills + the relevant living-knowledge; do not improvise production |

Do not give an agent write access “because they are senior.” Give it because the row above lists the skill and the permission.

---

## 5. Team model

### 5.1 Skill family and two images (working names)

**TechnicalSupervisor** is now a **shared skill family**, not one standing body. Same high-expertise set (§6.6 + stack + ticket literacy). Two *images* with different lifetime and write rights — ISP applied to sessions, because one session cannot be both “reset every board” and “warm on design.”

Working names (provisional; rename later if we find better UL):

| Working name | Lifetime | Job |
|---|---|---|
| **Design architect** | Long-lived | Project design spec, architectural characteristics, TA tickets, design-level living-knowledge when a TA requires it |
| **Kanban master** | Transient; fresh instance per ceremony | Board, non-TA tickets, living-knowledge that is not the design spec, worker facilitation, TA *creation* (problem only) |
| **Worker** | Transient per ticket | Implementation after human-approved approach |

There is still no non-technical manager and no third ticket-cutter. Architect-first: when structure and board speed conflict, structure wins or the human is asked. Do not shrink a design-level coupling problem into a cute worker ticket.

Init files (this is what “init file vs commit name” meant):

- `DesignArchitect.md` — skills + permissions for that image
- `KanbanMaster.md` — skills + permissions for that image
- `WorkerAgentInit.md` — worker skills including §7.1

Commit messages still use the **image name** + ticket code + human title, not the init filename.

**Worker**

- Software development, propositional logic (internal), domain of this app.
- OOP, OOD, SOLID, temporal coupling, common pitfalls and anti-patterns. Code must follow those; names must be readable (no `x` for a business idea). The human reads the code.
- Bite-sized living-knowledge: ticket layers + pointed-at cross-cuts, until the stop-rule (§5.7).
- May not write living-knowledge or the design spec. May not write TA tickets.
- May comment on the non-TA ticket they are executing (even if they did not create it).
- May create a new non-TA ticket directly in `open` only under §8.11.
- May not edit fields of a ticket they did not create, other than comments and allowed state moves on the ticket they are executing.
- Stop-rule: tell the human; comment addressed to the Kanban master if both agree to escalate.

**Design architect**

- Long-lived so design discussion can stay warm.
- Reads/writes `project-design-spec*.md` and TA tickets.
- May read/write living-knowledge when a design change requires it.
- Must **not** read the kanban board file, and must **not** read non-TA tickets, unless a TA ticket is so incomplete that there is no other way — that is a Kanban-master defect, not a habit.
- Must not pick up high-frequency ticket chatter. That is the point of the split.

**Kanban master**

- Spawned fresh each ceremony so ticket/comment junk dies.
- Reads design spec (read-only). Does not edit design spec.
- Reads/writes kanban board, non-TA tickets, living-knowledge (gotchas, conventions, module bodies — not the design spec).
- Creates TA tickets that state the problem, impact, needs. Does **not** put the design solution in the TA ticket.
- Looks at `in-progress`, `blocked`, and other columns during a ceremony, not only `backlog`/`open`/`done`/`closed`.
- For blockers: if they can see solution option(s), present them to the human; if not, or a decision is required, escalate to the human; if the gap is design-level, write a TA ticket.

This split is **candidate-accepted for spec writing**, with defects recorded in §5.9. Not a claim that ISP on agents is free.

### 5.2 Cost model

Design conversation stays in the long-lived Design architect + the design spec on git. Board/ticket chatter dies with each Kanban master instance. Worker chatter dies with each worker. Mixing those three histories in one never-reset session is the 100k-token failure mode. §5.6 and §5.9.

### 5.3 Concurrency (decided)

One worker at a time for now. `open` cap (formerly called `open`) is a sprint-size buffer, proposed 10, so nine tickets may sit idle on purpose.

Serialisation: Kanban master fills `open` from design spec + `backlog` → human + worker pick one `open` ticket → approach approval → implement → human test → `done` → next ceremony `done → closed`.

### 5.4 Topology decision

**Decided:** no non-technical manager. Architect-first. Shared TechnicalSupervisor *skill family*.

**Working topology:** two images of that family (Design architect long-lived, Kanban master transient) + worker. Defects of that split are in §5.9. Names are provisional.

Holding constraints still in force: no god-agent; small channel count; restore from init files + git; workers transient; living-knowledge and design spec are files, not session memory.

### 5.5 Phase 1 and Phase 2 — definitions (lock if the user agrees)

These are staffing + artifact definitions, not a product waterfall. Implementation still happens as tickets. Phase 1 is not “code the whole system on paper.”

**Phase 1 — design spec accepted.**  
The Design architect works with the human until `project-design-spec*.md` is good enough to drive ticket breakdown. That spec includes: bounded contexts and UL starter; **3–4 priority architectural characteristics** for this project (§5.5.1); module/layer map; chosen technologies and how they will be used; component-level implementation order (dependencies), **not** a ticket list. Ticket breakdown is Kanban master work and is not Phase 1’s definition of done.

**Phase 2 — ticketed delivery against that spec.**  
Kanban master runs ceremonies, breaks the accepted spec into tickets, maintains living-knowledge (non-spec), watches the board including `in-progress` and `blocked`. Design architect stays long-lived but is invoked through TA tickets and spec edits, not through reading the board. Workers implement `open` tickets. Ceremonies cannot be skipped: without them there is no legal source of new `backlog` tickets.

**Mini Phase 1.** A new bounded context or a change that rewrites the spec’s skeleton reopens Phase 1 presence for that slice. Human declares phase changes.

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
7. Living-knowledge *skeletons* exist for the mapped modules (cards + empty-or-thin bodies are allowed; full gotchas come from delivery).
8. Human answers the explicit question: “Do you accept Phase 1 as done for this spec version?”

A first thin slice through the test loop is **useful** but is Phase 2 evidence, not a hidden extra Phase 1 gate, unless the human adds it.

### 5.6 Option 1 cons — token arithmetic (what Con 1 actually is)

Con 1 is not “one mixed ceremony costs 50k+50k=100k instead of two 50k ceremonies.” Two ceremonies **in the same standing session** still share one growing input context.

It is also not “triage × design = 100k from 10k×10k.” A single prompt’s cost is roughly **input tokens + output tokens**. Input is (system) + (skills files) + (living-knowledge loaded) + (tickets opened) + (conversation so far). Design material and triage material **add** inside that input. They do not multiply as 10k×10k in one shot.

What *does* blow up:

- After N turns with no reset, each new turn re-sends the accumulated history. Total cost across the session is roughly the **triangle number** of that growth: turn 1 sends H, turn 2 sends H+Δ, … turn N sends ~NΔ. That is **quadratic in session length**, not linear in “number of concerns.”
- If both triage chatter and design chatter stay in that history, H grows faster. Two concerns in one uncompacted session therefore cost more *over a week* than two short sessions that each load only the files they need and then die.
- Writing the result into living-knowledge and resetting is what makes the cost additive-per-ceremony instead of quadratic-per-week.

So: separate ceremonies help **only if** the session does not keep both histories warm. Files are the memory. The 30M-token recipes-site stretch is the quadratic pattern, not proof that architecture and triage are each 15M.

### 5.7 Stop-rule and who gets spawned

**Stop-rule (the condition that was missing before, now explicit):**

The worker may open another **already existing** living-knowledge file only when the current ticket or an already-loaded file **points at it** (Relies-on / dependency / “see module card”).

The worker **stops** — no more inventing, no more “I will just assume” — when any of these is true:

- they are not sure how to approach the ticket;
- they hit a boundary, unknown, or unexpected chunk that living-knowledge does not clearly cover;
- the next fact they need is not written;
- the work looks like a design-level coupling / structure change.

Then: comment on the ticket (self-contained; if escalating, address the Kanban master by name in the comment), tell the human, increment friction (§8.9). Do not create a TA ticket. Do not edit the design spec.

Escalation ladder (human still drives spawns):

1. Worker ↔ human (always first).
2. If both agree it needs the board role: human spawns **Kanban master**; worker’s comment is waiting.
3. If the Kanban master cannot solve it, or it is design-level: Kanban master writes a **TA ticket** (problem only). Human spawns **Design architect** on that TA ticket only.
4. Worker-spawns-Design-architect is **forbidden** until the human separately approves that evolution. Reliability signal for that future decision: friction counters + fail comments on closed tickets, aggregated by the Kanban master. No automatic unlock.

**Kanban master spawn triggers:** ceremony time; human asks; worker comment addressed to them; board is dry (`backlog`+`open` emptying); blocker review.

**Design architect spawn / attention triggers:** Phase 1 work; a TA ticket in `open`; human asks a structural question; mini Phase 1.

### 5.8 Planning token budget (user estimate, not a vendor fact)

Grok does not publish SuperGrok weekly token quotas in this spec’s sources. The following is the **user’s planning assumption**, recorded so “cheap / expensive” has a denominator:

- User observed ~30M+ tokens over ~2 weeks on a Grok Build recipes site (the failure mode this workflow is trying not to repeat).
- User’s working budget for *this* kind of work: on the order of **6 million tokens per week**, leaving headroom inside a guessed SuperGrok week, with leftover quota usable if it exists.
- That number is an estimate. It may be wrong. It is still the figure “expensive” is judged against until the user replaces it.

Relative language:

- A ticket or architecture change is **expensive** if the TechnicalSupervisor believes it would consume a large fraction of the weekly planning budget (proposed escalate threshold: on the order of 15–20% of 6M, i.e. roughly 0.9–1.2M tokens — proposal, not measured). Then escalate to the human with the estimate and the uncertainty.
- “Cheap ticket” means small relative to that budget, not “free.”

What “15–20% escalate band” meant (proposal only, **not frozen**): if an agent believes a single ticket or design change would burn on the order of 15–20% of the 6M weekly planning assumption (roughly 0.9–1.2M tokens), they must ask the human before doing it, with the estimate and the uncertainty. It is a “this might eat the week” tripwire, not a measured law. Ignore or replace the percentage; keep the idea that expensive is relative to §5.8.

### 5.9 Defects and weak points of the two-image split

This split **does** attack Con 1 / quadratic cost: ticket junk no longer has to live in the same session as design talk. The Design architect’s context grows only with design turns + spec text. The Kanban master starts empty each ceremony.

It does **not** make long-lived design free. Design conversation still grows quadratically inside the Design architect session if that session never writes-to-spec-and-resets. “Only does design” caps the *kind* of junk, not the *amount* of design talk.

Other weak points (do not lose these):

- New channel: Kanban master → TA ticket → Design architect. That is an extra hop. The hop is justified only if TA tickets stay self-contained.
- Two writers of living-knowledge (Kanban master primary, Design architect “at will”) can drift UL or fight. Rule: design-shaped changes go through the spec or a TA; Kanban master writes gotchas/conventions; if unsure which file, escalate to human rather than double-edit.
- Dual board representation (kanban.md + ticket filenames) can drift. §8.10.
- Worker-created `open` tickets bypass the architecture gate. Constrained by §8.11. Still a hole if the worker is wrong about “I know how.”
- “Finished design spec then tickets” can slide into waterfall over-design. Phase 1 exit is *sufficient spec to break tickets*, not *every class named*. Spec remains editable via TA in Phase 2.
- Cold Kanban master ticket quality = design spec quality. That is intended. If the spec is vague, tickets will be vague; fix the spec, do not make the Kanban master long-lived “to remember what we meant.”
- Design architect forbidden to read non-TA tickets means a bad TA ticket yields a bad design. That is a feature if we enforce self-contained TA writing; a defect if we do not.

### 5.10 Init files versus commit names

An **init file** is the skills-and-permissions document an image reads at spawn (`DesignArchitect.md`, `KanbanMaster.md`, `WorkerAgentInit.md`).

A **commit agent name** is the short name in the git message (`DesignArchitect`, `KanbanMaster`, `Worker`). Same image, two strings. Do not put `WorkerAgentInit.md` in the commit line.

---

## 6. Knowledge model

Agents have two distinct, first-class knowledge types:

1. **Skills** — what they know how to do, and how well.
2. **Problem context** — how many things they know about a problem or feature before they start.

Ideal: reduce unknowns and uncertainty toward zero before implementation. Do not implement against assumptions.

### 6.1 Skill loading

When an agent is spawned, it learns base skills from a `.md` file dedicated to that agent type. Example from the brief: “you are a software engineer agent — go and read `SoftwareEngineerAgent.md` to find out about your skills.”

After base skills:

- The agent is pointed at the next ticket, or
- The agent offers a short list of potential next tickets that were already ordered / prioritised, so the human can choose.

### 6.2 Problem-context loading (living-knowledge)

App knowledge lives in a series of markdown files under the repo-relative path `/app-name-1/living-knowledge/` (formerly drafted as `/app-name-1/living-context/`).

The **project design spec** is a distinct artifact next to that tree (proposal: `/app-name-1/living-knowledge/project-design-spec.md`, or a small set `project-design-spec-*.md`). It is not a ticket and it is not a module gotcha file. Only the Design architect and the human write it. The Kanban master reads it to cut tickets.

`app-name-1` is a stand-in for the actual application / repo project directory name. Each software project the team builds gets its own such tree. Paths in this spec that start with `/app-name-1/` are repo-relative, not host-absolute.

Properties required of this store:

- Files are pushed to git as soon as they are modified.
- Workers do not write these files. Kanban master writes module living-knowledge. Design architect writes the design spec and may edit living-knowledge when a TA/design change requires it. Human may edit either.
- Files must be well differentiated and encapsulated, with high cohesion and low coupling among themselves.
- Avoid “microservice hell”: not 30 `.md` files that refer to each other until the dependency graph is a spider web or a connected graph.
- **Source of truth:** living-knowledge files (plus ticket files and this spec), not an agent’s session memory. Session memory is a cache. If a decision matters after the session ends, it is written to git-backed markdown. Quality of overview files is a first-class requirement, because later agents — including a reset long-lived agent or a transient architect — are only as good as those files.
- Resulting context must be:
  - **AI-model-account independent:** any AI model can spawn the intended agent types; agents pick up work because skills and required app-context are in files, not in a vendor-specific memory blob.
  - **Machine independent:** commit all context files to git so work restores on another machine if the current dev machine is unavailable or changed.

### 6.3 File types inside living-knowledge

Two granularities per module+layer.

**A. General overview files, one per module+layer**

Example given: 2 modules × 3 layers each → 6 overview files.

Audience: architect or manager (product / line / project) who needs a good overview of that area. That audience also defines the level of detail: overview, not expert dump.

Useful to the manager-architect so it can understand each area without swallowing every expert file.

**B. Detailed module files, one per module+layer**

Loaded by workers when:

- a ticket touches that area, or
- a cross-cutting concern appears with another module+layer than the ticket originally named.

A brand-new initialised worker with initialised skills should gain a good feeling of the area without relying on assumptions.

Content that is worth storing in the **body** of a living-knowledge module file (keep this list; it is the content policy):

- General technical decisions for that module+layer.
- Conventions.
- Integrations with other layers and/or modules and/or APIs.
- Principles used across the module.
- Design / architectural priorities that apply here (including the project’s chosen “-ilities,” e.g. maintainability).
- Preferences for which principles to use or prioritise in code.
- Gotchas, warnings, dangers, module-specific pitfalls.
- What to avoid, and what to do instead.
- Technologies or usage styles to prefer, and which to avoid (work *with* the stack).
- Things that need special attention.
- Traps; temptations to resist, and why.
- Preferred ways of working.
- Preferred directions for the code.

Content that is not wanted by default:

- Trivial member lists.
- Blind method documentation in the style of javadocs.
- A copy-of-the-code-in-written-words.
- Exception: if a method is key or tricky, it may be mentioned.

Size constraint: detail files must not be so large that a brand-new worker loading 3 or 4 of them becomes a “context beast” costing about 30k tokens per prompt.

Keeping files focused is the human user’s and the manager-architect’s job — mainly the agent’s job, following discussions with the user.

If the manager-architect notices something wrong, or can add real value by improving any of these files or any `.md` files in general, it should pro-actively escalate that to the human at any opportunity.

### 6.4 Transient extra skills and extra app-context during a ticket

If the ticket implies work on a certain layer, the worker reads the knowledge / project-context file for that area.

If additional transient skills would be useful for the ticket, the worker adds those skills to its knowledge for that session / ticket only, and leaves a comment on the ticket naming the extra skills used.

Manager-architect periodically reads finished tickets. If workers consistently need certain skills for tickets in a certain area, consider adding those skills as defaults to the area-context markdown file, in the form: “if you are going to work on this area, acquire additional skills A, B, C.”

### 6.5 Cross-cutting discovery mid-ticket (keep the full scenario)

A worker may discover that the ticket has a cross-cutting concern with another app layer or with another module’s specific layer. Then the worker must research that other layer’s app-context file, or that other module’s specific layer file.

Scenario given in the brief (do not drop parts):

- Project has 3 large modules.
- Each module has: DB, Model, business-logic services, 3rd-party API integration services, local API controllers, and UX layers.
- Worker starts a Module 1 ticket that touches Model and business-logic layers, so it acquires app-context for those 2 layers.
- During work it discovers further changes needed in Module 2 in: 3rd-party API layer, Model layer, and local API controllers. Those Module 2 changes were not foreseen in the ticket.
- Worker must learn app-context for those Module 2 layers so it can do the cross-work integration with complete required context, without assumptions or cutting corners, and without needing to know everything about all parts of the app.
- Worker leaves comments on the ticket detailing extra work discovered and extra app-context acquired.
- Manager-architect observes and may conclude: something is wrong; there is tight coupling; or this is expected.

If the worker discovers dependencies such as the need to use an API:

- Familiarise with that API.
- If that API belongs to another module layer, familiarise with that layer for the duration of the ticket.

After the ticket, the worker session ends; the extra context is not kept as a standing whole-app brain.

### 6.6 Skills required to *write* living-knowledge

Reading a living-knowledge file and writing one are different jobs. The brief already said manager-architect and the human create/modify these files. Workers may propose patches via ticket comments; they are not the default authors.

**Decided** by the user, including the should-haves. The author must have **software-development** skills (that is the work) and **domain knowledge of the app’s domain** so the agent is fluent in that domain’s UL. The rest of the must/should list below is also in force.

**Must have**

1. **Software development** — enough working skill to describe real structures, not tourist architecture. Living-context is about a software system; an author who cannot read or judge code will write fiction.
2. **Domain knowledge of this app’s subject** — fluency in the problem UL (what the product *is*), not only in framework names. Without this, “Ubiquitous Language discipline” is an empty procedure.
3. **Propositional-logic discipline** — used internally: detect contradiction, keep `A-B-x-y` and `A-B-t-w` when merging, refuse silent drop of scenario-bound detail. Not used to dump jargon into the files.
4. **Uncertainty honesty (§0.5)** — write “unknown / undecided / depends on X” instead of filling holes so the file looks complete.
5. **Specification drafting** — sentences that a later agent with zero session memory can execute against.
6. **Ubiquitous Language discipline (§2.5)** — one term per concept inside a bounded context; aliases recorded; no private dialect. Includes the skill of telling **domain terms** apart from **mechanism terms** (Customer vs a particular table).
7. **Software architecture (decision writing, not code copying)** — record decisions, constraints, integrations, gotchas, rejected temptations and why. Not a prose mirror of the codebase.
8. **Cohesion / coupling as a writing skill** — one concern per file; no spider-web of cross-references; no 30-file microservice-hell.
9. **Audience split** — overview files stay at manager/architect altitude; detail files stay loadable 3–4 at a time without a ~30k-token prompt.
10. **Edit policy (§0.3)** — compress without deleting requirements; edit inline; no append-only diff addenda.

**Should have**

11. **Requirements / problem-framing** — tell a domain fact from a solution choice, so a UI sketch does not get written as if it were the domain.
12. **Contract / integration thinking** — how modules and layers talk (payloads, ownership, failure), without turning the overview into an API dump.
13. **Project/line management sense** — what a later worker needs in order not to guess; what can wait.
14. **Information architecture of the markdown corpus itself** — when to split a file, when not to; prevent living-knowledge from becoming the spider web §6.2 forbids.
15. **Stack literacy for the actual stack** — enough of the chosen framework/runtime conventions that the files do not invent a second parallel framework.
16. **Git skill** — living-knowledge is pushed as soon as it changes.

**Optional, when the project actually has that risk**

17. Security, privacy, or operational constraints as *decision writing* — only if the app has those concerns. Do not load a generic security essay into every project.

**Must not treat as a living-knowledge skill**

- Javadoc-style member lists.
- “Document every method.”
- Unsolicited rewrite of already-stable terms.
- Loading every implementation skill in the universe “because software.” Broader ≠ god-agent. Transient extra skills still belong on the ticket, not in the standing author profile.

Only the TechnicalSupervisor and the human write living-knowledge. Both need this set. Workers do not write these files. Tickets the TechnicalSupervisor cuts must still use UL, or tickets become a second language.

### 6.7 Living-knowledge name, splits, and module “interfaces”

**Name.** The store is **living-knowledge** (folder proposal: `/app-name-1/living-knowledge/`). Earlier drafts said “living-context.” Same artifact. Use living-knowledge in new writing so the name matches “this is what an agent must know,” not “a chat context window.”

**Splits.** A file that is correct and concise can still be too large for a transient worker to load. Size is a first-class reason to split, alongside cohesion.

- Split **vertically** when two bounded contexts or two product modules do not need each other’s depth (Recipes vs Accounts).
- Split **horizontally** when one module’s layers have different audiences (overview vs DB-detail vs UX-detail) — this is the existing overview/detail cut in §6.3.
- Split **both** when a vertical module’s detail file alone still blows the “3–4 files ≈ 30k-token beast” budget.
- Do not split into microservice-hell (30 tiny files that all point at each other). A split must reduce what a typical ticket loads, not increase hops.

**Pointers.** Files may point at each other. Pointers are coupling. Prefer few, stable pointers over mood-of-the-day prose.

**Module card — what this word means.**

A module card is **not** a third kind of document and **not** a replacement for living-knowledge. It is a **fixed header block at the top of each living-knowledge `.md` file**. The rest of that same file is the real content (§6.3): gotchas, principles, pitfalls, avoid/instead, stack preferences, “-ilities” that apply here, etc. Those bodies are not three fields long.

The card exists so a cold worker can see, without parsing the author’s mood that day: what this file is for, what it points at, and what it refuses to answer (stop-rule).

User agreed the card shape. Mandatory short header:

```text
## Module card
- Id: <stable-id>
- Audience: overview | detail
- Provides: <3–7 bullets a worker can trust to find in this file>
- Relies-on: <other-id> — <one line: why you would open it>
- Does-not-cover: <explicit out-of-scope>
```

No method signatures, return types, or parameter lists. If the card grows longer than useful, cut the card, not the body.

---

## 7. Mandatory worker initialisation and ticket loop

This sequence is mandatory. It must appear as a list of steps every worker always completes.

**Step 1 — Init.**  
The human initialises a new worker by pointing a non-initialised agent at a generic file such as `WorkerAgentInit.md`. That file instructs the worker to acquire generic skills needed:

- software engineering expertise, plus
- general expertise in the business domain of that project, plus
- anything technical or topical that is specific for that project,

but **not** yet all expertise in all project-layer areas (not yet UX, DB, SQL, etc. as a full set).

Also load §7.1 (OOP/OOD/SOLID, pitfalls, readable names). If the ticket needs git, containers, or deploy, load those skills before touching those artifacts (§4.6).

**Step 2 — Show the board, then propose.**  
The worker’s first visible act is to show the human the current `/app-name-1/kanban.md` board. Then the worker may suggest 4–5 next tasks from `open` (and mention `blocked` if relevant). The Kanban master built that list in the last ceremony.

**Step 3 — Human picks.**  
The human picks 1 ticket from those 4–5 proposals.

**Step 4 — Approach discussion (mandatory).**  
Worker proposes one or more approaches. Prefer simplest approaches first that still produce maintainable professional code (see §4.4–§4.5). Human chooses an approach as-is or alters it.

**Step 5 — Implement.**  
Only after agreement does the worker implement as discussed.

**Step 6 — Commit and push.**  
Worker commits and pushes to `dev`, either as direct commit(s) or via a feature branch when appropriate, then merge into `dev`.

**Step 7 — Human test (required step; human may skip the actual exercise).**  
Human pulls latest `dev`. The workflow asks the human to test the ticket in the app (start containers etc. as needed) and confirm to the worker whether it works. The human may make a gut decision not to run the test that time. That does not remove the step from the workflow and does not authorise an agent-tester substitute (token cost; supervision is the point).

**Step 8 — Mark done or continue. Do not mark closed.**  
If the human confirms it works as expected, the worker (or the human) moves the ticket to `done` in both filename and body. **Closed is a TechnicalSupervisor ceremony** (§8.8), not a worker action. If it does not work, work continues until it does.

If during Steps 4–8 the worker hits the §5.7 stop-rule, they comment the ticket, tell the human, and do not invent the missing rule.

Related checklist item for every worker session: do not forget commit + push (see §4.3).

### 7.1 Worker construction skills (decided)

When writing application code the worker must know and use:

- OOP and OOD
- SOLID
- Temporal coupling (and that it is a coupling)
- Common software-development pitfalls and anti-patterns — avoid them
- Readability: method and variable names that a human can read; no throwaway names like `x` for a domain idea

These are standing worker skills, not optional style.

---

## 8. Tickets

### 8.1 Store

Tickets live in the git repo as markdown files, one file per ticket, in a repo-relative folder.

Proposed folder (not yet locked if the user prefers another name): `/app-name-1/tickets/`

Same meaning of `/app-name-1/` as in §6.2: repo-relative project directory, not a host-absolute path.

Do not use GitHub Issues or an external tracker unless the user later says to. Ticket comments that the TechnicalSupervisor must read are body text inside the ticket file (and therefore in git).

### 8.2 Shared increment, typed prefix

Ticket numbers use prefix codes. The increment variable is shared among all ticket types. Each ticket prefix also contains abbreviations of the ticket type(s) it has.

Examples:

- `F-07-DB` — a database ticket
- `F-08-DM-BL` — cross-cutting data model and business layer / services
- `F-09-CTL-UI-JS` — cross-cutting controllers, UI, and JavaScript
- `TA-13-open` — technical-architecture ticket for the Design architect (§8.13)

`F` and `TA` share one increment. There is not a separate TA sequence.

Tickets 7, 8, 9 were created in sequence and share the sequence. There are not separate sequences per type (not one DB sequence, one DM sequence, etc.). Shared sequence lets the human — and an agent listing the folder — infer which tickets are newer or older without opening file bodies.

### 8.3 Filename encodes identity and state

The filename is a status index. An agent that only needs to know **what exists and what state it is in** lists the ticket directory and reads filenames. It must **not** open a ticket file merely to learn status. It opens a ticket file only when it needs the body (title, approach, comments, extra skills, extra work discovered).

Pattern from the user (examples, not an exhaustive museum):

- `F-09-CTL-UI-JS-open.md`
- `F-10-CTL-done.md`
- `F-11-UI-JS-in-progress.md`
- `F-12-DB-blocked.md`
- `F-13-DM-closed-wont-do.md`

Filename parts, in order:

1. Kind prefix `F` (feature / ticket family as used in the brief; other kind letters are not defined yet).
2. Shared increment, zero-padded in the examples (`07`, `09`, `10`). Padding width is not locked; pick one width per project and keep it stable so sort order matches creation order.
3. One or more type abbreviations for the layer(s) / concern(s) the ticket is about, separated by `-`.
4. One state token, matching the state field inside the file.
5. Suffix `.md`.

When state changes, the agent updates the state field **inside** the file and **renames** the file so the two cannot drift. A filename that disagrees with the body is a defect.

### 8.4 Board states (kanban; decided starting vocabulary)

Filename state tokens and board columns are the same list. The column formerly called `todo` is named **`open`**.

Columns, left to right:

| Token | Column | Who may move *into* this state | Cap |
|---|---|---|---|
| `backlog` | Backlog | **Only** Kanban master creates here (from design spec, during ceremony) | any number |
| `open` | Open | Kanban master via `backlog → open`; worker via §8.11 exception only | fixed; **10** |
| `in-progress` | In progress | Worker when they start the human-approved ticket | 1 worker ⇒ typically 1 |
| `blocked` | Blocked | Worker or Kanban master | any |
| `done` | Done | Worker or human after test step passes (or human gut-skips and accepts) | any |
| `closed` | Closed | **Only** Kanban master, via `done → closed` | any |
| `closed-wont-do` | Closed (rejected) | Kanban master or human | any |

`done` is not terminal. `closed` and `closed-wont-do` are terminal. Do not collapse `done` into `closed`.

Example filenames after this vocabulary:

- `F-09-CTL-UI-JS-backlog.md`
- `F-10-CTL-open.md`
- `F-11-UI-JS-in-progress.md`
- `F-12-DB-blocked.md`
- `F-13-DM-done.md`
- `F-14-UI-closed.md`
- `F-15-DM-closed-wont-do.md`

### 8.5 Human-readable title

Every ticket also needs a human-friendly title / description of what the ticket is doing. That lives **inside** the file. The filename is the index; the title is not required to be duplicated into the filename (the user’s examples do not put the title in the filename).

### 8.6 Discovery comments on tickets

Workers write into the ticket file body:

- extra transient skills added for the ticket;
- extra living-knowledge files loaded;
- extra work discovered mid-ticket (especially unforeseen cross-module work);
- stop-rule events (§5.7): unknown / unexpected boundary / missing living-knowledge, written so a later TechnicalSupervisor can act without the worker’s session;
- each friction event: increment the ticket `friction` field by 1 and leave a concise comment (§8.9).

Comments the worker addresses to the Kanban master must say so in the comment. The Kanban master is required to read those in the next ceremony.

### 8.7 Why filename-state exists (cost)

Opening every ticket body to build a board would grow context with ticket history the agent does not need. Listing filenames is the cheap board. This is the same cost instinct as bite-sized worker context and “do not become a 30k-token context beast.”

### 8.8 Kanban ceremony (Kanban master; cannot be skipped)

A ceremony is a fresh Kanban master instance. It is not optional: new `backlog` tickets come from this role reading the design spec, not from magic. If `backlog` and `open` run dry, work stops until a ceremony runs.

The ceremony is more than two column moves. The Kanban master:

- Reads `project-design-spec*.md` (read-only) and breaks remaining work into `backlog` tickets (component-level order from the spec, not inventing a new architecture).
- Moves `backlog → open` up to the cap of 10.
- Reviews `in-progress`, `blocked`, `done`, and comments, including friction counters.
- For blockers: options to the human if known; else escalate; design-level gaps become TA tickets (problem only).
- Moves `done → closed` when implementation is accepted and living-knowledge (not the design spec) is updated if a recurring gotcha earned a sentence.
- Updates `/app-name-1/kanban.md` and ticket filenames together (§8.10).
- Must not edit the design spec.

Ticket breakdown happens **after** the human accepts a Phase 1 design spec. Weak spec ⇒ weak tickets; fix the spec via TA, do not warm the Kanban master to “remember what we meant.”

### 8.9 Friction counter (on the ticket, not in living-knowledge)

`friction` is an integer field on the ticket, default **0**, no upper bound. It is **not** a 1–5 opinion score.

On each friction encounter the **worker increments by 1** and leaves one concise comment describing that fail (stop-rule, test fail, wrong implementation, user/architect had to intervene, hit-and-miss). Four fails ⇒ four comments and `friction += 4`.

The counter is shown on the kanban board row for that ticket. Tickets are not living-knowledge; they live under `/app-name-1/tickets/`.

The Kanban master **looks** at friction during ceremony. High friction + a pattern that will recur may justify a short living-knowledge gotcha. High friction on a one-off does not. The counter is not a compiler that writes files.

This counter is also the reliability signal if, later, the human considers letting workers spawn the Design architect. Not authorised today.

### 8.10 Kanban board file

Path proposal: `/app-name-1/kanban.md`.

A newly spawned worker shows this file first so the human sees the board without opening every ticket.

Both Kanban master and worker may write it, with the column permissions in §8.4.

**Dual-representation defect:** filenames already encode state. The board file is a second copy. Any state change must update **filename + ticket body + board row** in the same action. Mismatch is a defect. Prefer generating the board from filenames if an agent can do that cheaply; until then, atomic triple update.

Board rows show at least: ticket code, title, state, friction, created-by.

### 8.11 Worker-created `open` tickets (exception)

Default: only the Kanban master creates tickets (`backlog`) and only the Kanban master fills `open` from `backlog`.

Exception: if the worker hits unexpected extra work **and it is clear to the worker how to implement it** without a design question, the worker may create a new **non-TA** ticket directly in `open` and work it (still after human approach approval on that new ticket). Purpose: avoid you having to mid-sprint gymnastics-spawn the Kanban master for an obvious follow-on.

Hard limits:

- Must not be a TA / design-level / coupling-structure ticket.
- If not clearly implementable from existing living-knowledge + current ticket, this exception is forbidden — stop-rule instead.
- Created-by = Worker. Next ceremony the Kanban master still reviews it.

### 8.12 Ticket body fields (decided)

Every ticket file includes:

- `Created by:` agent image name
- `DateTimeCreated:`
- `TicketType:` unabbreviated types, e.g. `Technical Architecture` or `Database, Data Model, UI`
- `friction:` integer, default 0
- `Depends on:` list of ticket code + title (may include `blocked by:`)
- State field matching the filename
- Human title and description
- Comments log

### 8.13 TA tickets (Design architect’s inbox)

Kind prefix `TA`. Same shared increment as `F-` tickets, same folder. Example: `TA-13-open.md`.

- Only the Kanban master creates them.
- Worker never creates them. Worker blocks and comments; Kanban master decides whether a TA is warranted.
- Content: problem, impact, needs, enough context to solve **without** reading a non-TA ticket. No recommended design solution.
- May depend on a non-TA ticket; the Design architect must not follow that link unless the TA is incomplete (then that is a Kanban-master writing defect).
- May depend on other TA tickets; Design architect may read those.
- Design architect marks the TA `done` and leaves a concise solution comment. Kanban master later closes it in ceremony after reflecting any spec change the Design architect already wrote.

---

## 9. Git conventions

### 9.1 Gitflow stance from the brief

- Start from the latest `dev`.
- Use a feature branch when appropriate.

### 9.2 Commit messages

Every git commit for a ticket should be prefixed with:

- the agent name, and
- the ticket code if there is one, and
- a human-oriented commit title: short, intuitive description of what the ticket does.

Purpose: the human can walk the git tree and see where work was done.

Forbidden: commit names that only reflect an agent’s internal thinking process, e.g. `apply logic GARHJE345JER3 based on internal step KSLDF9G23G48`. Those are treated as anathema: they make no sense to a human and are the opposite of clarity.

### 9.3 Commit granularity

One feature per commit. Do not mix unrelated changes.

### 9.4 Persistence of living context

Living-context `.md` files are committed and pushed as soon as they are modified. Context is not allowed to live only on one machine or only in one agent session.

---

## 10. Communication rules

- Prefer few agents over many specialists, to keep the channel count small.
- Keep talk on project / feature / workflow substance.
- Do not run unbounded agent-to-agent threads.
- If a thread becomes long, escalate to the human; do not hope more tokens will converge it.
- Treat looping discussions as a failure mode requiring escalation.
- Shared-vocabulary risk between skillsets is in-scope (§2.4). Workflow must not assume two agents “just understand” each other’s layer jargon without shared living-knowledge files, shared ticket type abbreviations, and human-readable ticket bodies.
- Questions to the human must carry their own context (§0.7).
- One worker at a time (§5.3), so worker-to-worker channels are out of scope until that constraint is lifted.

Human is in the loop for:

- requirements and planning with the TechnicalSupervisor;
- invoking Phase 2 ceremonies and spawn triggers;
- choosing the next ticket from `open` (worker may suggest 4–5 from that list);
- approving or altering implementation approach;
- testing after pull;
- receiving stop-rule reports from the worker and spawning the TechnicalSupervisor;
- long-thread escalation;
- important choices and expensive-change estimates the TechnicalSupervisor cannot safely make alone;
- living-knowledge improvements the TechnicalSupervisor escalates.

---

## 11. Mapping of original “nice to have” items

The brief labelled the following as “Nice to have’s,” but wrote them as operative design. They are recorded here as design intent from the brief, status: **proposed as part of the workflow, not yet classified as optional vs mandatory.** Gate 3 should classify them.

1. Two first-class knowledge types (skills vs problem context); resolve unknowns; do not code against assumptions. (§6)
2. Spawn protocol: type-specific skills file → next tickets → layer context → transient extra skills with ticket comments → manager promotion of repeated skills into area defaults → mid-ticket cross-cutting context load with comments → session teardown. Restore manager via `ManagerArchitectAgent.md`. (§5–§7)
3. Worker vs manager-architect skill/context split; manager avoids reading all details; worker context is bite-sized. (§5)
4. `/app-name-1/living-knowledge/` as model-independent and machine-independent app memory; cohesion/coupling constraints on those files. (§6.2)
5. Overview files vs detailed files per module+layer; content policy; size cap vs ~30k token prompts; manager escalates file improvements. (§6.3)
6. Shared ticket sequence with type abbreviations in the code; human titles; human-readable commit prefixes. (§8–§9)
7. Gitflow: latest `dev`; feature branch when appropriate. (§9.1)

---

## 12. Explicit non-goals of *this* document (Gate 1)

This rewrite does not:

- implement agents, prompts, or Grok Build wiring;
- decide that this design beats industry alternatives (that is Gate 3);
- invent a different team topology unless needed later after discussion;
- silently “fix” the brief by dropping constraints that look costly or awkward;
- add a large new architecture that was not in the brief.

---

## 13. Decision log and remaining questions

### 13.1 Resolved

1. **Living-knowledge path.** Repo-relative `/app-name-1/living-knowledge/` (formerly living-context). `app-name-1` stands for the project directory name.
2. **Ticket store.** One markdown file per ticket. Filename includes shared sequence, type abbreviations, and board state. States: `backlog`, `open`, `in-progress`, `blocked`, `done`, `closed`, `closed-wont-do`. Token `open` is retired. Proposed folder: `/app-name-1/tickets/`. `open` cap proposed at 10. Only TechnicalSupervisor may `backlog → open` and `done → closed`.
3. **Concurrency.** One worker at a time for now.
4. **Human approve-then-test loop.** Required step. Human may gut-skip a given test. Agent-tester rejected. Pass → `done`, not `closed`.
5. **Spec format.** Markdown only. No `.docx`.
6. **Document versions.** Do not increment until the user says to. Edit this file in place.
7. **Questions to the human.** Must carry their own context (§0.7).
8. **Shared vocabulary.** A + B + DDD extra (§2.4–§2.5).
9. **Living-knowledge author skill set.** Full §6.6 must + should list.
10. **Team topology.** Option 1 architect-primary, class name TechnicalSupervisor. No non-technical manager. No separate ticket-cutter. Two staffing phases. Workers do not write living-knowledge.
11. **Semantic-defect pass.** Editors run §0.8 after material changes.

### 13.2 Still open

1. **Agent names in commits.** `TechnicalSupervisor` / `worker`, plus session id, or something else?
2. **First living-knowledge files.** Human only, or human + first TechnicalSupervisor session? Current topology implies the latter in Phase 1.
3. **Grok Build platform constraints** (session spawn, shared working tree).
4. **“Feature branch when appropriate.”**
5. **Ticket type abbreviations.** Fixed list vs per-project list in living-knowledge.
6. **Ticket folder name.** `/app-name-1/tickets/` is a proposal.
7. **Kind prefix `F`.** Only `F`, or other families later?
8. **Zero-padding width** for the shared ticket increment.
9. **`open` cap.** 10 is a proposal.
10. **Escalate threshold** in §5.8 (~15–20% of the 6M weekly planning assumption).
11. **Living-knowledge module card** (§6.7). Recommendation: mandatory short card. Not frozen.
12. **Friction ordinal** (§8.9). Try it; not a science.
13. **Phase 1 exit criteria.** Still need an observable checklist.
14. **Weekly token budget.** User planning assumption (~6M), not a vendor fact.

### 13.3 Suggested next step

Review the defects listed in the conversation that added §0.8, the kanban vocabulary change, and the module-card recommendation. No implementation until the user explicitly says so.
