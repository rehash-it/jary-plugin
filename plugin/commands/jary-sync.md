---
description: Sync this repo into Jary (backlog, tasks, QA plan, or idea refinement) via the Jary MCP server.
argument-hint: "[backlog | qa-plan | task <desc> | feature-done <name> | idea]"
---

Use the **jary-sync** agent to sync the current repository into Jary.

Intent (from `$ARGUMENTS`, ask if empty):

- `backlog` — analyze the codebase and backfill missing features (deduped).
- `qa-plan` — analyze the codebase and import an end-to-end QA plan
  (epics → features → tests → bugs) via `import_qa_plan`.
- `task <description>` — create a tracked task (or several) for the work
  being discussed.
- `feature-done <name>` — mark the feature done and persist the test suite
  that now exists in code.
- `idea` — capture/continue a refined idea (dimensions + competitors), then
  optionally promote it to a project.

Before any large write, show a short preview and get confirmation. Report a
concise created/skipped/failed summary at the end.
