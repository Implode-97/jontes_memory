---
type: short-term-learning
topic: databricks
created: 2026-06-17 09:57:09
source: codex-conversation
confidence: high
---

# Learning: refactor_aula Pass-Through Review

## Context
During a read-only security review of `/Users/jnyjc2/code/innovation-week/refactor_aula`, the user wanted it compared against the `/Users/jnyjc2/code/innovation-week/everything_app` pass-through-only Databricks App PoC.

## Learning
`refactor_aula` currently mixes forwarded user tokens with app service-principal access. Its Streamlit pages cache token-bearing managers, mutate cached `user_token` fields on rerun, use default `WorkspaceClient()` for SQL Statements and Files fallback, grant SQL warehouse, table, and volume resources to the app service principal in `databricks.yml`, and expose token metadata in UI errors. This conflicts with the intended pass-through-only boundary.

## Future Use
For future work on this app, refactor toward the `everything_app` pattern: extract `x-forwarded-access-token` per request/session, build user-scoped SQL and SDK clients explicitly, remove app data resources, avoid caching token-scoped clients/results, and fail closed when the forwarded token or user scopes are missing.
