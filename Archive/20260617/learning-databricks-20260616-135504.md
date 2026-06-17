---
type: short-term-learning
topic: databricks
created: 2026-06-16 13:55:04
source: codex-conversation
confidence: high
---

# Learning: Project-Local Databricks Agent Skills Placement

## Context
During the Databricks Apps PoC in `/Users/jnyjc2/code/innovation-week/everything_app`, the user clarified that they wanted relevant Databricks agent skills copied directly into the project rather than only consulted from `/tmp` or installed globally.

## Learning
For project-local Databricks agent guidance, prefer a focused copy under `.agents/skills/databricks-agent-skills` instead of vendoring the full upstream `databricks-agent-skills` repository. Include the stable upstream `skills/` tree and only relevant experimental skills for the project, such as Apps Python, DBSQL, docs, Python SDK, and Unity Catalog. Omit plugin packaging, hooks, upstream tests, and unrelated experimental examples unless explicitly needed. Exclude `.agents/**` from Databricks Asset Bundle sync and from project linting so it remains agent-only support material.

## Future Use
When a user asks to “copy the Databricks skills into the project,” implement a small `.agents` skill snapshot plus root `AGENTS.md` pointers, then validate that DAB sync excludes it and app checks still pass.
