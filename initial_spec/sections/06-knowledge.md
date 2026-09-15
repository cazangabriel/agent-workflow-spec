## 6. Knowledge model

Agents have two distinct, first-class knowledge types:

1. **Skills** — what they know how to do, and how well.
2. **Problem context** — how many things they know about a problem or feature before they start.

Ideal: reduce unknowns and uncertainty toward zero before implementation. Do not implement against assumptions.

### 6.1 Skill loading

When an agent is spawned, it learns base skills from a `.md` file dedicated to that agent type (`DesignArchitect-warm.md`, `KanbanMaster-cold.md`, `Worker-cold.md`).

After base skills:

- The agent is pointed at the next ticket, or
- The agent offers a short list of potential next tickets that were already ordered / prioritised, so the human can choose.

### 6.2 Problem-context loading (living-knowledge)

App knowledge lives in a series of markdown files under the repo-relative path `/app-name-1/living-knowledge/` (formerly drafted as `/app-name-1/living-context/`).

The **project design spec** is a distinct artifact next to that tree (proposal: `/app-name-1/living-knowledge/project-design-spec.md`, or a small set `project-design-spec-*.md`). It is not a ticket and it is not a module gotcha file. Only DesignArchitect-warm and the human write it. KanbanMaster-cold reads it to cut tickets.

`app-name-1` is a stand-in for the actual application / repo project directory name. Each software project the team builds gets its own such tree. Paths in this spec that start with `/app-name-1/` are repo-relative, not host-absolute.

Properties required of this store:

- Files are pushed to git as soon as they are modified.
- Workers do not write these files. KanbanMaster-cold writes module living-knowledge. DesignArchitect-warm writes the design spec and may edit living-knowledge when a TA/design change requires it. Human may edit either.
- Files must be well differentiated and encapsulated, with high cohesion and low coupling among themselves.
- Avoid “microservice hell”: not 30 `.md` files that refer to each other until the dependency graph is a spider web or a connected graph.
- **Source of truth:** living-knowledge files (plus ticket files and this spec), not an agent’s session memory. Session memory is a cache. If a decision matters after the session ends, it is written to git-backed markdown. Quality of overview files is a first-class requirement, because later agents — including a reset warm agent or a cold agent — are only as good as those files.
- Resulting context must be:
  - **AI-model-account independent:** any AI model can spawn the intended agent types; agents pick up work because skills and required app-context are in files, not in a vendor-specific memory blob.
  - **Machine independent:** commit all context files to git so work restores on another machine if the current dev machine is unavailable or changed.

### 6.3 File types inside living-knowledge

Two granularities per module+layer.

**A. General overview files, one per module+layer**

Example given: 2 modules × 3 layers each → 6 overview files.

Audience: DesignArchitect-warm or KanbanMaster-cold who needs a good overview of that area. That audience also defines the level of detail: overview, not expert dump.

**B. Detailed module files, one per module+layer**

Loaded by workers when a ticket touches that area, or a cross-cutting concern appears with another module+layer than the ticket originally named.

Content that is worth storing in the **body** of a living-knowledge file (keep this list; it is the content policy):

- General technical decisions for that module+layer.
- Conventions.
- Integrations with other layers and/or modules and/or APIs.
- Principles used across the module.
- Design / architectural priorities that apply here (including the project’s chosen “-ilities,” e.g. maintainability).
- Preferences for which principles to use or prioritise in code.
- Gotchas, warnings, dangers, module-specific pitfalls.
- What to avoid, and what to do instead.
- Technologies or usage styles to prefer, and which to avoid (work *with* the stack).
- Things that need special attention.
- Traps; temptations to resist, and why.
- Preferred ways of working.
- Preferred directions for the code.

Content that is not wanted by default:

- Trivial member lists.
- Blind method documentation in the style of javadocs.
- A copy-of-the-code-in-written-words.
- Exception: if a method is key or tricky, it may be mentioned.

Size constraint: detail files must not be so large that a brand-new worker loading 3 or 4 of them becomes a “context beast” costing about 30k tokens per prompt.

### 6.4 Transient extra skills and extra app-context during a ticket

If the ticket implies work on a certain layer, the worker reads the living-knowledge file for that area.

If additional transient skills would be useful for the ticket, the worker adds those skills for that session / ticket only, and leaves a comment on the ticket naming the extra skills used.

KanbanMaster-cold periodically reads finished tickets. If workers consistently need certain skills for tickets in a certain area, consider adding those skills as defaults to the area living-knowledge file, in the form: “if you are going to work on this area, acquire additional skills A, B, C.”

### 6.5 Cross-cutting discovery mid-ticket (keep the full scenario)

A worker may discover that the ticket has a cross-cutting concern with another app layer or with another module’s specific layer. Then the worker must research that other layer’s living-knowledge file.

Scenario given in the brief (do not drop parts):

- Project has 3 large modules.
- Each module has: DB, Model, business-logic services, 3rd-party API integration services, local API controllers, and UX layers.
- Worker starts a Module 1 ticket that touches Model and business-logic layers, so it acquires app-context for those 2 layers.
- During work it discovers further changes needed in Module 2 in: 3rd-party API layer, Model layer, and local API controllers. Those Module 2 changes were not foreseen in the ticket.
- Worker must learn app-context for those Module 2 layers so it can do the cross-work integration with complete required context, without assumptions or cutting corners, and without needing to know everything about all parts of the app.
- Worker leaves comments on the ticket detailing extra work discovered and extra app-context acquired.
- KanbanMaster-cold observes and may conclude: something is wrong; there is tight coupling; or this is expected. If it is a design-level coupling problem, write a TA ticket rather than shrinking it into a cute follow-on ticket.

After the ticket, the worker session ends (unless §5.11 reuse applies); the extra context is not kept as a standing whole-app brain.
