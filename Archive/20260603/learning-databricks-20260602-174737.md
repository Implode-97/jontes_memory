---
type: short-term-learning
topic: databricks
created: 2026-06-02 17:47:37
source: codex-conversation
confidence: high
---

# Learning: Databricks Apps Identity Split

## Context
A Databricks Streamlit app investigation showed a common auth split between app service-principal calls and forwarded user-token calls when reading Unity Catalog tables and volume files.

## Learning
In Databricks Apps, default SDK clients such as `WorkspaceClient()` use the app service principal from injected environment variables, while user pass-through requires explicitly using the forwarded `x-forwarded-access-token`. SQL path discovery and volume file reads can accidentally run under different principals if helper methods mix SDK default auth, direct REST calls with a user token, and silent fallback to SDK auth.

## Future Use
When debugging Databricks Apps data-access failures, trace each API call to the exact credential source. Check `WorkspaceClient()` usage, SQL Statements calls, Files API calls, declared user scopes, app resource grants, and Streamlit caches that store per-user tokens globally. Recommend fail-fast diagnostics over silent SP fallback when user authorization is intended.
