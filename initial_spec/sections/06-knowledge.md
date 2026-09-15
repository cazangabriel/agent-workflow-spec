## 6. Knowledge model

Two knowledge types: skills, and problem context. Do not implement against assumptions.

### 6.1 Skill loading

Spawn reads that type’s init file.

### 6.2 Living-knowledge

Repo-relative `/app-name-1/living-knowledge/`. Design spec written only by DesignArchitect-warm + human. KanbanMaster-cold reads it to cut tickets. Workers do not write living-knowledge.

### 6.3 File types

Overview vs detail per module+layer. Size cap: 3–4 detail files must not become a ~30k-token beast.

### 6.4 Transient extra skills

Named on the ticket. Repeats may be promoted into the area file by KanbanMaster-cold.

### 6.5 Cross-cutting discovery

Load the other file, comment the extra work. Design-level coupling → TA ticket.

### 6.6 Skills to write living-knowledge

Must: software development, domain UL, propositional-logic discipline, uncertainty honesty, spec drafting, UL discipline, architecture as decision writing, cohesion/coupling as writing, audience split, edit policy.

Should: requirements framing, contracts, what a later worker needs, corpus IA, stack literacy, git.

### 6.7 Living-knowledge file header

```text
## Living-knowledge file header
- Id: <stable-id>
- Audience: overview | detail
- Provides: <3–7 bullets>
- Relies-on: <other-id> — <why you would open it>
- Does-not-cover: <explicit out-of-scope>
```

### 6.8 Per-project abbreviation lists

Two lists, both written by DesignArchitect-warm in Phase 1, both living-knowledge (not this workflow spec):

1. **Module abbreviations** — product areas in this repo (`AM`, `CHANADV`, `RECIPES`, plus a reserved cross-cut token such as `XCUT`). Used as the first token of every ticket filename (§8.3).
2. **Layer / ticket-type abbreviations** — technical slices (`DB`, `UI`, `API`, …). Used after the kind token.

KanbanMaster-cold may only use tokens from these lists. Adding a token later is a living-knowledge edit, not a silent invention on a filename.
