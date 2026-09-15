## 2. Purpose and problem

### 2.1 Purpose

Design the workflow for a team of agents running via Grok Build.

The team’s job is to build a software development project specified by the human user. Example project types (not an exhaustive closed list): public-facing website with a CMS; mobile store app; mobile game; Unity game; other software the user specifies.

### 2.2 What “optimal” means here

Design an optimal team workflow based on proven methods that give good or outstanding results in real-life human teams — then adapt those methods to agents, token cost, and context windows.

### 2.3 Suggested targets, and what is now decided

Original brief treated these as starting targets, not hard constraints. User decisions since then:

- **Decided:** no TechnicalSupervisor *instance*. Spawned roles are DesignArchitect-warm, KanbanMaster-cold, and Worker-cold. No non-technical manager. No separate ticket-cutter besides KanbanMaster-cold.
- DesignArchitect-warm owns the design spec. KanbanMaster-cold cuts tickets and runs the board. Worker-cold implements.
- Knowledge that must survive a session lives in organised `.md` files on git alongside the project (living-knowledge).
- Worker agents are transient: spawned for one or a few tasks / features, so their context windows stay short and they stay cheap in prompt-token cost.
- **Decided:** one worker at a time for now. Do not run parallel workers until the user reopens that constraint.
- **Decided:** human-approve-then-test loop is required on every ticket. The human may skip actually performing a test as a gut call, but the loop itself stays in the workflow. Reason: the user does not consider current AI reliable for unsupervised work; testing is how the user supervises. Agent-driven testing against requirements is **not viable yet** because it would consume scarce tokens.
- **Decided:** two staffing phases (§5.5). Phase 1 = design spec accepted (DesignArchitect-warm central). Phase 2 = ticketed delivery (KanbanMaster-cold ceremonies). Mini Phase 1 when a new bounded context is added.
- **Decided:** workers do not write living-knowledge. They write ticket comments and raise unknowns to the human.

### 2.4 Shared vocabulary between agents (original phrase: “language barriers”)

Original brief: “We must take into account potential language barriers between agents with different skillsets.”

That phrase is ambiguous. The two readings, and why the question was asked:

- **Reading A — natural language:** agents (or agent vs human) mixing English with another spoken/written language, or mixing dialects, so the same requirement is not stably worded.
- **Reading B — specialist vocabulary:** a worker loaded with DB skills and a worker loaded with UX skills use different words for the same thing, or the same word for different things, or different precision about the same feature. Tickets and living-knowledge then fail to bind.

The surrounding sentence named **skillsets**, not human national languages, so Reading B is the stronger parse of the original brief. Reading A can still happen and is cheap to forbid.

**Decided package:** A + B + DDD extra (§2.5). Rules below are the operational form of that package.

1. All tickets, living-knowledge files, commit messages, and agent-to-human questions are written in one project language (the language of this spec: English), unless the user sets another project language in living-knowledge.
2. Layer jargon is allowed **inside** a layer’s detailed living-knowledge file.
3. Anything that crosses agents or roles — ticket titles, ticket type abbreviations, shared terms for modules and layers, status words — must use the shared vocabulary recorded in living-knowledge and in this spec. Do not invent a second name for an existing shared term.
4. If two skillsets would naturally use different words for the same concept, the first writer picks one term, writes a one-line alias if needed, and later agents reuse that term. Do not “translate” between agent jargons in flight.
5. Precision differences are not resolved by chat. If a ticket is too vague for a layer specialist to implement without guessing, the worker raises the unknown to the human instead of filling it with assumptions (§0.5).

### 2.5 Ubiquitous Language (decided: A + B + DDD extra)

The professional name for most of Reading B is **Ubiquitous Language** (domain-driven design): one language shared by the people (and here, agents) who talk about the domain, used in speech, tickets, living-knowledge, and code, with no private translation layer between “business words” and “implementation words.”

Overlap with A / B / A+B:

- **A (natural language)** is only a precondition. Ubiquitous Language can be English, Romanian, or anything else; it does not choose the vehicle language. Rule 1 in §2.4 (one project natural language) sits *under* UL, not inside it.
- **B (specialist vocabulary clash)** is what UL is for. DB, UX, domain, and planning must not keep private synonyms for the same concept inside one bounded context.
- **A+B** is therefore: pick one vehicle language (A) + enforce UL inside each bounded context (B).

DDD nuance that must not be lost: **different bounded contexts may have different ubiquitous languages.** That is allowed. What is not allowed is an implicit, unwritten translation. If two contexts use the same English word for different things, living-knowledge at the boundary records the map. Layer specialists (DB vs UX) are **not** automatically different bounded contexts; often they are implementation dialects of one context and should collapse onto one term list.

Living-knowledge is where the UL is written down so a later cold agent can speak it. Tickets must use that language, not a second dialect.

**Decided:** adopt the package **A + B + DDD extra**, not A alone, not B alone, not A+B with a single global glossary.

- A alone: everyone writes English and still says “order” three ways. Useless against the original skillset-barrier sentence.
- B alone: shared terms, but files may switch vehicle language mid-project. Cheap to prevent; not the core problem.
- A+B with one glossary for the whole app: smashes distinct bounded contexts into false unity, or produces an unusable mega-glossary.
- DDD extra without B: boxes on a diagram and informal talk inside each box.

Default vehicle language is English unless the user sets another in living-knowledge. Layer implementation dialect (SQL-ish vs CSS-ish) may appear inside a *detail* file when it names a technical mechanism; domain concepts in that same file still use the context’s UL.

Status: **decided** by the user.
