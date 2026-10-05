# Context Builder / Master

Personal Chief of Staff over Claude Code, multiple repositories and multiple independent Claude sessions.

## Goal

Maintain a canonical operational model connecting topics, decisions, commitments, dependencies, people and evidence across:

- many repositories
- many Claude Code sessions
- meetings and transcripts
- Jira
- Git
- ad-hoc notes and user messages

Claude Code is the intended reasoning engine. This repository contains the state model, rules, skills and orchestration layer around Claude.

## Principles

1. Topic is the primary work object, not a Jira ticket.
2. New information must be reconciled with existing state.
3. Evidence is preserved; contradictions are not silently overwritten.
4. Ideas, hypotheses, facts, decisions and commitments remain distinct.
5. Dependencies are first-class objects.
6. Time and activity determine lifecycle; old does not automatically mean irrelevant.
7. Multiple repositories and multiple independent Claude sessions are first-class.
8. Background ingestion is incremental.
9. External side effects require explicit authorization.

See `docs/architecture.md` and `docs/domain-model.md`.
