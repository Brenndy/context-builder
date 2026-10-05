# Architecture

## Runtime model

Claude is the intended reasoning engine for the Master.

The repository should not hard-code business logic into a Python LLM client. Instead:

- the local file state is canonical
- skills define reasoning procedures
- Claude Code executes those procedures
- a background scheduler invokes non-interactive ingestion/reconciliation
- the CLI is a thin interface for humans and automation

Target runtime:

```
scheduler / user
      |
      v
  master CLI
      |
      v
 Claude Code
      |
      +--> .claude/skills/*
      |
      +--> state/*
      |
      +--> sources/*
      |
      +--> projects.yaml
```

## Multiple repositories

Registered projects point at independent repositories.

Claude sessions are independent execution units. There may be many sessions per repository, and sessions can be started either by the Master or manually by the user.

The ingestion layer must therefore discover and process session deltas without assuming ownership or a single active session.

## Background worker

The initial worker should use a cheap change detector and only invoke Claude when useful work exists.

```
scheduler
  -> detect changes
  -> collect delta
  -> invoke Claude
  -> run /ingest-session or /reconcile
  -> persist canonical state
```

Do not launch a full interactive Master session on every scheduler tick.

## Future GUI

A graphical client can later sit above the same CLI/state interfaces. The domain model must remain independent from presentation.

## External side effects

Reading and local knowledge updates may be automated. External writes remain explicit/authorized.
