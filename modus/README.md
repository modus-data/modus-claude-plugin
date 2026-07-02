# Modus plugin for Claude

Read/chat surface for Modus — scopes, workflows, and org context.

**Full install guide:** see the [marketplace README](../README.md) for marketplace setup, both MCP connections, sign-in, and manual configuration.

---

## What this plugin does

Once installed and signed in (`modus-context` connection), Claude can:

- **Answer org-specific questions** grounded in your Modus context.
- **Chat with published scopes** — configured experts on topics in your org.
- **Run workflows** — scheduled or triggered automations on your scopes.

The built-in `use-modus` skill teaches Claude when to reach for Modus versus general knowledge.

> **Need admin actions?** Install the separate **`modus-admin`** plugin from the same marketplace. Most users do not need it. See [`modus-admin/README.md`](../modus-admin/README.md).

---

## Sign in

Authenticate the **`modus-context`** connection in MCP settings (`/mcp` in Claude Code). Complete OAuth and choose your organization.

---

## Usage tips

Ask Claude normally — you do not need to learn Modus tool names. Scope chats may take **10–60 seconds**; the plugin uses a 3-minute timeout and the server streams progress during long answers.

---

## Advanced: manual MCP (this plugin only)

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

For both connections in one config, see the [marketplace README](../README.md#advanced-manual-mcp-setup).

---

## Security

This plugin is **read/chat only** — it cannot create, modify, or delete Modus configuration. Admin actions require the separate `modus-admin` plugin.
