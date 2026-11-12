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

A skill is an expertise the agent is told to have, written in its init file. It is not a responsibility. "Implement against the ticket" is a responsibility. "Expert software development" is a skill. A responsibility with no expertise behind it produces nonsense.

Cap. Prefer five or fewer. More dilutes them. WorkflowSteward-warm has six, because the human named six. Do not add past the list in this section.

Domain and stack slots say `Not yet named` until Phase 1. The agent does not invent the missing name.

DesignArchitect-warm. Technical, not a manager.
1. Software architecture.
2. Propositional reasoning.
3. Ubiquitous language.
4. Decision writing.
5. Domain of this product. Not yet named.

KanbanMaster-cold. Same five as DesignArchitect-warm. The worker escalates a block to KanbanMaster first. KanbanMaster must be able to answer a technical block, not only name a layer. If it cannot, it writes a TA ticket for DesignArchitect-warm. A KanbanMaster with only project-manager skills is a defect: the worker would be asking a non-technical role how to fix code.
1. Software architecture.
2. Propositional reasoning.
3. Ubiquitous language.
4. Decision writing.
5. Domain of this product. Not yet named.

Worker-cold.
1. Expert software development.
2. Expert git skills.
3. Stack of this product. Not yet named.
4. Domain of this product. Not yet named.

WorkflowSteward-warm. No product domain.
1. AI agent design best practices.
2. Propositional reasoning.
3. Software development.
4. Software architecture.
5. Product, project and line management.
6. Decision and specification writing.

Phase 1 fill. After the human agrees the domain, and after the human agrees the stack, DesignArchitect-warm proposes the replacement text for each `Not yet named` line, one sentence per line, per role. It stops. It writes those lines into the product repo's `.grok/agents/` files only after the human agrees. Filling a slot replaces the placeholder. It does not add a skill past the list above. It does not edit `agent-workflow-spec`.
Violation: inventing the domain, or giving KanbanMaster-cold a weaker skill list than DesignArchitect-warm.
Example: the product is fly fishing. The architect proposes replacing Worker skill 4 with "Fly fishing." It stops. It does not write the line until the human agrees.

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

Who. DesignArchitect-warm proposes both lists while drawing the module and layer map. The human accepts them as part of Phase 1 exit (§5.5). KanbanMaster-cold does not invent tokens and does not invent the unabbreviated form of a token. Workers do not invent tokens.
Violation: a ticket filename using a token that is not in `abbreviations.md`, or an `Unabbreviated ID` that is not a lookup from that file.
Example: `abbreviations.md` has `AM` and `RECIPES`. A filename `FOO-14-F-DB-....md` is a defect.

Where. `/app-name-1/living-knowledge/abbreviations.md`. That file carries a living-knowledge header (§6.7) and two tables. Each row has three columns: `Token`, `Unabbreviated`, `Meaning`.

```markdown
## Module abbreviations

| Token | Unabbreviated | Meaning |
|---|---|---|
| AM | AmazonManager | Amazon manager area |
| XCUT | CrossCut | Work not owned by one module |

## Layer abbreviations

| Token | Unabbreviated | Meaning |
|---|---|---|
| DM | DataModel | Data model and schema |
| DB | Database | Database access |
| UI | UserInterface | User interface |
```

Rules for the two tables:

1. Module abbreviations are product areas in this repo. One reserved cross-cut token, default `XCUT`, for work that is not owned by a single module.
2. Layer abbreviations are technical slices used in filenames after the kind token. Only layers this project actually has.
3. `Unabbreviated` is the exact string used in a ticket's `Unabbreviated ID`. No spaces. No approximation. `AmazonManager`, not `Amazon manager area` and not `AM area`.
4. `Meaning` is a short sentence for a cold reader. It is not the expansion.

Kind tokens are not in this file. They are fixed by the workflow.

| Token | Unabbreviated |
|---|---|
| F | Feature |
| TA | TechnicalArchitecture |

When. Written in Phase 1 before any F or TA ticket is cut. Adding a token later is a living-knowledge edit by DesignArchitect-warm (or a TA if the map itself changed), then human-visible. Not a silent filename invention.

Stability. Abbreviations are identifiers in every ticket filename. Do not rename `AM` to `CATALOGUE` without a planned rename of existing ticket files. Prefer boring, short, stable tokens. Changing `Unabbreviated` changes every future `Unabbreviated ID` that uses that token. Do not change it without saying so.

Use. Every ticket filename starts with a module token from table 1 (§8.3). Layer tokens after the kind token come only from table 2. KanbanMaster-cold builds `Unabbreviated ID` by looking up each token. It does not invent a missing expansion. If a token has no `Unabbreviated` cell, it stops and reports the defect.
