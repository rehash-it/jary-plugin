# Connect Jary to Claude

Jary exposes an MCP server so Claude can read and write your Jary
projects — refine ideas, log features and tasks, push a full QA plan —
without leaving your editor or chat.

> Replace `https://api.jary.dev` below with your actual Jary URL if
> different.

You connect once; Claude runs a normal sign-in (OAuth) in your browser and
everything you do is scoped to **your** workspace automatically. No API keys
to copy, no secrets to paste.

---

## Option A — Claude Code (the plugin)

Best experience: adds a `/jary-sync` command and a guided workflow agent.

```text
/plugin marketplace add <your-org>/<jary-plugin-repo>
/plugin install jary@jary
```

Then, from inside the repository you want to sync:

```text
/jary-sync backlog        # backfill features from the codebase
/jary-sync qa-plan        # import an end-to-end QA plan
/jary-sync idea           # capture / continue a refined idea
/jary-sync feature-done <name>
```

The first run opens a browser for Jary sign-in. Done.

### Pointing at a different Jary instance

The plugin defaults to `https://api.jary.dev`. To use another instance
(self-hosted, staging, local dev), set an environment variable before
starting Claude Code:

```bash
export JARY_MCP_URL="https://your-jary-host/mcp/messages"
```

---

## Option B — Claude.ai (web or desktop)

Claude.ai doesn't use plugins, but it can connect to the same server as a
**custom connector**.

1. Claude.ai → **Settings → Connectors → Add custom connector**.
2. URL:
   ```
   https://api.jary.dev/mcp/messages
   ```
3. Save, then click **Connect** and sign in to Jary when prompted.

You get all Jary tools (you won't have the `/jary-sync` shortcut — just
ask Claude in plain language, e.g. *"add these as features in Jary"*).

---

## What Claude can do once connected

| Area | Examples |
|------|----------|
| Ideas | Start/continue an idea session, set maturity dimensions, save competitors |
| Backlog | Create features (single or bulk), update status, promote an idea to a project |
| Tasks | Create implementation tasks linked to features/bugs |
| QA | Import a whole epic → feature → test → bug plan in one step |

Everything is permission-scoped: a read-only sign-in cannot write, and you
only ever see your own workspace's data.

---

## Troubleshooting

- **"Forbidden / missing scope"** — your Jary sign-in didn't grant that
  capability. Reconnect and accept the requested permissions.
- **Sign-in loop / connector won't connect** — the server URL must match
  Jary's configured public address exactly (scheme + host, no trailing
  slash). Confirm you used `https://` and the correct host.
- **Nothing happens in Claude Code** — confirm the plugin installed
  (`/plugin`) and that you're running the command from inside a project
  directory.
