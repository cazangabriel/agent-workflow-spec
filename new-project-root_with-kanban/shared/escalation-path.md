# Escalation path

When a worker hits the stop-rule, the path is:

1. The worker comments on the ticket and tells the human. The comment names the missing fact.
2. The human spawns KanbanMaster-cold.
3. KanbanMaster-cold answers the block if it can. If it cannot, it writes a TA ticket. The TA body is the problem. It does not contain the solution.
4. The human spawns DesignArchitect-warm on that TA. DesignArchitect-warm writes the solution into the design spec or living-knowledge, then comments that solution on the TA.
5. The worker can then continue.

The worker does not spawn anyone. The worker does not invent the missing rule.
KanbanMaster-cold does not invent the solution when it cannot answer. It writes the TA.
DesignArchitect-warm does not invent a ticket. It writes the solution for the TA it is given.

Violation: the worker inventing minutes, or spawning KanbanMaster-cold itself. KanbanMaster-cold writing a solution inside a TA body. DesignArchitect-warm cutting an F ticket.
