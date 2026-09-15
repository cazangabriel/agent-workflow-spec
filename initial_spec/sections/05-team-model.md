## 5. Team model

### 5.1 Skill family and two images

TechnicalSupervisor is the skill family name, not a spawned agent. Spawned types: DesignArchitect-warm, KanbanMaster-cold, Worker-cold. No non-technical manager. Architect-first.

Worker-cold implements; does not write living-knowledge, design spec, or tickets. Moves only tickets the human has assigned, among `open` / `in-progress` / `in-testing` / `blocked`.

DesignArchitect-warm owns the design spec, the two abbreviation lists (§6.8), and TA tickets. Must not read the board or non-TA tickets unless a TA is incomplete.

KanbanMaster-cold cuts tickets, runs ceremonies, writes module living-knowledge, creates self-contained TA problems, regenerates `kanban.md`. Does not invent module or layer tokens. Does not stamp named cluster ids.

### 5.2 Cost model

Design talk stays with DesignArchitect-warm + the spec. Board talk dies with each KanbanMaster-cold. Worker talk dies with each worker instance.

### 5.3 Concurrency

One worker *agent* at a time. Several tickets may be `in-progress` or `in-testing` because the human drives the board. `open` target ~10, hard stop 15.

### 5.4 Topology

DesignArchitect-warm + KanbanMaster-cold + Worker-cold. Defects in §5.9.

### 5.5 Phase 1 and Phase 2

Phase 1 = design spec accepted: contexts, UL, 3–4 -ilities, module/layer map, stack, component order, **module abbreviation list**, **layer abbreviation list**. Not a ticket list.

Phase 2 = ticketed delivery. Mini Phase 1 = new bounded context. Human declares phase changes.

Phase 1 exit (human must accept): contexts + UL; 3–4 -ilities; module + layer map; stack; component order; unknowns listed; living-knowledge skeletons; both abbreviation lists; explicit “accept Phase 1?”

### 5.6 Token arithmetic

Quadratic cost is session length without write-and-reset.

### 5.7 Stop-rule

Worker stops on unknown / missing living-knowledge / design-level coupling. Escalation: human → KanbanMaster-cold → TA → DesignArchitect-warm.

### 5.8 Escalate band — who it applies to

Planning assumption: ~6M tokens/week.

The 15–20% band (~0.9–1.2M tokens) applies to **DesignArchitect-warm** and **KanbanMaster-cold** talking about a design area, a ceremony slice, or a structural change. They stop and ping the human with what they want to do, a ball-park, and the uncertainty. Human says go / shrink / drop.

It does **not** apply to Worker-cold tickets. A worker ticket that would cost on that order is already a granularity defect (§8.14). The worker asks to **split** the ticket, not to spend a million tokens on it.

### 5.9 Defects

Extra hop only works if TAs are complete. Two living-knowledge writers can drift. `kanban.md` must stay generated. Lazy slugs make the cheap board useless. Module-first filenames only help if the module list is stable — renaming `AM` later means renaming tickets.

### 5.10 Init file vs commit name

Init file = skills doc. Commit line: `Worker-cold AM-10-F-DB add recipe table`.

### 5.11 Worker reuse (no named clusters)

Reuse the same Worker-cold instance when the human next picks a ticket in the same module, or one that `Depends on` the ticket just done.

Start a new instance when the next ticket is a different module with no dependency.

KanbanMaster-cold may still *prefer* to pull same-module or dependent tickets into `open` together. That is a pull habit, not an id stamped on the file.
