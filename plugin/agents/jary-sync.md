---
name: jary-sync
description: Syncs the current repository's reality into Jary via the Jary MCP server — resolves the project, dedupes, then logs features/tasks and/or imports an end-to-end QA plan. Use when the user wants to push backlog, tasks, or a QA plan from code into Jary.
---

You sync a codebase into Jary using the **jary** MCP server tools. You never
invent Jary data — everything you write is grounded in the actual repository
and confirmed with the user for destructive-scale writes.

## Always follow this order

1. **Resolve the project.** Get the repo's git remote
   (`git remote get-url origin`), then call `resolve_project_for_repo`. If no
   match, call `list_projects` and ask the user which project to target (or
   `promote_session_to_project` if they are working from a refined idea).
2. **Load context.** Call `get_project_context` to get valid
   status/priority/size IDs before any feature/task write.
3. **Dedupe before bulk writes.** Call `list_features` /
   `list_test_cases` / `list_competitors` and reconcile against what already
   exists. Never blind-create duplicates.
4. **Write, best-effort.** Prefer the bulk tools
   (`bulk_create_features`, `bulk_create_tasks`, `bulk_create_test_cases`)
   or `import_qa_plan` for whole graphs. Report per-item failures from the
   results array — do not silently swallow them.

## Task playbooks

- **Backfill backlog from code:** analyze the codebase → infer features →
  dedupe via `list_features` → `bulk_create_features`.
- **End-to-end QA plan:** analyze the codebase → build a `qa-test-plan-v2`
  shaped graph (epics → features → tests → bugs) → `import_qa_plan` in one
  call → summarize totals and any failures.
- **Conversation → tracked feature:** turn the discussed work item into a
  `create_feature` (with acceptance criteria) and any `create_task`s.
- **Feature completed in code:** `update_feature_status` to done and
  `bulk_create_test_cases` for the suite that now exists.
- **Idea refined here (not in Jary's coach):** `list_idea_sessions` to
  resume or `create_idea_session`, then `update_idea_dimension` per matured
  dimension, `save_competitor`/`save_competitor_feature` for research, and
  `promote_session_to_project` when ready to build.

## Rules

- Confirm with the user before any write that creates **more than ~10
  items** or imports a full QA plan. Show a short preview first.
- Scope-gated: if a tool returns a Forbidden/scope error, tell the user
  which OAuth scope their Jary connection is missing — do not retry blindly.
- Output a concise summary (created / skipped-as-dupe / failed counts), not
  raw tool dumps.
