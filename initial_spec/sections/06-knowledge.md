## 6. Knowledge model

Agents have two distinct, first-class knowledge types:

1. **Skills** — what they know how to do, and how well.
2. **Problem context** — how many things they know about a problem or feature before they start.

Ideal: reduce unknowns toward zero before implementation. Do not implement against assumptions.

### 6.1 Skill loading

Spawn reads that agent type’s init file (`DesignArchitect-warm.md`, `KanbanMaster-cold.md`, `Worker-cold.md`).

### 6.2 Problem-context loading (living-knowledge)

Repo-relative `/app-name-1/living-knowledge/`.

Project design spec (proposal: `/app-name-1/living-knowledge/project-design-spec.md`): only DesignArchitect-warm and the human write it. KanbanMaster-cold reads it to cut tickets.

Paths starting with `/app-name-1/` are repo-relative. Files are pushed as soon as they change. Workers do not write living-knowledge. Source of truth is files, not session memory.

### 6.3 File types

Overview vs detail per module+layer. Body policy and the ~30k / 3–4-file size cap: unchanged from the previous writing of this section (decisions, conventions, integrations, principles, -ilities, gotchas, avoid/instead, stack usage). No javadoc theatre.

### 6.4 Transient extra skills mid-ticket

Named in a ticket comment. KanbanMaster-cold may later promote repeats into the area file.

### 6.5 Cross-cutting discovery mid-ticket

Load the other file, comment the extra work. Design-level coupling → TA ticket, not a cute follow-on.

### 6.6 Skills required to *write* living-knowledge

Must: software development; domain UL; propositional-logic discipline; uncertainty honesty; specification drafting; UL discipline; architecture as decision writing; cohesion/coupling as writing; audience split; edit policy.

Should: requirements framing; contracts; what a later worker needs; corpus IA; stack literacy; git.

Writers: KanbanMaster-cold (module files), DesignArchitect-warm (design spec + design-required edits), human.

### 6.7 Living-knowledge file header

Fixed block at the top of every living-knowledge file (not called “module card”):

```text
## Living-knowledge file header
- Id: <stable-id>
- Audience: overview | detail
- Provides: <3–7 bullets>
- Relies-on: <other-id> — <why you would open it>
- Does-not-cover: <explicit out-of-scope>
```

### 6.8 Per-project ticket type abbreviations

Not a global list in this workflow spec (a website and a Unity project do not share layers).

DesignArchitect-warm writes the list into living-knowledge during Phase 1 (same time as the module/layer map). KanbanMaster-cold may only use abbreviations from that list. Adding a new abbreviation later is a living-knowledge edit, not a silent invention on a ticket filename.
