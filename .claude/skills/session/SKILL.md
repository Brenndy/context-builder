# /session

Manage Claude Code sessions associated with projects.

Support listing sessions, showing status, registering/linking sessions, preparing context, spawning when Claude Code tooling supports it, and ingesting sessions.

Sessions may be spawned by Master, started manually by the user, or discovered from local Claude/session artifacts. Never assume Master owns a session.

For a spawned session: identify project/repository, load Project Context, load only relevant Topics/Decisions/Dependencies/Commitments, provide objective and constraints, and keep the session independent.
