# Modus plugins for Claude

Connect Claude to your organization's knowledge — metrics, definitions, dashboards, and how your team actually works. Install in minutes. Modus hosts everything; you don't run any servers.

---

## Quick start

1. **Add the marketplace** (once) — see [Install](#install) below.
2. **Install Modus** — the chat plugin almost everyone needs.
3. **Sign in** when Claude asks — pick your organization.

Then try asking Claude:

- "What does our team define as an active customer?"
- "Summarize how we calculate monthly recurring revenue."

When a question depends on your organization's knowledge, Claude uses Modus automatically. You don't need to learn Modus tool names.

---

## What you can do

- **Ask org-specific questions** — grounded in your team's curated definitions, data sources, and processes.
- **Chat with scopes** — topic experts your organization configured (for example, revenue analytics or customer support).
- **Run workflows** — automations your team set up on those scopes.

**Terms in plain language:**

| Term | Meaning |
| --- | --- |
| **Scope** | A topic expert your org configured in Modus |
| **Workflow** | A scheduled or triggered automation built on a scope |
| **Context** | Your org's curated definitions, metrics, and documentation |

---

## Chat vs. Manage (two plugins)

Most people only need **Modus** (chat). Admins who want to create or change scopes and workflows from Claude also install **Modus Admin**.

| What you need | Plugin to install | What you'll connect in Claude |
| --- | --- | --- |
| Ask questions, chat with scopes, run workflows | **Modus** | **Modus (Chat)** |
| Create, update, deploy, or delete scopes and workflows | **Modus Admin** | **Modus (Manage)** |

If you install both, sign in to each and choose the **same organization** so chat and admin actions stay aligned.

More detail: [Modus plugin](modus/README.md) · [Modus Admin plugin](modus-admin/README.md)

---

## Requirements

- A **Modus account** with access to at least one organization.
- **Claude Code** (CLI) or **Claude Desktop** with Personal plugins support.
- An internet connection.

---

## Install

### Claude Code

```bash
# Add the Modus marketplace (once)
/plugin marketplace add modus-data/modus-claude-plugin

# Install Modus — chat and org questions (everyone)
/plugin install modus@modus
```

### Claude Desktop

1. Open **Customize** in the left sidebar.
2. Go to **Personal plugins** → **+** → **Add marketplace**.
3. Enter `modus-data/modus-claude-plugin` → **Sync**.
4. Install **Modus** from the list.

If you use both **Chat** and **Code** in Desktop, install the plugin in each — they are separate environments.

### Optional: Modus Admin (admins only)

Only if you need to create, update, deploy, or delete scopes, workflows, or context items from Claude:

```bash
/plugin install modus-admin@modus
```

In Claude Desktop, install **Modus Admin** from the same marketplace after syncing.

---

## Sign in

After install, Claude will prompt you to connect Modus:

1. Click **Connect** (or run `/mcp` in Claude Code and select Modus).
2. Sign in with your Modus account in the browser window that opens.
3. **Choose your organization** — Claude will use that org's knowledge going forward.

You only need to sign in once per connection. If you belong to multiple organizations, pick the one you want to work in now; you can switch later by reconnecting.

**If you installed Modus Admin:** repeat the connect flow for **Modus (Manage)** and pick the same organization.

### Switch organizations

Reconnect Modus and choose a different org at sign-in:

- **Claude Code:** `/mcp` → select Modus → connect again.
- **Claude Desktop:** disconnect Modus, then reconnect and sign in again.

If sign-in gets stuck, disconnect Modus, clear any saved credentials your client may have stored, and try again.

---

## Tips for good results

- Ask naturally — Claude routes org questions to Modus for you.
- Complex scope questions may take **up to a minute**; that's normal.
- For admin tasks ("create a scope", "deploy this workflow"), install **Modus Admin** and ask with clear verbs: *create*, *update*, *deploy*, *delete*.

---

## Advanced: manual MCP setup

Prefer configuring MCP servers directly? Add these entries in **Settings → Developer → Edit Config** (`claude_desktop_config.json`). Only include the servers you use.

```json
{
  "mcpServers": {
    "modus-context": {
      "type": "http",
      "url": "https://mcp.getmodus.com/modus?toolsets=context",
      "timeout": 180000
    },
    "modus-admin": {
      "type": "http",
      "url": "https://mcp.getmodus.com/modus?toolsets=manage"
    }
  }
}
```

Authenticate each server when prompted.

### Personal Access Token

For environments where browser sign-in is not available, use a Modus **Personal Access Token** (scoped to one organization). Send it as:

```
Authorization: Bearer modus_<orgUuid>_<prefix>_<secret>
```

Create a token in Modus under your account settings.

---

## Security

- **Your identity only** — Claude acts on your behalf with your user credentials. There is no shared service account.
- **Permission checks on every action** — each request is authorized against your role and the resources you can access.
- **One organization per connection** — the org you pick at sign-in is the org Claude works in. Switch orgs by reconnecting.
- **Chat and admin are separate** — the Modus plugin is read/chat only; changing configuration requires Modus Admin and the right permissions.

---

## Help & links

- **Modus product:** <https://getmodus.com>
- **Plugin repository:** <https://github.com/modus-data/modus-claude-plugin>
- **Modus plugin (chat):** [modus/README.md](modus/README.md)
- **Modus Admin plugin:** [modus-admin/README.md](modus-admin/README.md)

Questions about your Modus account or organization setup? Contact your Modus administrator or Modus support through your organization's usual channel.
