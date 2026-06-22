# Modus plugin for Claude

Connect Claude to your organization's **Modus context layer**. Once installed, Claude can:

- **Chat with Modus** across your whole org environment (`modus-assistant`).
- **Discover and chat with published skills** and **agents**, and pull org context (`modus-context`).
- **Manage** skills, agents, and context items for admins (`modus-organization`).

The plugin bundles **three OAuth-secured remote MCP servers** plus a guidance skill (`use-modus`) that teaches Claude when and how to use Modus.

| Server | What it does | URL (production) |
| --- | --- | --- |
| `modus-assistant` | Full-environment Modus (homepage agent) | `https://mcp.getmodus.com/assistant` |
| `modus-context` | Skills, agents, context compose, skill integration tools | `https://mcp.getmodus.com/context` |
| `modus-organization` | Create / update / deploy / delete skills, agents, context | `https://api.getmodus.com/api/v1/mcp` |

You **pick your organization when you sign in** — one install works for every org you belong to. To switch org, reconnect and pick a different one (see [Switching org](#switching-org)).

---

## Requirements

- A **Modus account** with access to at least one organization.
- **Claude Code** (CLI) or the **Claude Desktop** app (Personal plugins marketplace).
- Nothing to host — Modus runs the servers; the plugin only points at them.

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

> Authenticate **all three** servers. **Pick the same org for each** so assistant, context, and management operate on the same data.

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
    "modus-assistant": {
      "type": "http",
      "url": "https://mcp.getmodus.com/assistant"
    },
    "modus-context": {
      "type": "http",
      "url": "https://mcp.getmodus.com/context"
    },
    "modus-organization": {
      "type": "http",
      "url": "https://api.getmodus.com/api/v1/mcp"
    }
  }
}
```

### `mcp-remote` bridge (older Desktop)

Requires **Node.js**. The organization entry pins write scopes for consent.

```json
{
  "mcpServers": {
    "modus-assistant": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.getmodus.com/assistant"]
    },
    "modus-context": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.getmodus.com/context"]
    },
    "modus-organization": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://api.getmodus.com/api/v1/mcp",
        "--static-oauth-client-metadata",
        "{\"scope\":\"skills:read skills:write agents:read agents:write context:read context:write usage:read\"}"
      ]
    }
  }
}
```

---

## Other environments

| Environment | Assistant | Context | Organization |
| --- | --- | --- | --- |
| Production | `https://mcp.getmodus.com/assistant` | `https://mcp.getmodus.com/context` | `https://api.getmodus.com/api/v1/mcp` |
| Staging | `https://mcp.staging.getmodus.com/assistant` | `https://mcp.staging.getmodus.com/context` | `https://api.staging.getmodus.com/api/v1/mcp` |
| Local dev | `http://localhost:3041/assistant` | `http://localhost:3041/context` | `http://localhost:3040/api/v1/mcp` |

---

## PAT (no OAuth)

A Modus **Personal Access Token** works without a browser — already scoped to one org by its prefix. Set `Authorization: Bearer modus_<orgUuid>_<prefix>_<secret>`.

---

## How it's secured

- **On behalf of you** — your user token only; no service account.
- **Scope + per-call ACL** on every tool call.
- **One org per connection** — switch org by reconnecting.
- **Audience isolation** — MCP hosts and the API host use distinct OAuth audiences.

---

## Links

- Modus: <https://getmodus.com>
- Plugin source: <https://github.com/modus-data/modus-claude-plugin>
