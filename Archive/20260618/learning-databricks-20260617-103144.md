---
type: short-term-learning
topic: databricks
created: 2026-06-17 10:31:44
source: codex-conversation
confidence: high
---

# Learning: refactor_aula pass-through data boundary implemented

## Context
During the greenfield Databricks Streamlit refactor in `/Users/jnyjc2/code/innovation-week/refactor_aula`, the user owned the pass-through auth and SQL/data-access library slice while other agents were concurrently changing the app shell, DAB files, and pages.

## Learning
The active package now centralizes forwarded `x-forwarded-access-token` handling in `aula_app/auth.py`, fails closed in Databricks Apps when the token is absent, and allows only `DATABRICKS_USER_ACCESS_TOKEN` as a local fallback. `aula_app/sql.py` uses the Databricks SQL connector with `access_token=user.access_token`; `aula_app/repositories.py` validates/quotes Unity Catalog table identifiers and writes annotations through parameterized merge statements keyed by `sequence_id`, `camera_id`, and `attribute_name`. Tests cover auth fallback, config parsing, SQL connector token use, no default app-SP client regression, files access, image keys, and annotation validation.

## Future Use
For future work, keep data access behind these modules. Do not reintroduce `WorkspaceClient()` default auth, `DATABRICKS_TOKEN`, raw annotation SQL literals, or app service-principal data resources unless the user explicitly changes the security model.
