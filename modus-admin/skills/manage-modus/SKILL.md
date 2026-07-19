---
name: manage-modus
description: Use when the user explicitly asks to create, update, deploy, or delete Modus configuration (scopes, workflows, context items), or for explicit org-admin reads on this surface (org roster, usage/cost). Day-to-day chat/read of scopes and context is the separate `use-modus` skill in the `modus` plugin.
---

# Managing Modus

This skill governs the **administrative** surface of Modus — the `modus-admin` connection. Use it when the user explicitly asks to **change** the org's Modus configuration, or for **explicit admin reads** that only exist on this manage surface:

- **Scopes** — create, update, deploy, undelete, or delete published org experts; also evaluations / supervision / MCP config when asked.
- **Workflows** — create, update, deploy, undelete, delete, toggle, or manage interfaces for scheduled / triggered automations.
- **Context items** — create notes, links, saved queries, and custom items; update or delete existing items.
- **Org roster / usage** — read-only `users_*` and `usage_*` when the user explicitly asks who is in the org, which groups a member belongs to, or about org LLM spend / run counts.

For chatting with scopes, listing them, or pulling org context to answer a question, use the **`use-modus`** skill in the `modus` plugin instead. This skill should never be the first reach for scope chat or context composition.

## Hard rule: explicit user request

Do not use any tool in this skill unless the user has explicitly asked for an administrative change **or** an explicit admin read that lives on this surface (`users_*`, `usage_*`). Phrases that authorize this surface:

- "Create a new scope for X."
- "Deploy the X workflow to staging."
- "Delete this context item."
- "Update the X scope's instructions."
- "Who is in this org?" / "Which groups is X in?"
- "How much did we spend on LLM usage this month?"

Phrases that do **NOT** authorize this surface (use `use-modus` instead):

- "Ask the X scope about Y."
- "What does our data say about Z?"
- "List the scopes we have."

If you are unsure whether a request belongs here, ask the user before calling any tool here.

## Tools

The catalog is grouped by resource. The server's `tools/list` is the source of truth — read each tool's description before calling it. Major groups:

- **Scopes (`scopes_*`)** — CRUD on published org experts: `scopes_list`, `scopes_get`, `scopes_create`, `scopes_update`, `scopes_deploy`, `scopes_delete`, `scopes_undelete`. Variation inspection: `scopes_get_variation`. MCP exposure config: `scopes_patch_mcp_config`. Conversations: `scopes_conversations_list`, `scopes_conversations_get`. Memories: `scopes_memories_list`, `scopes_memories_search`, `scopes_memories_update`, `scopes_memories_delete`. Evaluations: `scopes_evaluations_get_config`, `scopes_evaluations_update_config`, `scopes_evaluations_list_runs`, `scopes_evaluations_get_run`, `scopes_evaluations_trigger_run`. Supervision: `scopes_supervision_get`, `scopes_supervision_set`, `scopes_supervision_set_active`. Ownership transfer: `scopes_request_ownership_transfer`, `scopes_accept_ownership_transfer`, `scopes_cancel_ownership_transfer`.
- **Workflows (`workflows_*`)** — CRUD on scheduled / triggered automations: `workflows_list`, `workflows_get`, `workflows_create`, `workflows_update`, `workflows_deploy`, `workflows_delete`, `workflows_undelete`. Enable/disable: `workflows_toggle`. Interfaces (triggers / entry points): `workflows_interfaces_list`, `workflows_interfaces_create`, `workflows_interfaces_update`, `workflows_interfaces_delete`, `workflows_interfaces_delete_all`. Ownership transfer: `workflows_request_ownership_transfer`, `workflows_accept_ownership_transfer`, `workflows_cancel_ownership_transfer`.
- **Context items (`context_*`)** — Reads/edits on existing items: `context_items_list`, `context_items_get`, `context_items_update`, `context_items_delete`. New items: `context_create_note`, `context_create_link`, `context_create_saved_query`. Custom items: `context_custom_items_list`, `context_custom_items_get`, `context_custom_items_create`, `context_custom_items_batch_create`, `context_custom_items_update`, `context_custom_items_delete`.
- **Users (`users_*`)** — Read-only org roster: `users_org_members_list`, `users_member_groups_list`. Use when the user asks who is in the org or which groups a member belongs to.
- **Usage (`usage_*`)** — Read-only org usage / cost queries: `usage_list`. Use this when the user asks about LLM spend, run counts, or other consumption metrics scoped to the org.

Note: org-wide actions such as deleting an entire organization are intentionally **not** exposed on this MCP surface. They live only in the Modus dashboard, where the user sees the full consequence screen. If a user asks to delete the entire org from chat, explain that this action is dashboard-only and offer to help with scoped deletes (single scope, workflow, or context item) instead.

## Destructive operations

Any tool that creates, updates, deploys, or deletes resources is **destructive** in the sense that it changes the org's published Modus configuration. Before calling one:

1. Restate to the user what will change, in plain English.
2. Confirm the target resource (id + name).
3. For deletes and deploys to production, get explicit user confirmation.

Some tools require a `confirm: true` argument; honor it. Never bypass confirmation by setting it on a user's behalf without their say-so. Note that confirmation is the agent's job — most tools do not prompt at the server level — so the responsibility for restating the change and getting a clear yes rests here.

## Org & auth

You act **on behalf of the signed-in user**, with the **admin permissions** they hold in the **single organization** they chose when connecting. Permission checks happen server-side on every call; a request that exceeds the user's role will fail. To work in a different org, the user reconnects this plugin and picks that org at sign-in.

## What this skill does NOT do

- Chat with a scope — that's `chat_with_scope` in `use-modus`.
- Pull org context to answer a question — that's `get_scope_context` in `use-modus`.
- Invoke a scope's published integration tools — that's handled inside `chat_with_scope`.

If a request needs both administration and chat (e.g., "create a scope and then test it"), do the admin step here, then switch to `use-modus` for the chat step.
