## 6. Knowledge model

Agents have two distinct, first-class knowledge types:

1. **Skills** — what they know how to do, and how well.
2. **Problem context** — how many things they know about a problem or feature before they start.

Ideal: reduce unknowns and uncertainty toward zero before implementation. Do not implement against assumptions.

### 6.1 Skill loading

When an agent is spawned, it learns base skills from a `.md` file dedicated to that agent type (`DesignArchitect-warm.md`, `KanbanMaster-cold.md`, `Worker-cold.md`).

After base skills: the agent is pointed at the next ticket, or offers a short list of already-ordered tickets so the human can choose.

### 6.2 Problem-context loading (living-knowledge)

App knowledge lives under the repo-relative path `/app-name-1/living-knowledge/` (formerly `/app-name-1/living-context/`).

The **project design spec** is a distinct artifact (proposal: `/app-name-1/living-knowledge/project-design-spec.md`). Only DesignArchitect-warm and the human write it. KanbanMaster-cold reads it to cut tickets.

`app-name-1` is a stand-in for the actual project directory name. Paths starting with `/app-name-1/` are repo-relative.

Properties:

- Files are pushed to git as soon as they are modified.
- Workers do not write these files. KanbanMaster-cold writes module living-knowledge. DesignArchitect-warm writes the design spec and may edit living-knowledge when a TA/design change requires it. Human may edit either.
- High cohesion and low coupling among files. Avoid “microservice hell” of 30 files that all point at each other.
- **Source of truth:** living-knowledge files (plus ticket files and this spec), not session memory.
- Context must be AI-model-account independent and machine independent (everything on git).

### 6.3 File types inside living-knowledge

**A. Overview files**, one per module+layer. Audience: DesignArchitect-warm or KanbanMaster-cold. Overview altitude, not expert dump.

**B. Detailed files**, one per module+layer. Loaded by workers when a ticket touches that area, or a cross-cut appears.

Body content policy: technical decisions, conventions, integrations, principles, -ilities that apply here, gotchas, pitfalls, what to avoid and what to do instead, stack usage to prefer/avoid, traps, preferred ways of working.

Not wanted by default: member lists, javadoc theatre, a copy of the code in words.

Size constraint: loading 3 or 4 detail files must not make a new worker a ~30k-token context beast.

### 6.4 Transient extra skills mid-ticket

If the ticket implies a layer, the worker reads that living-knowledge file. Extra skills for the ticket only are named in a ticket comment. KanbanMaster-cold may later promote repeated extras into the area file as defaults.

### 6.5 Cross-cutting discovery mid-ticket

If work spills into another module/layer, the worker loads that file, does the integration without assumptions, and comments the extra work. KanbanMaster-cold may conclude tight coupling — if it is design-level, write a TA ticket rather than a cute follow-on ticket. Session ends unless §5.11 reuse applies.

### 6.6 Skills required to *write* living-knowledge

Workers may propose via ticket comments; they are not default authors.

**Must have:** software development; domain knowledge of this app; propositional-logic discipline; uncertainty honesty (§0.5); specification drafting; UL discipline (§2.5); architecture as decision writing; cohesion/coupling as a writing skill; audience split; edit policy (§0.3).

**Should have:** requirements framing; contract/integration thinking; what a later worker needs in order not to guess; information architecture of the markdown corpus; stack literacy; git skill.

**Must not:** javadoc dumps; document every method; unsolicited rewrite of stable terms; load every implementation skill in the universe.

Writers: KanbanMaster-cold (module living-knowledge), DesignArchitect-warm (design spec + design-required living-knowledge edits), human.

### 6.7 Living-knowledge name, splits, and file header

Store name: **living-knowledge**. Folder: `/app-name-1/living-knowledge/`.

Split vertically (bounded contexts / modules), horizontally (overview vs layer detail), or both when size still blows the 3–4-file budget. Do not split into microservice-hell.

**Living-knowledge file header** (do not call this a “module card”). Fixed block at the top of every living-knowledge file. The body under it is the real content (§6.3).

```text
## Living-knowledge file header
- Id: <stable-id>
- Audience: overview | detail
- Provides: <3–7 bullets a worker can trust to find in this file>
- Relies-on: <other-id> — <one line: why you would open it>
- Does-not-cover: <explicit out-of-scope>
```

No method signatures. If the header grows longer than useful, cut the header, not the body.
