## 4. Ideals that apply to the workflow and to every agent in it

### 4.1 High cohesion and low coupling

This principle must permeate this specification, the later workflow, and the agents’ own work.

### 4.2 Git competence is not assumed; it is trained

If an agent needs git, that agent must be trained with git skills. One feature per commit. Do not mix unrelated changes.

### 4.3 Commit and push are not optional extras

If an agent is expected to commit and push, forgetting is a serious workflow defect: the human cannot pull `dev` and test.

### 4.4 Human-approved approach before implementation

1. Agent proposes approach; human approves or changes it.
2. Implementation starts only after that.
3. Implementation follows the agreed approach.
4. Agent commits and pushes **directly to `dev`**. No feature branch (§9.1).
5. Worker moves the ticket to `in-testing`. Human pulls `dev` and tests (or gut-skips). No agent-tester substitute.
6. Human moves `in-testing` → `done` on pass. `closed` waits for the KanbanMaster-cold ceremony (§8.8).

Prefer simplest approaches that still produce maintainable professional code. No hacks.

If the agent believes the work would consume on the order of 15–20% of the weekly planning budget (§5.8), it stops and asks before doing it.

### 4.5 Professional maintainable code

Professional, maintainable code for the current project size; not hacks.

### 4.6 Skills are per artifact, not per job title

Whoever writes artifact X must have the skills X needs.

| Artifact | Writer | Required skills (minimum) |
|---|---|---|
| Living-knowledge (gotchas, conventions, module body) | KanbanMaster-cold; DesignArchitect-warm may also edit | Full §6.6 set + stack literacy |
| Project design spec | **Only** DesignArchitect-warm + human | §6.6 + -ilities (§5.5.1) + stack + component order |
| Per-project ticket-type abbreviation list | **Only** DesignArchitect-warm + human | UL + module/layer map (§6.8) |
| Kanban board file (`kanban.md`) | Generated | §8.10 |
| New `backlog` / non-TA ticket | **Only** KanbanMaster-cold | Ticket-cutting, UL, the project type-abbrev list |
| TA ticket | **Only** KanbanMaster-cold | Self-contained design *problem*; no solution; never “see F-n” |
| Ticket comment | Worker on their ticket; KM on non-TA; DA on TA | Clear, self-contained |
| Application code | Worker-cold after approved approach | SE + domain + area living-knowledge + §7.1 |
| Git commit / push to `dev` | The agent who changed the tree | Git skill; §9 |
| Containers / local run | Worker when the ticket needs it; human for test | Container skill |
| Deploy / host | Only if the ticket says so, with those skills loaded | Those skills + living-knowledge |

Do not give write access “because they are senior.”
