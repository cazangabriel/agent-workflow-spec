## 6. Knowledge model

Two knowledge types: skills, and problem context. Do not implement against assumptions.
Violation: the worker filling a missing living-knowledge fact and coding it.
Example: the unit of `cook_time` is not in living-knowledge. The worker does not assume minutes and write the migration.

### 6.2 Living-knowledge

Repo-relative `/app-name-1/living-knowledge/`. The design spec is written only by DesignArchitect-warm and the human. KanbanMaster-cold reads it to cut tickets. Workers do not write living-knowledge.
Violation: the worker editing a file under `/app-name-1/living-knowledge/`.
Example: the worker comments on the ticket. It does not open `recipes.md` for write.

### 6.3 File types

Overview versus detail, per module and layer. Size cap: 3–4 detail files must not become a ~30k-token beast.
Violation: one living-knowledge file that a worker must load in full to answer a single-column question.
Example: `recipes.md` is an overview. `recipes-db.md` is the detail. The worker on a DB ticket loads `recipes-db.md`, not every recipes file.

### 6.6 Skills to write living-knowledge

Must: software development, domain ubiquitous language, propositional-logic discipline, uncertainty honesty, spec drafting, UL discipline, architecture as decision writing, cohesion and coupling as writing, audience split, edit policy.
Should: requirements framing, contracts, what a later worker needs, corpus information architecture, stack literacy, git.
Violation: a non-technical writer editing a living-knowledge file.
Example: a role that cannot state the ubiquitous language of the module does not write that module's file.

### 6.7 Living-knowledge file header

Every living-knowledge file starts with this header. A cold agent reads the header to decide whether to open the body.

```text
## Living-knowledge file header
- Id: <stable-id>
- Audience: overview | detail
- Provides: <3–7 bullets>
- Relies-on: <other-id> — <why you would open it>
- Does-not-cover: <explicit out-of-scope>
```

Violation: a living-knowledge file with no header, or a `Relies-on` that does not say why.
Example: `Relies-on: recipes-db — open this for column types`. Not `Relies-on: recipes-db`.

### 6.8 Abbreviation lists — Phase 1 step, DesignArchitect-warm owns them

These lists are not an open item. They cannot be filled in this workflow spec because they name areas of a specific product. They are a mandatory Phase 1 living-knowledge artifact.

Who. DesignArchitect-warm proposes both lists while drawing the module and layer map. The human accepts them as part of Phase 1 exit (§5.5). KanbanMaster-cold does not invent tokens. Workers do not invent tokens.
Violation: a ticket filename using a token that is not in `abbreviations.md`.
Example: `abbreviations.md` has `AM` and `RECIPES`. A filename `FOO-14-F-DB-....md` is a defect.

Where. `/app-name-1/living-knowledge/abbreviations.md`. That file carries a living-knowledge header (§6.7) and two tables:

1. Module abbreviations — product areas in this repo. Example shape: `AM` = Amazon-manager area, `RECIPES` = recipes area. One reserved cross-cut token, default `XCUT`, for work that is not owned by a single module.
2. Layer / ticket-type abbreviations — technical slices used in filenames after the kind token (`DB`, `UI`, `API`). Only layers this project actually has.

When. Written in Phase 1 before any F or TA ticket is cut. Adding a token later is a living-knowledge edit by DesignArchitect-warm (or a TA if the map itself changed), then human-visible. Not a silent filename invention.

Stability. Abbreviations are identifiers in every ticket filename. Do not rename `AM` to `CATALOGUE` without a planned rename of existing ticket files. Prefer boring, short, stable tokens.

Use. Every ticket filename starts with a module token from table 1 (§8.3). Layer tokens after the kind token come only from table 2.
