# /reconcile

Reconcile new information against the canonical state.

## Procedure

1. Extract claims, entities, people, projects, repositories, dates and actions.
2. Classify each claim as FACT, DECISION, COMMITMENT, TASK, DEPENDENCY, HYPOTHESIS, IDEA, OBSERVATION, RISK or OPEN_LOOP.
3. Find candidate existing entities.
4. Resolve entity identity using IDs, aliases, projects, repositories, people, keywords, semantic meaning, dependencies and history.
5. Decide whether the new information creates, enriches, updates, resolves, reopens or conflicts with existing state.
6. Update relationships and temporal fields.
7. Check downstream dependency consequences.
8. Preserve evidence and uncertainty.
9. Do not invent facts or relationships.

The key question is:

"Does this information change the current model of reality?"

Possible outcomes:
- NO_CHANGE
- ENRICH_EXISTING
- UPDATE_STATUS
- CREATE_RELATIONSHIP
- CREATE_DEPENDENCY
- RESOLVE_DEPENDENCY
- CREATE_COMMITMENT
- RESOLVE_COMMITMENT
- CREATE_CONFLICT
- SUPERSEDE_PREVIOUS_INFORMATION
- REOPEN_TOPIC
- CREATE_NEW_TOPIC
