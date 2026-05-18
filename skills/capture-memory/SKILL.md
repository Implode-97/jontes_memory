---
name: capture-memory
description: Automatically write concise Obsidian short-term memory notes when Codex learns reusable user preferences, project context, decisions, implementation patterns, mistakes to avoid, or domain facts from a conversation. Use at the end of substantive sessions or immediately after a durable learning appears, especially for architecture, planning, implementation, Terraform, Terragrunt, Databricks, Codex workflow, or Obsidian memory behavior.
---

# Capture Memory

Write one short-term learning note per durable learning. The goal is future recall, not a transcript. Do not store secrets, credentials, personal sensitive data, private HR details, or transient chatter.

## Location

Resolve the memory root before writing:

- Prefer the current working directory when it contains `Short-Term`, `Long-Term`, `Dreams`, and `Archive`.
- Otherwise use the default memory root:

`/Users/jnyjc2/Documents/Obsidian Vault/CodexMemory/CodexMemory/Memory`

Write notes under `Short-Term` within that memory root.

Use this filename pattern:

`learning-XXX-YYYYMMDD-HHMMSS.md`

`XXX` is a short lowercase topic abbreviation such as `architecture`, `plan`, `implementation`, `terraform`, `terragrunt`, `databricks`, `docs`, `ci`, `jira`, `obsidian`, or `codex`. Use the local timestamp from the environment when possible.

## Note Shape

Always use this exact frontmatter and heading schema. Do not invent alternate frontmatter, omit required fields, or write tags-only notes.

```markdown
---
type: short-term-learning
topic: XXX
created: YYYY-MM-DD HH:MM:SS
source: codex-conversation
confidence: high|medium|low
---

# Learning: short title

## Context
One or two sentences about where this came from.

## Learning
The reusable learning, preference, decision, or pattern.

## Future Use
How future Codex sessions should apply it.
```

## Verbosity

Target 90-180 words. Expand notes below roughly 80 words unless the learning is trivial. Each note should be understandable without the original conversation and should contain enough `Future Use` guidance for a later agent to act correctly.

## Behavior

Capture only if the learning is likely to help future sessions. Prefer separate notes for separate durable learnings. If a learning corrects or supersedes an earlier memory, state that clearly in the title or Context, but do not edit older notes unless explicitly asked.

Prefer direct file writes to the Short-Term folder. Do not perform extra memory lookup in this skill; recall-memory handles retrieval before work begins. Do not include recall search output unless it is part of the actual new learning.
