---
type: short-term-learning
topic: databricks
created: 2026-06-16 14:04:10
source: codex-conversation
confidence: high
---

# Learning: Databricks Apps PoC Skill Review Outcome

## Context
During the Databricks Apps pass-through PoC review in `/Users/jnyjc2/code/innovation-week/everything_app`, the repo-local Databricks skills and current official Databricks docs were used to check implementation and deployment shape.

## Learning
This project intentionally deviates from generic Databricks Apps skill guidance that recommends app resources and cached framework clients. The PoC must not declare SQL warehouse, Unity Catalog, or volume app resources because those would grant permissions to the app service principal. User-token SQL and SDK clients should not be cached with `st.cache_resource` because the forwarded token is per user/session and can rotate.

## Future Use
For future work in this repo, preserve the pass-through-only boundary unless the user explicitly changes the goal to app service-principal access. Keep data retrieval bounded, use explicit forwarded user credentials for SQL/SDK calls, and treat current official Databricks docs as authoritative when vendored skill text is stale.
