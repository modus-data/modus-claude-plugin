# Modus plugin for Claude

Connect Claude to your organization's **Modus context layer**. Once installed, Claude can:

- **Discover and chat with published skills** and **workflows**, and pull org context (`modus-context`).
- **Manage** skills, workflows, and context items for admins (`modus-manage`).

The plugin points at **one unified Modus MCP** (`mcp.getmodus.com/modus`), exposed as **two OAuth-secured connections** — a read/invoke `modus-context` and an admin `modus-manage` — plus a guidance skill (`use-modus`) that teaches Claude when and how to use Modus. The two entries hit the same endpoint and differ only by the `toolsets` they expose.

| Server | What it does | URL (production) |
| --- | --- | --- |
| `modus-context` | Skills, workflows, context compose, skill integration tools | `https://mcp.getmodus.com/modus?toolsets=context` |
| `modus-manage` | Create / update / deploy / delete skills, workflows, context | `https://mcp.getmodus.com/modus?toolsets=manage` |

You **pick your organization when you sign in** — one install works for every org you belong to. To switch org, reconnect and pick a different one (see [Switching org](#switching-org)).

---

## Requirements

- A **Modus account** with access to at least one organization.
- **Claude Code** (CLI) or the **Claude Desktop** app (Personal plugins marketplace).
- Nothing to host — Modus runs the server; the plugin only points at it.

---

## Install in Claude Code

```bash
# 1. Add the Modus marketplace (once)
/plugin marketplace add modus-data/modus-claude-plugin

# 2. Install the plugin
/plugin install modus@modus
```

Then **authenticate** (see [Sign in](#sign-in-and-pick-your-org)).

> **Local dev** (from a Modus monorepo checkout): `pnpm claude-plugin:sync --out /tmp/modus-claude-plugin`, then `/plugin marketplace add /tmp/modus-claude-plugin`.

---

## Install in Claude Desktop

1. Open **Customize** (left sidebar).
2. **Personal plugins** → **+** → **Add marketplace**.
3. Enter `modus-data/modus-claude-plugin` (or `https://github.com/modus-data/modus-claude-plugin`) → **Sync**.
4. Install **modus** from the list → authenticate each MCP server (see below).

> **Chat vs Code tab:** plugin installs in Desktop chat are separate from the in-app **Code** tab — install in both if you use both.

---

## Sign in and pick your org

Run `/mcp` (Claude Code) or complete the connect flow in Desktop. Select each Modus server and **Authenticate**. A browser opens for Modus sign-in; OAuth is discovered automatically (RFC 9728).

During consent you **pick which organization** the connection is for. The token is bound to that org.

> Authenticate **both** entries. **Pick the same org for each** so context and management operate on the same data. Both connect to the same Modus MCP, so a single sign-in covers them.

### Switching org

Reconnect and re-pick:

```bash
# Claude Code
/mcp   # select server → re-authenticate
```

In Desktop: disconnect the server and reconnect. With `mcp-remote` (fallback below): `rm -rf ~/.mcp-auth` then reconnect.

---

## Fallback: manual Claude Desktop MCP config

If Personal plugins are unavailable, add servers to `claude_desktop_config.json` (**Settings → Developer → Edit Config**).

### Native remote MCP (newer Desktop)

```json
{
  "mcpServers": {
    "modus-context": {
      "type": "http",
      "url": "https://mcp.getmodus.com/modus?toolsets=context"
    },
    "modus-manage": {
      "type": "http",
      "url": "https://mcp.getmodus.com/modus?toolsets=manage"
    }
  }
}
```

### `mcp-remote` bridge (older Desktop)

Requires **Node.js**. The manage entry pins the full management scopes for consent; the context entry pins read/invoke scopes. Workflows are governed by the `agents:*` scopes (legacy naming — the OAuth vocabulary has no separate `workflows:*` scope).

```json
{
  "mcpServers": {
    "modus-context": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.getmodus.com/modus?toolsets=context",
        "--static-oauth-client-metadata",
        "{\"scope\":\"skills:read skills:invoke agents:read agents:invoke context:read\"}"
      ]
    },
    "modus-manage": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.getmodus.com/modus?toolsets=manage",
        "--static-oauth-client-metadata",
        "{\"scope\":\"skills:read skills:invoke skills:write agents:read agents:invoke agents:write context:read context:write users:read users:write usage:read\"}"
      ]
    }
  }
}
```

---

## Other environments

| Environment | Context | Manage |
| --- | --- | --- |
| Production | `https://mcp.getmodus.com/modus?toolsets=context` | `https://mcp.getmodus.com/modus?toolsets=manage` |
| Staging | `https://mcp.staging.getmodus.com/modus?toolsets=context` | `https://mcp.staging.getmodus.com/modus?toolsets=manage` |
| Local dev | `http://localhost:3041/modus?toolsets=context` | `http://localhost:3041/modus?toolsets=manage` |

---

## PAT (no OAuth)

A Modus **Personal Access Token** works without a browser — already scoped to one org by its prefix. Set `Authorization: Bearer modus_<orgUuid>_<prefix>_<secret>`.

---

## How it's secured

- **On behalf of you** — your user token only; no service account.
- **Scope + per-call ACL** on every tool call.
- **One org per connection** — switch org by reconnecting.
- **Capability split by toolset** — the `context` and `manage` connections expose different tools from the same endpoint; each tool is gated by its required scope.

---

## Links

- Modus: <https://getmodus.com>
- Plugin source: <https://github.com/modus-data/modus-claude-plugin>
