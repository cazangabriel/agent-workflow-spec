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

### 6.8 Abbreviation lists — Phase 1 step, DesignArchitect-warm owns them

These lists are **not** left as a vague “open item.” They cannot be filled in *this* workflow spec because they name areas of a specific product. They **are** a mandatory Phase 1 living-knowledge artifact.

**Who.** DesignArchitect-warm proposes both lists while drawing the module/layer map. The human accepts them as part of Phase 1 exit (§5.5). KanbanMaster-cold does not invent tokens. Workers do not invent tokens.

**Where.** One living-knowledge file, proposed path:

`/app-name-1/living-knowledge/abbreviations.md`

That file carries a living-knowledge header (§6.7) and two tables:

1. **Module abbreviations** — product areas in this repo. Example shape: `AM` = Amazon-manager area, `RECIPES` = recipes area. One reserved cross-cut token, default `XCUT`, for work that is not owned by a single module.
2. **Layer / ticket-type abbreviations** — technical slices used in filenames after the kind token (`DB`, `UI`, `API`, …). Only layers this project actually has.

**When.** Written in Phase 1 before any `F-` / `TA-` ticket is cut. Adding a token later is a living-knowledge edit by DesignArchitect-warm (or a TA if the map itself changed), then human-visible. Not a silent filename invention.

**Stability.** Abbreviations are identifiers in every ticket filename. Do not rename `AM` to `CATALOGUE` without a planned rename of existing ticket files. Prefer boring, short, stable tokens.

**Use.** Every ticket filename starts with a module token from table 1 (§8.3). Layer tokens after the kind token come only from table 2.
