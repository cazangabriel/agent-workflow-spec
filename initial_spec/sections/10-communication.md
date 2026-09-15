## 10. Communication rules

- Prefer few agents over many specialists, to keep the channel count small.
- Keep talk on project / feature / workflow substance.
- Do not run unbounded agent-to-agent threads.
- If a thread becomes long, escalate to the human; do not hope more tokens will converge it.
- Treat looping discussions as a failure mode requiring escalation.
- Shared-vocabulary risk between skillsets is in-scope (§2.4).
- Questions and statements to the human must carry their own context (§0.7).
- One worker at a time (§5.3), so worker-to-worker channels are out of scope until that constraint is lifted.

Human is in the loop for:

- requirements and planning with DesignArchitect-warm;
- invoking KanbanMaster-cold ceremonies and spawn triggers;
- choosing the next ticket from `open` (worker may suggest 4–5 from that list);
- approving or altering implementation approach;
- testing after pull;
- receiving stop-rule reports from the worker and spawning KanbanMaster-cold (and DesignArchitect-warm via TA if needed);
- long-thread escalation;
- important choices and expensive-change estimates;
- living-knowledge improvements an agent escalates.
