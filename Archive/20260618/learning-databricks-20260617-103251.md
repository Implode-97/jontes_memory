---
type: short-term-learning
topic: databricks
created: 2026-06-17 10:32:51
source: codex-conversation
confidence: high
---

# Learning: refactor_aula Greenfield Pass-Through Refactor

## Context
During implementation in `/Users/jnyjc2/code/innovation-week/refactor_aula`, the user asked for a greenfield Databricks App refactor guided by `/Users/jnyjc2/code/innovation-week/everything_app` and the no-app-service-principal data-access proof.

## Learning
The active app path now uses `app.py` plus the lowercase `aula_app/` package. The intended pattern is pass-through only: forwarded `x-forwarded-access-token` is centralized in `aula_app/auth.py`, SQL and Files access require explicit user credentials, annotation writes are parameterized and keyed by `sequence_id`, `camera_id`, and `attribute_name`, and the greenfield DAB excludes legacy `Pages/**`, `Utilities/**`, `.agents/**`, notebook/debug scripts, and tests from bundle sync. The warehouse ID is an explicit deployment variable/placeholder, not inherited from old app config.

## Future Use
For future `refactor_aula` work, edit the active `aula_app/` package and tests first. Treat old `Pages/` and `Utilities/` as legacy/reference unless the user explicitly asks to delete or migrate them. Do not add app `resources` for SQL warehouses, UC securables, or volumes unless the security model changes.
