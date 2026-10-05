# /project

Manage the Master project registry and project lifecycle.

## Commands

Support natural-language project operations such as:
- add/register a project
- list projects
- show project
- update project metadata
- remove/archive a project

When adding a project, require at minimum:
- stable project id/name
- repository path or repository reference

Then use project-context discovery to enrich the registry rather than requiring the user to manually describe the project.

Do not invent project purpose, ownership, architecture, or status. Mark unresolved fields as unknown.

Keep `projects.yaml` as the registry of projects and repositories. Detailed project understanding belongs in project context/state, not in the registry itself.

Preserve existing entries and update incrementally.
