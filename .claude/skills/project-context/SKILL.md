# /project-context

Build and maintain the operational context of a project from the repository itself.

## Primary principle

The repository's existing documentation and guidelines are the primary source of truth for understanding why the project exists, what it does, how it is structured, how it should be changed, and what constraints apply.

Do not infer the project's purpose primarily from filenames or source code when authoritative documentation exists.

## Source priority

Inspect sources in roughly this order:

1. `CLAUDE.md` and nested `CLAUDE.md` files
2. project README and repository documentation
3. `docs/` and other explicitly documented project guides
4. architecture/design documents
5. contribution/development guidelines
6. ADRs and decision records
7. package/application metadata and configuration
8. source code, tests, infrastructure and deployment configuration as validation/evidence

Follow links between documents when they are relevant.

## Extract

Build a concise project context containing, where supported by evidence:

- project purpose / why it exists
- business or product problem it addresses
- users or consumers
- major capabilities
- system boundaries
- important dependencies and integrations
- architecture and major components
- runtime/deployment model
- data ownership and important flows
- conventions and engineering constraints
- testing/quality expectations
- operational concerns
- known risks and limitations
- important terminology and aliases
- related repositories/projects
- key documentation sources
- unresolved/unknown areas

Separate documented facts from interpretations.

## Evidence and confidence

Every material claim should have source evidence.

Prefer explicit statements from project documentation over inference from code.

Use confidence levels:
- high: directly documented or strongly corroborated
- medium: supported by multiple repository signals
- low: inferred from implementation without clear documentation

Never turn suggestions, TODOs, examples, or speculative language into facts or decisions.

## Reconciliation

Compare the discovered context with the existing Master project context.

- Preserve existing knowledge that remains supported.
- Add newly discovered facts.
- Update superseded facts while preserving evidence/history.
- Detect contradictions instead of silently overwriting them.
- Record documentation sources and relevant paths.
- Flag when code appears inconsistent with documented intent.

## Incremental behavior

Do not reread the entire repository on every run.

Track relevant source paths and content/change fingerprints where possible. Reprocess changed documentation first, then inspect affected code/configuration only when needed.

A project-context refresh should be safe to run repeatedly.

## Output

Produce/update a canonical project context that Master can use when:
- correlating topics across projects
- deciding which repository owns a topic
- spawning a Claude Code session
- delegating work
- answering project-status questions
- interpreting findings from a repo session

The context should explain not only what the repository contains, but why the project exists and what role it plays in the wider system.
