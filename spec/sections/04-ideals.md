## 4. Ideals that apply to the workflow and to every agent in it

### 4.1 High cohesion and low coupling

High cohesion: related rules live in the same section. Low coupling: a topic is not scattered across seven places without a reason.
Required: a rule about tickets lives in §8. A rule about git lives in §9. A cross-cutting rule lives in one place and other sections point at it.
Violation: the same constraint stated differently in two sections, so a cold agent can obey one and break the other.
Example: "no feature branch" is stated in §9.1. §4.4 points at §9.1. It does not restate a different branch rule.

### 4.2 Git competence is not assumed; it is trained

If an agent needs git, that agent must be trained with git skills. One feature per commit. Do not mix unrelated changes.
Violation: an agent that commits without a git skill loaded, or a commit that mixes two tickets.
Example: the worker loads the git skill before Step 7 of §7. The commit contains only AM-10.

### 4.3 Commit and push are not optional extras

If an agent is expected to commit and push, forgetting is a workflow defect: the human cannot pull `dev` and test.
Violation: "done" with no commit, or a commit with no push.
Example: the worker writes the migration, commits, and pushes to `dev` before it moves the ticket to `in-testing`. It does not say "ready" while the commit is only local.

### 4.4 Human-approved approach before implementation

1. The agent proposes an approach. The human approves or changes it.
2. Implementation starts only after that.
3. Implementation follows the agreed approach.
4. The agent commits and pushes directly to `dev`. No feature branch (§9.1).
5. The worker moves the ticket to `in-testing`. The human pulls `dev` and tests or gut-skips. There is no agent-tester substitute.
6. The human signals the ticket is done. The signal is the human saying the test passed, gut-skipping, or saying to consider the ticket done. Silence is not a signal. Worker-cold then moves `in-testing` → `done`. Worker-cold does not move to `closed`. `closed` waits for the KanbanMaster-cold ceremony (§8.8).

Prefer the simplest approach that still produces maintainable professional code. No hacks.

The 15–20% band (§5.8) applies to DesignArchitect-warm and KanbanMaster-cold. It does not apply to Worker-cold. A worker ticket that looks that large is a split request (§8.14).
Violation: writing code before the human agrees the approach.
Example: the worker proposes "one migration, `cook_time integer not null default 0`." It stops. It does not write the file until the human agrees.

### 4.5 Professional maintainable code

Professional, maintainable code for the current project size. Not hacks.
Violation: a one-off that the worker cannot explain against §7.1.
Example: a raw SQL string concatenated in a controller is a hack. The worker does not ship it.

### 4.6 Skills are per artifact, not per job title

Whoever writes artifact X must have the skills X needs. Do not give write access because a role sounds senior.

| Artifact | Writer | Required skills (minimum) |
|---|---|---|
| Living-knowledge (gotchas, conventions, module body) | KanbanMaster-cold; DesignArchitect-warm may also edit | Full §6.6 set + stack literacy |
| Project design spec | Only DesignArchitect-warm + human | §6.6 + -ilities (§5.5.1) + stack + component order |
| Per-project ticket-type abbreviation list | Only DesignArchitect-warm + human | UL + module/layer map (§6.8) |
| Kanban board file (`kanban.md`) | Generated | §8.10 |
| New `backlog` / non-TA ticket | Only KanbanMaster-cold | Ticket-cutting, UL, the project type-abbrev list |
| TA ticket | Only KanbanMaster-cold | Self-contained design problem; no solution; never "see F-n" |
| Ticket comment | Worker on their ticket; KM on non-TA; DA on TA | Clear, self-contained |
| Application code | Worker-cold after approved approach | SE + domain + area living-knowledge + §7.1 |
| Git commit / push to `dev` | The agent who changed the tree | Git skill; §9 |
| Containers / local run | Worker when the ticket needs it; human for test | Container skill |
| Deploy / host | Only if the ticket says so, with those skills loaded | Those skills + living-knowledge |

Violation: a worker writing living-knowledge, or a KanbanMaster inventing a module abbreviation.
Example: the worker hits a missing unit. It comments and stops. It does not edit `recipes.md`.
