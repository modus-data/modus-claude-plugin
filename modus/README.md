# Modus plugin for Claude

Ask Claude questions about your organization — definitions, metrics, scopes, and workflows.

**Full install guide:** [marketplace README](../README.md)

---

## What this plugin does

Once installed and signed in, Claude can:

- Answer org-specific questions using your team's curated knowledge.
- Chat with **scopes** — topic experts your org configured.
- Run **workflows** — automations built on those scopes.

The built-in guide teaches Claude when to use Modus versus general knowledge.

> **Need to change Modus configuration?** Install **Modus Admin** from the same marketplace. Most users do not need it. See [Modus Admin](../modus-admin/README.md).

---

## Sign in

When Claude prompts you to connect Modus, sign in and choose your organization. In MCP settings this connection appears as **Modus (Chat)** (`modus-context`).

---

## Tips

Ask naturally — you don't need Modus tool names. Complex questions may take up to a minute; that's normal.

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

For both chat and admin servers, see the [marketplace README](../README.md#advanced-manual-mcp-setup).

---

## Security

This plugin is **read/chat only** — it cannot create, modify, or delete Modus configuration.
