# Modus plugin for Claude

Bring your organization's knowledge into Claude. Modus is a **context layer** that connects AI assistants to curated, governed information about your data, systems, metrics, and workflows — so answers reflect how your organization actually works, not generic guesses.

This plugin connects Claude to Modus in minutes. No servers to run; Modus hosts everything.

> **Need admin actions?** This plugin is the read/chat surface. To create, update, deploy, or delete scopes, workflows, and context items, install the separate **`modus-admin`** plugin from the same marketplace. Most users do not need it.

---

## What you can do

Once installed and signed in, Claude can:

- **Answer org-specific questions** — metrics, definitions, data sources, dashboards, and internal processes grounded in your Modus context.
- **Chat with published scopes** — each scope is a configured expert on a topic (for example, revenue analytics or customer support).
- **Run workflows** — scheduled or triggered automations built on your published scopes.

The plugin includes a built-in guide (the `use-modus` skill) that teaches Claude when to reach for Modus versus general knowledge.

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
2. You will see one Modus connection: **`modus-context`**. Authenticate it.
3. A browser window opens for Modus sign-in. Complete OAuth and **choose your organization** during consent.

You only need to sign in once per machine. One plugin install works for every organization you belong to — pick the org at sign-in time.

### Switch organizations

To work in a different org, reconnect and choose it at sign-in:

- **Claude Code:** `/mcp` → select the Modus server → re-authenticate.
- **Claude Desktop:** disconnect the Modus server, then reconnect and sign in again.

If sign-in gets stuck, disconnect, clear any cached MCP credentials your client may store, and try again.

---

## Using Modus

You do not need to learn Modus tool names. Ask Claude normally — for example:

- "What does our team define as an active customer?"
- "Summarize how we calculate monthly recurring revenue."
- "Which scope should I use for product analytics questions?"

When a question depends on your organization's knowledge, Claude uses Modus automatically.

Scope chats run a real LLM on the server, so a single answer may take **10–60 seconds** (longer for complex scopes). The plugin uses a 3-minute connection timeout and the server streams progress so the call stays alive — but if you ask broad onboarding-style questions, expect them to take a while. Focused questions are faster.

For admin tasks ("create a new scope", "deploy this workflow"), install the separate **`modus-admin`** plugin.

---

## Advanced: manual MCP setup

Prefer configuring MCP servers directly? Add this entry in **Settings → Developer → Edit Config** (`claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "modus-context": {
      "type": "http",
      "url": "https://mcp.getmodus.com/modus?toolsets=context",
      "timeout": 180000
    }
  }
}
```

Authenticate the server when prompted.

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
- **Read/chat only** — this plugin cannot create, modify, or delete Modus configuration. Those actions live in the separate `modus-admin` plugin.

---

## Help & links

- **Modus product:** <https://getmodus.com>
- **Plugin repository:** <https://github.com/modus-data/modus-claude-plugin>

Questions about your Modus account or organization setup? Contact your Modus administrator or Modus support through your organization's usual channel.
