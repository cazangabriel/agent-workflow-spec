## 3. Scenarios to avoid (anti-patterns)

### 3.1 The expensive god-agent

A god-agent is one agent that is expected to know every project detail, with a context that grows without a bound.

Required: do not use an agent with a massive, growing context as the default working unit.
Violation: a single agent loaded with the whole repo, the whole board, and the whole living-knowledge on every prompt.
Example: a worker prompt that pastes every living-knowledge file and every ticket body "for context" is this anti-pattern. The worker loads the ticket and the living-knowledge files that ticket points at.

Concrete failure from the user's history: an agent that consumed about 100k tokens per prompt. That burns through a Super Grok subscription quickly under Grok Build. That figure is from the user's report, not a measured vendor quota.

### 3.2 Too many specialists and a dense communication graph

A dense graph is one where many specialised agents each talk to many others.

Required: keep the number of agents small so the channel count stays small. Agents stay on project, feature, and workflow substance. If a thread becomes long, escalate to the human. Do not add more tokens in the hope it converges. A loop in inter-agent discussion is a failure mode; break it by escalation, not by more talk.
Violation: an agent-to-agent thread that continues after it has repeated the same claim, or a new specialist spawned to "help" a stuck thread.
Example: the worker and a second agent trade three messages on the same column name. The third message is an escalation to the human, not a fourth message to the other agent.
