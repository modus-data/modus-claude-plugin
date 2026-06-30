# Modus plugin for Claude

Bring your organization's knowledge into Claude. Modus is a **context layer** that connects AI assistants to curated, governed information about your data, systems, metrics, and workflows — so answers reflect how your organization actually works, not generic guesses.

This plugin connects Claude to Modus in minutes. No servers to run; Modus hosts everything.

---

## What you can do

Once installed and signed in, Claude can:

- **Answer org-specific questions** — metrics, definitions, data sources, dashboards, and internal processes grounded in your Modus context.
- **Chat with published scopes** — each scope is a configured expert on a topic (for example, revenue analytics or customer support).
- **Run workflows** — scheduled or triggered automations built on your published scopes.
- **Manage Modus (admins only)** — create, update, deploy, or retire scopes, workflows, and context items.

The plugin includes a built-in guide that teaches Claude when to reach for Modus versus general knowledge.

---

## Requirements

- A **Modus account** with access to at least one organization.
- **Claude Code** (CLI) or **Claude Desktop** with Personal plugins support.
- An internet connection — Modus runs the service; you only install the plugin.

---

## Install

### Claude Code

```bash
# Add the Modus marketplace (once)
/plugin marketplace add modus-data/modus-claude-plugin

# Install the plugin
/plugin install modus@modus
```

### Claude Desktop

1. Open **Customize** in the left sidebar.
2. Go to **Personal plugins** → **+** → **Add marketplace**.
3. Enter `modus-data/modus-claude-plugin` → **Sync**.
4. Install **Modus** from the list.

If you use both **Chat** and **Code** in Desktop, install the plugin in each — they are separate environments.

---

## Sign in

After install, connect Modus to Claude:

1. Open MCP settings — run `/mcp` in Claude Code, or follow the connect flow in Desktop.
2. You will see two Modus connections. **Authenticate both:**
   - **Modus (context)** — chat, scopes, workflows, and org context.
   - **Modus (manage)** — administrative actions (scopes, workflows, context items).
3. A browser window opens for Modus sign-in. Complete OAuth and **choose your organization** during consent.

Authenticate both connections and **select the same organization for each** so chat and admin actions stay in sync.

You only need to sign in once per connection. One plugin install works for every organization you belong to — pick the org at sign-in time.

### Switch organizations

To work in a different org, reconnect and choose it at sign-in:

- **Claude Code:** `/mcp` → select the Modus server → re-authenticate.
- **Claude Desktop:** disconnect each Modus server, then reconnect and sign in again.

If sign-in gets stuck, disconnect, clear any cached MCP credentials your client may store, and try again.

---

## Using Modus

You do not need to learn Modus tool names. Ask Claude normally — for example:

- "What does our team define as an active customer?"
- "Summarize how we calculate monthly recurring revenue."
- "Which scope should I use for product analytics questions?"

When a question depends on your organization's knowledge, Claude uses Modus automatically. For admin tasks ("create a new scope", "deploy this workflow"), ask explicitly — those actions use the manage connection and require appropriate permissions in Modus.

---

## Advanced: manual MCP setup

Prefer configuring MCP servers directly? Add these entries in **Settings → Developer → Edit Config** (`claude_desktop_config.json`):

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

Authenticate each server when prompted.

### Personal Access Token

For environments where browser OAuth is not available, use a Modus **Personal Access Token** (scoped to one organization). Send it as:

```
Authorization: Bearer modus_<orgUuid>_<prefix>_<secret>
```

Create a token in Modus under your account settings.

---

## Security

- **Your identity only** — Claude acts on your behalf with your user credentials. There is no shared service account.
- **Permission checks on every action** — each request is authorized against your role and the resources you can access.
- **One organization per connection** — the org you pick at sign-in is the org Claude works in. Switch orgs by reconnecting.
- **Separate chat and admin access** — the context and manage connections expose different capabilities; admin tools require explicit permissions.

---

## Help & links

- **Modus product:** <https://getmodus.com>
- **Plugin repository:** <https://github.com/modus-data/modus-claude-plugin>

Questions about your Modus account or organization setup? Contact your Modus administrator or Modus support through your organization's usual channel.
