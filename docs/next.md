# Next implementation steps

1. Add project/repository management tooling around `projects.yaml`.
2. Implement `/project-context` to discover and maintain the purpose, role, architecture, constraints and operational context of each repository.
3. Make repository documentation and guidelines the primary source of project context: `CLAUDE.md`, README, `docs/`, architecture/design docs, contribution guides and ADRs; use code/configuration/tests as validation.
4. Implement source adapters for Claude session events and transcripts.
5. Implement full file schemas for Topic, Commitment, Dependency, Decision, Evidence and Project Context.
6. Add persistent indexes for fast relation lookup.
7. Add incremental sync cursors and documentation fingerprints so context refreshes are incremental.
8. Implement `/ingest-session` with Claude.
9. Implement `/ingest-meeting` with Claude.
10. Add stale/archival review.
11. Add daily-review.
12. Add Jira adapter.
13. Add cross-repo Git adapter.
14. Add Claude Code session spawning/tooling so Master can start work in the appropriate repository with the right project context.
15. Add optional reminders/notifications.
16. Add GUI on top of the same state/tooling layer.

## Project context rule

A repository is not understood merely by scanning its source tree. Master should first learn the project's documented intent and operating rules, then use implementation evidence to validate and enrich that understanding.

Keep the first implementation local and file-first.
