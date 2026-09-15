## 4. Ideals that apply to the workflow and to every agent in it

### 4.1 High cohesion and low coupling

This principle must permeate:

- This specification’s structure.
- The later workflow design.
- The agents’ own work (tickets, code, living-knowledge files, communication).

### 4.2 Git competence is not assumed; it is trained

If an agent needs to work with git, that agent must specifically be trained with git skills.

Rules:

- One feature per commit.
- Do not mix unrelated changes.

### 4.3 Commit and push are not optional extras

If an agent works a ticket and is expected to commit and push, it is paramount that the agent does not forget.

Suggested control: every spawned worker gets a small session checklist that includes commit + push.

Why this exists (keep the failure mode in the spec): the agent reports a ticket implemented; the human expects to see it in the app; the change exists only in the agent’s local workspace and was never pushed; the human cannot pull and test. That situation is treated as a serious workflow defect.

### 4.4 Human-approved approach before implementation

Whenever an agent picks up a new task / ticket / feature:

1. Discussion first: agent proposes approach; human approves or changes it.
2. Only after the human approves the approach may implementation start.
3. Implementation follows the agreed approach.
4. Agent commits and pushes (direct commit(s) to `dev`, or a feature branch merged into `dev` when appropriate).
5. Human pulls latest `dev`, starts containers etc. as needed, and is asked to test in the app. The human may decide not to exercise a given ticket, but the workflow still presents the test step. This step exists because the user treats testing as supervision; current AI is not trusted to work unsupervised. Do not add an agent that tests against requirements as a substitute: that path was considered and rejected as too expensive in tokens.
6. If the change works, the ticket is marked `done` per §8.4. `closed` waits for the KanbanMaster-cold ceremony in §8.8. Otherwise work continues until it works and issues are fixed.

Aim of approaches: simplest approaches first, that still lean toward maintainable professional code. No quick-and-dirty code, no hacks, no bypasses, no lazy code. Code must suit current project size and remain maintainable.

### 4.5 Professional maintainable code

Restated because it is both an implementation standard and an approach-selection standard: professional, maintainable code for the current project size; not hacks.

### 4.6 Skills are per artifact, not per job title

Whoever writes artifact X must have the skills X needs. This is the operational form of §0.8 item 1. Skills are loaded from that agent type’s init file (§6.1, §5.10). Missing skill + write permission = defect.

| Artifact | Writer | Required skills (minimum) |
|---|---|---|
| Living-knowledge (gotchas, conventions, module body) | KanbanMaster-cold; DesignArchitect-warm may also edit | Full §6.6 set + stack literacy for this project |
| Project design spec (`project-design-spec*.md`) | **Only** DesignArchitect-warm + human | §6.6 + architectural characteristics work (§5.5.1) + stack choice + component-level order |
| Kanban board file (`kanban.md`) | Generated from ticket files; not hand-maintained | §8.10 |
| New `backlog` ticket | **Only** KanbanMaster-cold | Ticket-cutting, UL, dependency order, read design spec; must not invent structure the design spec does not support |
| New non-TA ticket | **Only** KanbanMaster-cold | Same as backlog. Worker comments describe extra work; they do not create the file (§8.11) |
| TA ticket | **Only** KanbanMaster-cold | Write a self-contained design *problem*; must not propose the design solution |
| Ticket comment | Worker-cold on the ticket they are executing; KanbanMaster-cold on any non-TA; DesignArchitect-warm on TA only | Clear, concise, self-contained; no novels |
| Application code in an area | Worker-cold (after human-approved approach) | Software engineering + domain + that area’s living-knowledge + OOP/OOD/SOLID + pitfalls/anti-patterns + readable names (§7.1) + any transient extra skills noted on the ticket |
| Git commit / push | The agent who changed the tree | Git skill; one feature per commit; message = spawn name + ticket code + human title (§9.2) |
| Containers / local run | Worker-cold when the ticket needs it; human for test | Container skill for that stack |
| Deploy / host on a machine | Only an agent whose ticket says so, and only with deploy/host skills loaded | Those skills + the relevant living-knowledge; do not improvise production |

Do not give an agent write access “because they are senior.” Give it because the row above lists the skill and the permission.
