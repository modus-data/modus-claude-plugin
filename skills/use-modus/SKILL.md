---
name: use-modus
description: Use when a question needs THIS organization's own data, systems, metrics, definitions, or internal knowledge — anything generic training data can't answer correctly. Routes the question to the right Modus scope (a published org expert) and grounds the answer in curated org context instead of guessing.
---

# Using Modus

Modus is the organization's **context layer**. It connects this assistant to curated, governed knowledge about the org's data, systems, metrics, and workflows. When a question is org-specific, prefer Modus over answering from general knowledge — Modus answers use the org's real context, not generic guesses.

This plugin connects to **one unified Modus MCP**, exposed as two capability-scoped entries:

- **`modus-context`** — discover published **scopes** (org experts) and **workflows**, chat with them, and pull raw org context. This is what you use for most org questions.
- **`modus-manage`** — create / update / deploy / delete scopes, workflows, and context items. Administrative; use only on explicit admin requests.

## When to use Modus

Reach for Modus whenever the answer depends on something only this organization knows:

- The org's metrics, KPIs, definitions ("what counts as an active account?"), data sources, schemas, or dashboards.
- Phrasings like "what does our data say about…", "according to our…", "in our system…".
- Anything where a generic answer would be a guess and the org has its own source of truth.

If the question is general world knowledge, or about the codebase/files already in front of you, you don't need Modus.

## Context workflow (scopes & workflows)

1. **Find the right expert.** Call `list_scopes` (optionally with a `query`) to see the org's published scopes, or `list_workflows` for workflows.
2. **Ask it.** Call `chat_with_scope(scopeId, message)` — returns JSON with `content` and `threadId`; pass `threadId` on follow-ups. For workflows, use `chat_with_workflow(workflowId, message)`.
3. **Need raw grounding instead of an answer?** Call `get_scope_context(scopeId, message)` for composed context without a chat reply. Use `get_context_item(sessionId, path, …)` for large `contextFiles` bodies.
4. **Integration tools** — call `list_scope_tools(scopeId)` then `invoke_scope_tool(scopeId, toolName, arguments)`.

Use `get_scope(scopeId)` / `get_workflow(workflowId)` when you need one resource's details.

## Organization (admin only)

The **`modus-manage`** tools (`scopes_*`, `workflows_*`, `context_*`) **change** the org's Modus configuration. Use them only when the user explicitly asks to create, edit, deploy, or delete something. Destructive operations require an explicit confirmation flag.

## Org & auth

You act **on behalf of the signed-in user**, in the **single organization** they chose when connecting. Both entries connect to the same Modus MCP, so one sign-in covers them; to work in a different org, the user reconnects and picks that org at sign-in.
