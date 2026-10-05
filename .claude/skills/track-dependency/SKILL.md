# /track-dependency

Create and update prerequisite relationships.

When A requires B:
A -> depends_on -> B
B -> blocks -> A

Track:
- prerequisite
- affected topic
- owner
- status
- due_at when known
- evidence
- created_at
- resolved_at

When a prerequisite changes, inspect all affected downstream topics.
