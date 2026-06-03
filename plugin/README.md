# Jary Claude Code plugin

Bundles the Jary MCP connection plus a `/jary-sync` command and a
`jary-sync` subagent so a developer can sync their codebase into Jary
(backlog, tasks, end-to-end QA plan, idea refinement) without leaving the
editor.

## What's inside

| File | Purpose |
|------|---------|
| `.mcp.json` | Connects the `jary` MCP server (Streamable HTTP + OAuth) |
| `commands/jary-sync.md` | `/jary-sync` slash command |
| `agents/jary-sync.md` | Subagent encoding the resolve→dedupe→write workflow |

> **End users:** see [CONNECT.md](./CONNECT.md) for the user-facing
> connect guide (Claude Code plugin **and** Claude.ai connector). This
> README is for maintainers of the plugin.

## Configuration

The MCP endpoint defaults to `https://api.jary.dev/mcp/messages`.
Override for local dev / staging / self-host:

```bash
export JARY_MCP_URL="http://localhost:8105/mcp/messages"   # local dev
```

On first use, Claude Code runs the OAuth 2.1 flow (browser consent) against
the Jary OIDC provider. The token's scopes gate which tools work:
`projects:*`, `features:*`, `tests:*`, `tasks:write`, `qa:write`,
`ideas:*`, `competitors:*`.

## Install (private — nothing is published)

This plugin ships inside the (private) monorepo. It is **not** hosted or
made public. Install it straight from your local checkout:

```
/plugin marketplace add ./apps/jary/claude-plugin
/plugin install jary@jary
```

Teammates with monorepo access can alternatively point
`/plugin marketplace add` at the private git URL — Claude Code clones it
using their existing git credentials; the repo stays private.

Then run `/jary-sync backlog` (or `qa-plan`, `idea`, …) from inside the
repository you want to sync.

## Distributing to external users (monorepo stays private)

External users never get the monorepo. Publish only this plugin directory
to a small standalone repo (it contains no proprietary code — a manifest,
an MCP URL, a command, an agent prompt):

```bash
# 1. Clone the (separate) public distribution repo somewhere
# 2. Mirror the plugin into it (syncs only — never commits or pushes):
make publish-plugin DIR=/path/to/cloned/jary-plugin-repo
# 3. Review the diff in that repo, then commit & push it yourself
```

Authoring stays here (single source of truth); the distribution repo is a
generated mirror. Users then install via
`/plugin marketplace add <your-org>/<distribution-repo>`. See
[CONNECT.md](./CONNECT.md).

## Prerequisite

The Jary API (`jary-api`, port 8105) must be reachable and its OIDC
provider configured. For a real Claude.ai / hosted connection the server
must be exposed over public HTTPS (project Phase 0).
