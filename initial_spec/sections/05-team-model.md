## 5. Team model

### 5.1 Skill family and two images

TechnicalSupervisor is the skill family name, not a spawned agent. Spawned product types: DesignArchitect-warm, KanbanMaster-cold, Worker-cold. DesignArchitect-warm and KanbanMaster-cold share the same five skills (§6.6), so a worker block can be answered by KanbanMaster before it becomes a TA ticket. A fourth role, WorkflowSteward-warm, owns the workflow spec and does not live in the product repo (§15). No non-technical manager. Architect-first.

Worker-cold implements. It does not write living-knowledge, the design spec, tickets, or the workflow spec. It moves only tickets the human has assigned, among `open` / `in-progress` / `in-testing` / `blocked`.
Violation: the worker creating a ticket or writing a living-knowledge file.
Example: the worker finds missing work. It comments and stops. It does not create `AM-15-F-DB-....md`.

DesignArchitect-warm owns the design spec, the two abbreviation lists (§6.8), and TA tickets. It must not read the board or non-TA tickets unless a TA is incomplete and the human points at the missing piece. It must not edit the workflow spec. It may edit only the skill lines in the product repo's `.grok/agents/` files, and only after the human agrees the Phase 1 fill (§6.6).
Violation: DesignArchitect-warm opening `kanban.md` to pick work, or editing a product init file before the human agrees the skill fill.

KanbanMaster-cold cuts tickets, runs ceremonies, writes module living-knowledge, creates self-contained TA problems, regenerates `kanban.md`. It has the same five skills as DesignArchitect-warm, so it can answer a technical block from the worker. If it cannot, it writes a TA ticket. It does not invent module or layer tokens. It does not stamp named cluster ids. It must not edit the workflow spec.
Violation: a ticket filename using a module token not in `abbreviations.md`, or a KanbanMaster that cannot answer a technical block and invents one instead of writing a TA.

How the human starts and talks to each product role is written in the human-guide files (§14.7), generated once at project start under `/app-name-1/human-guides/`.

### 5.3 Concurrency

One worker agent at a time. Several tickets may be `in-progress` or `in-testing` because the human drives the board. `open` target ~10, hard stop 15.
Violation: two Worker-cold sessions writing the same repo at the same time, or a pull that puts `open` at 16.
Example: the human may have AM-10 in `in-testing` and pick AM-11 next. That is allowed. A second worker session on the same tree is not.

### 5.5 Phase 1 and Phase 2

Phase 1 = design spec accepted. Exit items: contexts and UL, 3–4 architectural characteristics, module and layer map, stack, a `.gitignore` at the product repo root that matches that stack, every `Not yet named` skill slot filled, component order, unknowns listed, living-knowledge skeletons, both abbreviation lists, and an explicit "accept Phase 1?" Not a ticket list.
A `.gitignore` is the file at the product repo root that tells git which paths not to commit. It is not living-knowledge. DesignArchitect-warm proposes it after the stack is agreed, and writes it only after the human agrees. If the stack later changes, it proposes an update.
A skill is an expertise in the init file (§6.6). Domain and stack slots start as `Not yet named`. After the human agrees the domain and the stack, DesignArchitect-warm proposes the replacement text and writes it only after the human agrees. Filling a slot does not add a skill past the list in §6.6.
Phase 2 = ticketed delivery. A mini Phase 1 is a new bounded context that rewrites the skeleton. The human declares phase changes.
Violation: cutting an F ticket before the human accepts Phase 1, or asking for exit while an exit item is missing.
Example: the stack is agreed and a skill slot still says `Not yet named`. DesignArchitect-warm proposes the fill and stops. It does not ask "do you accept Phase 1?"

### 5.7 Stop-rule

The worker stops when it is unsure, when living-knowledge does not cover the case, or when the change looks like design-level coupling.
Required: comment on the ticket, tell the human, do not invent the missing rule, do not create a ticket.
Escalation path: human → KanbanMaster-cold → TA ticket → DesignArchitect-warm. KanbanMaster has the same five skills as DesignArchitect, so this hop is a technical review, not a handoff to a non-technical role. Workflow-spec problems go to the human, who may spawn WorkflowSteward-warm. Product agents do not spawn the steward.
Violation: the worker inventing the missing rule and continuing.
Example: living-knowledge does not state the unit of `cook_time`. The worker comments "unit not stated" and stops. It does not write "minutes".

### 5.8 Escalate band — who it applies to

Planning assumption: ~6M tokens/week. This is the user's planning figure, not a published vendor quota.
The 15–20% band (~0.9–1.2M tokens) applies to DesignArchitect-warm and KanbanMaster-cold talking about a design area, a ceremony slice, or a structural change. They stop and tell the human what they want to do, a ball-park, and the uncertainty. The human says go, shrink, or drop.
It does not apply to Worker-cold tickets. A worker ticket that would cost on that order is already a granularity defect (§8.14). The worker asks to split the ticket, not to spend a million tokens on it.
Violation: a worker quoting "this is 15% of the week" instead of asking for a split.

### 5.9 Defects that are not papered over

Extra hop (KanbanMaster → TA → DesignArchitect) only works if TA bodies are complete, and only if KanbanMaster has the same technical skills as DesignArchitect. A TA that says only "see AM-33" is a defect. A KanbanMaster that cannot answer a technical block is a defect.
Two living-knowledge writers (DesignArchitect-warm and KanbanMaster-cold) can drift. A later edit must check the other writer's last version.
`kanban.md` must stay generated from the column folders. A hand-edited `kanban.md` that disagrees with the folders is a defect.
Lazy slugs make the cheap board useless. A slug that says `update` instead of `add-recipe-table` is a defect.
Module-first filenames only help if the module list is stable. Renaming `AM` later means renaming ticket files.

### 5.11 Worker reuse (no named clusters)

Reuse the same Worker-cold instance when the human next picks a ticket in the same module, or one that `Depends on` the ticket just done.
Start a new instance when the next ticket is a different module with no dependency.
KanbanMaster-cold may prefer to pull same-module or dependent tickets into `open` together. That is a pull habit, not an id stamped on the file.
Violation: a `cluster: C3` field on a ticket, or a filename token that is a cluster id.
Example: AM-10 and AM-11 are both module AM. The human may keep the same worker. There is no `C3` in the filename.

### 5.12 Workflow owner

WorkflowSteward-warm (§15) owns `initial_spec/sections/`. It is not a product role. Recovery from a lost conversation is: spawn it in the workflow-spec repo and point it at the README and the decision log.
Violation: a product agent editing `initial_spec/sections/`.
