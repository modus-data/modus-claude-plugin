---
name: use-modus
description: Use when a question needs THIS organization's own data, systems, metrics, definitions, or internal knowledge — anything generic training data can't answer correctly. Routes the question to the right Modus skill (a published org expert) and grounds the answer in curated org context instead of guessing.
---

# Using Modus

Modus is the organization's **context layer**. It connects this assistant to curated, governed knowledge about the org's data, systems, metrics, and workflows. When a question is org-specific, prefer Modus over answering from general knowledge — Modus answers use the org's real context, not generic guesses.

This plugin exposes two Modus MCP servers:

- **`modus-context`** — discover published **skills** and **agents**, chat with them, and pull raw org context. This is what you use for most org questions.
- **`modus-organization`** — create / update / deploy / delete skills, agents, and context items. Administrative; use only on explicit admin requests.

## When to use Modus

Reach for Modus whenever the answer depends on something only this organization knows:

- The org's metrics, KPIs, definitions ("what counts as an active account?"), data sources, schemas, or dashboards.
- Phrasings like "what does our data say about…", "according to our…", "in our system…".
- Anything where a generic answer would be a guess and the org has its own source of truth.

If the question is general world knowledge, or about the codebase/files already in front of you, you don't need Modus.

## Context workflow (skills & agents)

1. **Find the right expert.** Call `list_skills` (optionally with a `query`) to see the org's published skills, or `list_agents` for automations.
2. **Ask it.** Call `chat_with_skill(skillId, message)` — returns JSON with `content` and `threadId`; pass `threadId` on follow-ups. For automations, use `chat_with_agent(agentId, message)`.
3. **Need raw grounding instead of an answer?** Call `get_skill_context(skillId, message)` for composed context without a chat reply. Use `get_context_item(sessionId, path, …)` for large `contextFiles` bodies.
4. **Integration tools** — call `list_skill_tools(skillId)` then `invoke_skill_tool(skillId, toolName, arguments)`.

Use `get_skill(skillId)` / `get_agent(agentId)` when you need one resource's details.

## Organization (admin only)

The **`modus-organization`** tools (`skills_*`, `agents_*`, `context_*`) **change** the org's Modus configuration. Use them only when the user explicitly asks to create, edit, deploy, or delete something. Destructive operations require an explicit confirmation flag.

## Org & auth

You act **on behalf of the signed-in user**, in the **single organization** they chose when connecting. To work in a different org, the user reconnects the Modus server and picks that org at sign-in.
