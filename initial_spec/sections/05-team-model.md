## 5. Team model

### 5.1 Skill family and two images

TechnicalSupervisor is the skill family name, not a spawned agent. Spawned types: DesignArchitect-warm, KanbanMaster-cold, Worker-cold. Init files and commit names as already written. No non-technical manager. Architect-first.

Worker-cold implements; does not write living-knowledge, design spec, or tickets. May move only the ticket they are executing, and only among `open` / `in-progress` / `in-testing` / `blocked`.

DesignArchitect-warm owns the design spec and TA tickets. Must not read the board or non-TA tickets unless a TA is incomplete.

KanbanMaster-cold cuts tickets, runs ceremonies, writes module living-knowledge, creates self-contained TA problems, stamps cluster ids, regenerates `kanban.md`.

### 5.2 Cost model

Design talk stays with DesignArchitect-warm + the spec. Board talk dies with each KanbanMaster-cold. Worker talk dies with each worker (or cluster run).

### 5.3 Concurrency

One worker at a time. `open` target ~10, hard stop 15 (§8.8). Blocked work outranks starting an unrelated `open` ticket.

### 5.4 Topology

DesignArchitect-warm + KanbanMaster-cold + Worker-cold. Defects in §5.9.

### 5.5 Phase 1 and Phase 2

Phase 1 = design spec accepted. That spec includes: bounded contexts and UL starter; 3–4 architectural characteristics; module/layer map; stack and how it will be used; component-level implementation order; per-project ticket-type abbreviation list (§6.8). Not a ticket list.

Phase 2 = ticketed delivery against that spec. Ceremonies cannot be skipped.

Mini Phase 1 = a **new bounded context** is added. Routine features inside an already-mapped module are not Mini Phase 1. Human declares phase changes.

Phase 1 exit (human must accept):

1. Bounded contexts named; UL starter written.
2. 3–4 -ilities ranked and written.
3. Module + layer map written.
4. Technology choices written.
5. Component-level implementation order written.
6. Remaining unknowns listed as unknowns.
7. Living-knowledge skeletons exist for mapped modules.
8. Type-abbreviation list written (§6.8).
9. Human answers: “Do you accept Phase 1 as done for this spec version?”

### 5.6 Token arithmetic

Quadratic cost is session length without write-and-reset, not “triage × design.”

### 5.7 Stop-rule

Worker stops on unknown / missing living-knowledge / design-level coupling. Comment, tell human, increment friction. Escalation: human → KanbanMaster-cold → TA → DesignArchitect-warm. Worker must not spawn the Design architect.

### 5.8 Escalate band — what the human actually sees

Planning assumption: ~6M tokens/week.

If an agent believes **this ticket or this design change** would burn on the order of 15–20% of that (~0.9–1.2M tokens), it must **stop before doing the work** and ping the human with: what it wants to do, the rough estimate, and the uncertainty. The human says go / shrink / drop. It is a warning + hold, not a silent refusal and not a background metric.

### 5.9 Defects of the two-image split

Extra hop only works if TAs are complete; two living-knowledge writers can drift; `kanban.md` must stay generated; workers do not create tickets; Phase 1 is sufficient-spec not every-class-named.

New: title slugs and cluster tokens in filenames can rot if KM regroups often (rename cost). Soft `open` cap can still inflate if “cluster” is used as an excuse. Filename slugs are not the full title — body remains authoritative.

### 5.10 Init file vs commit name

Init file = skills doc. Commit line = spawn name + ticket code + title.

### 5.11 Worker reuse and clusters

KanbanMaster-cold assigns `C<n>` when tickets share a dependency or the same module+layer, and pulls those into `open` together when it is straightforward — no weird stretches.

Worker-cold, looking only at `kanban.md` and filenames, suggests which cluster to take. Same instance walks the cluster. New instance when the next work is a different module with no dependency.

Do not keep one worker for the whole `open` column if later tickets are unrelated.
