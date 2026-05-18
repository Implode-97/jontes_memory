---
name: recall-memory
description: Search the Obsidian Codex memory folders before acting when prior user preferences, project decisions, architecture context, implementation patterns, Terraform/Terragrunt/Databricks conventions, or past Codex workflow learnings may be relevant. Use at the start of tasks that could benefit from remembered context across sessions.
---

# Recall Memory

Look for relevant prior learnings before making recommendations or edits when the task touches recurring user work, preferences, or project/domain conventions.

## Sources

Resolve the memory root before searching:

- Prefer the current working directory when it contains `Short-Term`, `Long-Term`, `Dreams`, and `Archive`.
- Otherwise use the default memory root:

`/Users/jnyjc2/Documents/Obsidian Vault/CodexMemory/CodexMemory/Memory`

Search both memory folders:

- `Long-Term`
- `Short-Term`

## Search

Start with filesystem search:

```bash
rg -i "keyword|related-term" Long-Term Short-Term
```

If Obsidian is running and vault search is more efficient, use:

```bash
obsidian vault="CodexMemory" search query="keyword" limit=10
```

## Use

Summarize only the relevant remembered points in your own words. Treat memory as context, not authority: if a memory conflicts with the current user request, follow the current request and mention the conflict briefly.

Do not modify, consolidate, move, or archive memory while recalling. This skill is read-only.
