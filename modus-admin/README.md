# Modus Admin plugin for Claude

Create, update, deploy, and delete scopes, workflows, and context items from Claude.

**Full install guide:** [marketplace README](../README.md)

> **Most users do not need this plugin.** The **Modus** plugin already covers chat, scopes, workflows, and reading org context.

---

## Requirements

- **Admin permissions** in at least one Modus organization.
- The **Modus** plugin from the same marketplace (recommended) — so you can test changes in chat right after.

---

## Sign in

Connect **Modus (Manage)** when Claude prompts you (`modus-admin` in MCP settings). Choose the **same organization** as your Modus chat sign-in.

If you lack permission for an action, Modus returns a clear error.

---

## Usage

Ask with explicit verbs — *create*, *update*, *deploy*, *delete*. Examples:

- "Create a scope called Revenue Q4 with these instructions…"
- "Deploy the weekly-product-updater workflow."
- "Delete the old Slack-notes context item."

Claude asks you to confirm before destructive operations like delete or deploy.

---

## Advanced: manual MCP (this plugin only)

```json
{
  "mcpServers": {
    "modus-admin": {
      "type": "http",
      "url": "https://mcp.getmodus.com/modus?toolsets=manage"
    }
  }
}
```

For both chat and admin servers, see the [marketplace README](../README.md#advanced-manual-mcp-setup).

---

## Security

- Every action is checked against your Modus permissions.
- Delete and deploy require explicit confirmation.
- Installing this plugin does not change what the Modus chat plugin can do.
