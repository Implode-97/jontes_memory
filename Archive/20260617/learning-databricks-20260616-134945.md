---
type: short-term-learning
topic: databricks
created: 2026-06-16 13:49:45
source: codex-conversation
confidence: high
---

# Learning: Databricks Apps Pass-Through PoC Pattern

## Context
While building `/Users/jnyjc2/code/innovation-week/everything_app`, current Databricks Apps docs and the `databricks-agent-skills` repo were checked for Streamlit, DABs, UV, and user authorization guidance.

## Learning
For a PoC proving user pass-through credentials, do not declare SQL warehouse, Unity Catalog table, volume, or other data resources under the app resource, because those resources grant permissions to the app service principal. Instead, declare only `user_api_scopes` such as `iam.current-user:read`, `sql`, and `files.files`, then explicitly read `x-forwarded-access-token` and pass it to `databricks-sql-connector` or `WorkspaceClient(host=..., token=..., auth_type="pat")`. Current Databricks Apps docs support `pyproject.toml` plus `uv.lock`; older skill guidance saying Python apps only support `requirements.txt` is stale.

## Future Use
For Databricks Apps auth/debugging, trace every SQL/SDK/Files call to its credential source. Use default `WorkspaceClient()` only when intentionally proving app-SP identity, and verify user scopes after deployments because app config updates can replace rather than merge.
