# Modus Admin plugin for Claude

Administrative surface for Modus — create, update, deploy, and delete scopes, workflows, and context items.

**Full install guide:** see the [marketplace README](../README.md) for marketplace setup, both MCP connections, sign-in, and manual configuration.

> **Most users do not need this plugin.** The `modus` plugin already covers chat, scopes, workflows, and reading org context.

---

## Requirements

- **Admin permissions** in at least one Modus organization.
- The **`modus`** plugin from the same marketplace (recommended) — admin tasks are usually followed by a quick test chat against the changed scope.

---

## Sign in

Authenticate the **`modus-admin`** connection in MCP settings. Choose the **same organization** as your `modus` plugin sign-in.

Permission checks happen server-side on every call. A request that exceeds your role fails with a clear error.

---

## Usage

Ask Claude in plain language with explicit verbs — *create*, *update*, *deploy*, *delete*. The built-in `manage-modus` skill routes to the right tool.

Examples:

- "Create a scope called Revenue Q4 with these instructions…"
- "Deploy the weekly-product-updater workflow."
- "Delete the old Slack-notes context item."

Claude restates destructive changes in plain English and asks you to confirm before running delete or deploy operations.

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

For both connections in one config, see the [marketplace README](../README.md#advanced-manual-mcp-setup).

---

## Security

- **Permission checks on every action** — admin tools require the corresponding Modus permission.
- **Destructive operations are gated** — delete and deploy require explicit confirmation.
- **Separate from the read surface** — installing this plugin does not change what the `modus` plugin can do.
