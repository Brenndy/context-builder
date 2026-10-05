# Domain model

Core entities:

- Project
- Repository
- ClaudeSession
- Topic
- Decision
- Commitment
- Dependency
- Person
- Evidence
- Risk
- OpenLoop

The most important object is Topic.

Relationships include:

- topic -> related_topic
- topic -> linked_project
- topic -> linked_repo
- topic -> blocked_by -> dependency
- dependency -> owned_by -> person
- commitment -> owned_by -> person
- commitment -> related_topic
- evidence -> supports -> entity
- decision -> affects -> topic
- ClaudeSession -> produced -> evidence

Temporal fields:

- created_at
- first_seen
- last_activity_at
- last_meaningful_change
- resolved_at

Statuses should be evidence-based and may be uncertain.
