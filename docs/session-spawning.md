# Session spawning

Master may spawn Claude Code sessions, but Claude Code remains the development runtime.

Master tooling provides project/repository selection, Project Context, relevant operational context, objective, constraints and session metadata.

Do not build a second AI runtime or custom LLM driver.

A spawned session is equivalent to a manually started session: independently usable and later ingestible. Exact Claude Code invocation should follow current CLI capabilities rather than being hard-coded into the knowledge model.
