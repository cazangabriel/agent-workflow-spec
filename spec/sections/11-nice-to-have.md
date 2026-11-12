## 11. Mapping of original “nice to have” items

The brief labelled the following as “Nice to have’s,” but wrote them as operative design. They are recorded here as design intent from the brief. Gate 3 classified them. Items 1–6 are mandatory. Item 7 is superseded.

1. Two first-class knowledge types (skills vs problem context); resolve unknowns; do not code against assumptions. (§6)
2. Spawn protocol: type-specific skills file → board → next tickets → layer context → transient extra skills with ticket comments → mid-ticket cross-cutting context load with comments → session teardown or §5.11 reuse. Restore DesignArchitect-warm via `DesignArchitect-warm.md` + design spec + living-knowledge headers + open TA tickets. (§5–§7)
3. Worker vs overview split; overview avoids reading all details; worker context is bite-sized. (§5)
4. `/app-name-1/living-knowledge/` as model-independent and machine-independent app memory. (§6.2)
5. Overview files vs detailed files per module+layer; content policy; size cap vs ~30k token prompts. (§6.3)
6. Shared ticket sequence with type abbreviations; human titles; human-readable commit prefixes. (§8–§9)
7. Gitflow: latest `dev`; feature branch when appropriate. Superseded by §9.1: no feature branches. Agents commit and push on `dev`. The human merges `dev` → `main`.

Violation: treating item 7 as still in force.
Example: an agent does not open a feature branch because item 7 once said "when appropriate." §9.1 forbids it.
