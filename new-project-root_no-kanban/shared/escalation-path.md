# Escalation path

When a worker hits the stop-rule, the path is:

1. The worker comments on the feature and tells the human. The comment names the missing fact.
2. The human spawns DesignArchitect-warm if the gap is design-level.
3. DesignArchitect-warm writes the solution into the design spec or living-knowledge.
4. The worker can then continue.

The worker does not invent the missing rule.
DesignArchitect-warm does not invent a feature. It writes the solution for the design gap it is given.

Violation: the worker inventing minutes. DesignArchitect-warm cutting a feature file.
