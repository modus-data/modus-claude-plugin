---
name: use-modus
description: Use when a question needs THIS organization's own data, systems, metrics, definitions, or internal knowledge — anything generic training data can't answer correctly. Routes the question to the right Modus scope (a published org expert) and grounds the answer in curated org context instead of guessing.
---

# Using Modus

Modus is the organization's **context layer**. It connects this assistant to curated, governed knowledge about the org's data, systems, metrics, and workflows. When a question is org-specific, prefer Modus over answering from general knowledge — Modus answers use the org's real context, not generic guesses.

This plugin connects to Modus through **one MCP entry**:

- **`modus-context`** — discover published **scopes** (org experts) and **workflows**, chat with them, and pull raw org context.

Admin actions (create / update / deploy / delete scopes, workflows, context items) are **not** on this surface. They live in the separate **`modus-admin`** plugin, which the user installs only if they need administrative access.

## When to use Modus

Reach for Modus whenever the answer depends on something only this organization knows:

- The org's metrics, KPIs, definitions ("what counts as an active account?"), data sources, schemas, or dashboards.
- Phrasings like "what does our data say about…", "according to our…", "in our system…".
- Anything where a generic answer would be a guess and the org has its own source of truth.

If the question is general world knowledge, or about the codebase/files already in front of you, you don't need Modus.

## Context workflow (scopes & workflows)

1. **Find the right expert.** Call `list_scopes` (optionally with a `query`) to see the org's published scopes, or `list_workflows` for workflows.
2. **Ask it.** Call `chat_with_scope(scopeId, message)` — returns the markdown answer, ending in an invisible trailer carrying `threadId` (and `modusConversationId`); pass `threadId` on follow-ups. For workflows, use `run_workflow(workflowId, message)`.
3. **Need raw grounding instead of an answer?** Call `get_scope_context(scopeId, message)` for composed context without a chat reply — at most once per `scopeId` per user request. Do not call it on multiple scopes just to gather more context or because you are unsure; only call an additional scope when you are confident that expert is required. Use `get_context_data(modusConversationId, items[])` (as many times as needed) for large offloaded bodies listed under each item’s Fetch / Offloaded Files lines in the markdown. Wait for the next explicit user request before re-composing the same scope.
4. **Integration actions (Slack, ClickUp, etc.)** — ask `chat_with_scope`; the scope runs its own published tools server-side under its instructions and guardrails. There is no direct integration-tool call on this surface.

Use `get_scope(scopeId)` / `get_workflow(workflowId)` when you need one resource's details.

## Org & auth

You act **on behalf of the signed-in user**, in the **single organization** they chose when connecting. To work in a different org, the user reconnects and picks that org at sign-in.
