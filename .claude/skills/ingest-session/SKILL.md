# /ingest-session

Ingest incremental information from a Claude Code session.

Prefer structured session events when available.

Useful event types:
- finding
- change
- decision
- status
- test
- blocker
- commitment

If structured events are unavailable, use the session transcript/log as enrichment.

Do not assume a session was started by the Master. Independently started sessions are valid sources.

After extracting meaningful events, run /reconcile.
