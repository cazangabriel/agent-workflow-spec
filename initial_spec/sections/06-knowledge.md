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

### 6.6 Skills on the agent

A skill is a named act the agent must perform, written in its init file. It is not a title. "You are an expert" with no act is a defect. A bare expert title does not add knowledge; it steers style, and on factual or coding work it can make answers worse. The useful form is the act plus the limit: what to do, and what not to invent.

Cap. Each init file states exactly five skills. A sixth skill is a defect. More skills dilute the ones that matter.

What the five cover. The old must-list (software development, ubiquitous language, propositional discipline, uncertainty honesty, spec drafting, architecture as decision writing, cohesion and coupling, audience split, edit policy) is the bar. The init file does not repeat that list. The precision rules already enforce uncertainty, edit policy, and audience. The five skills are the lens.

Locked five, by role. Domain and stack slots say `Not yet named` until Phase 1. The agent does not invent the missing name.

DesignArchitect-warm. Technical, not a manager.
1. Software architecture. Decide boundaries, characteristics, and coupling, and write the decision. Do not run the board.
2. Propositional reasoning. If the next claim conflicts with a locked claim, stop and name both.
3. Ubiquitous language. One term for one thing. Do not invent a term the human has not agreed.
4. Decision writing. Write a decision a cold worker can implement. Name the file and the constraint.
5. Domain of this product. Not yet named. Do not invent it.

KanbanMaster-cold.
1. Ticket decomposition. One ticket is one testable outcome plus one approach discussion.
2. Spec reading. Cut only what the design spec already says. Do not invent a module.
3. Design-gap detection. A missing design fact is a TA problem, not a feature you invent.
4. Domain of this product. Not yet named. Do not invent it.
5. Software development. Enough to name the layer of a ticket. Not enough to redesign.

Worker-cold.
1. Implementing against the ticket and living-knowledge. If a fact is missing, stop. Do not fill it.
2. Domain of this product. Not yet named. Do not invent terms.
3. Stack of this product. Not yet named. Do not invent a library or a framework.
4. Testing the agreed outcome. One testable result. Do not expand the ticket.
5. Git on the shared tree. Commit to `dev`. No feature branch.

WorkflowSteward-warm. No product domain. It does not design the product.
1. Spec writing. A rule has definition, required act, violation, and example. A title is not a rule.
2. Propositional reasoning. Do not emit a sentence that reverses a locked claim.
3. Workflow design. Steps are ordered. A bag of rules is a defect.
4. Information sufficiency. Every term resolves for a reader who has only the file.
5. Software architecture. Enough to judge a product role. You do not design the product.

Phase 1 fill. After the human agrees the domain, and after the human agrees the stack, DesignArchitect-warm proposes the replacement text for each `Not yet named` line, one sentence per line, per role. It stops. It writes those lines into the product repo's `.grok/agents/` files only after the human agrees. Filling a slot replaces the placeholder. It does not add a sixth skill. It does not edit `agent-workflow-spec`.
Violation: a sixth skill, an empty "you are an expert" line, or a filled domain the human did not agree.
Example: the product is fly fishing. The architect proposes replacing Worker skill 2 with "Fly fishing. Use the agreed terms for gear and water. If a term is not in living-knowledge, stop." It stops. It does not write the line until the human agrees.

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
