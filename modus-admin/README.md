# Modus Admin plugin for Claude

The administrative surface for Modus — create, update, deploy, and delete scopes, workflows, and context items from Claude.

This plugin is **separate** from the main `modus` plugin so that most users never see an admin connection grey out on their connectors screen. Install it only if you need to manage Modus configuration from Claude.

> **Most users do not need this plugin.** The `modus` plugin already covers chat, scopes, workflows, and reading org context. Install this one only if you'll be authoring or deploying Modus scopes, workflows, or context items.

---

## Requirements

- A **Modus account** with **admin permissions** in at least one organization.
- The **`modus`** plugin from the same marketplace (recommended) — admin tasks are usually followed by a quick test chat against the changed scope.
- **Claude Code** (CLI) or **Claude Desktop** with Personal plugins support.
- An internet connection.

---

## Install

### Claude Code

```bash
# If you have not added the marketplace yet:
/plugin marketplace add modus-data/modus-claude-plugin

# Install the admin plugin
/plugin install modus-admin@modus
```

### Claude Desktop

1. Open **Customize** in the left sidebar.
2. Go to **Personal plugins** → **+** → **Add marketplace**.
3. Enter `modus-data/modus-claude-plugin` → **Sync**.
4. Install **Modus Admin** from the list.

---

## Sign in

After install, authenticate the **`modus-admin`** connection in MCP settings. Choose the **same organization** as your `modus` plugin sign-in so chat and admin actions stay aligned.

Permission checks happen server-side on every call. A request that exceeds your role will fail with a clear error — admin tools require the corresponding Modus permission (`SkillsWrite`, `AgentsWrite`, `ContextWrite`, etc.).

---

## Using Modus Admin

Ask Claude in plain language. Use explicit verbs — *create*, *update*, *deploy*, *delete*. The plugin's built-in `manage-modus` skill will route to the right tool.

Examples:

- "Create a scope called Revenue Q4 with these instructions…"
- "Deploy the weekly-product-updater workflow."
- "Delete the old Slack-notes context item."
- "Add a note to context with the following text…"

Claude will restate the change in plain English and ask you to confirm before running destructive operations (delete, deploy) — that's by design in the skill that ships with this plugin.

---

## Advanced: manual MCP setup

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

Authenticate when prompted.

---

## Security

- **Your identity only** — Claude acts on your behalf with your user credentials. There is no shared service account.
- **Permission checks on every action** — each request is authorized against your role and the resources you can access.
- **Destructive operations are gated** — admin tools that delete or deploy require explicit confirmation.
- **Separate from the read surface** — installing this plugin does not change what `modus` (the read/chat plugin) can do.

---

## Help & links

- **Modus product:** <https://getmodus.com>
- **Plugin repository:** <https://github.com/modus-data/modus-claude-plugin>

Questions about your Modus account or admin role? Contact your Modus administrator.
