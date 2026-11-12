# agent-workflow-spec

## What this is

This is a self-contained team of agents that can handle the building of a project.
The agents have been designed for sequential work; they have not been designed for work in parallel.

There are 3 agents for the project itself and one agent for tweaking all other agents and the workflow itself
 
Worker-cold agent, implementer of tickets. Session should be reset before every task or cluster of related tasks to keep token usage down.
Discussions can digress.
https://github.com/cazangabriel/agent-workflow-spec/blob/main/new-project-root/app-name-1/human-guides/Worker-cold.md

DesignArchitect-warm agent, handles product business and technical requirements. Session should be kept alive throughout the project lifetime to keep architecture accuracy high.
Do not digress discussion to avoid context slime buildup.
https://github.com/cazangabriel/agent-workflow-spec/blob/main/new-project-root/app-name-1/human-guides/DesignArchitect-warm.md

KanbanMaster-cold agent, splits tickets and manages a kanban board. Session should be reset before every new kanban ceremony to avoid context slime buildup from ceremonies.
Discussions can digress.
https://github.com/cazangabriel/agent-workflow-spec/blob/main/new-project-root/app-name-1/human-guides/KanbanMaster-cold.md

WorkflowSteward-warm agent can make changes on the worklflow itself and alter the init prompts of all other agents. Session should be kept alive throughout the project.
Do not digress discussion to avoid context slime buildup.
https://github.com/cazangabriel/agent-workflow-spec/blob/main/new-project-root/app-name-1/human-guides/WorkflowSteward-warm.md


## How the 3 project agents work

The project is initially designed by the user along with the DesignArchitect agent (technologies along with product requirements), that spec is written under /living-knowledge/

Once the spec is built, the developmnent phase can start:
The user does performs periodic kanban sessions along with the KanbanMaster agent to create new work tickets,
The user instructs the Worker agent to work on tickets, sequentially, the worker should know which tickets are best suited to work on next.



## How to get started

https://github.com/cazangabriel/agent-workflow-spec/blob/main/new-project-root/app-name-1/human-guides/project-flow.md#start-a-product-repo--before-phase-1

##