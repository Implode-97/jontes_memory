---
scope: repo
repos: [everything_app, refactor_aula]
topics: [streamlit, databricks-apps, user-pass-through, annotations, bundles]
stability: verify-current
last-reviewed: 2026-07-10
---

# Innovation Week Databricks Apps

## everything_app

- Preserve user-pass-through-only access unless the user explicitly changes the security model.
- Do not cache user-token SQL or SDK clients with `st.cache_resource`.
- Use stable widget keys and session-state-backed inputs; selecting a catalog/schema/table/volume clears stale dependent selections.
- Build volume paths as `/Volumes/{catalog}/{schema}/{volume}`.
- Render wide results below compact action-button rows and use full-width dataframes.
- Jira `CAV-133791` records the explicit forwarded-token volume image-loading fix.

## refactor_aula

- Treat root `app.py` and lowercase `aula_app/` as active; removed legacy top-level pages, utilities, resources, queries, data, and runner scripts stay removed.
- Forwarded token handling belongs in `aula_app/auth.py`; SQL and Files API calls use the explicit user token.
- Do not reintroduce default `WorkspaceClient()`, ambient `DATABRICKS_TOKEN`, token-bearing global caches, raw annotation SQL, or app-principal data resources.
- Annotation identity is `(sequence_id, camera_id, attribute_name)` with typed rows and image keys containing sequence and camera.
- Preserve the warehouse selector contract and required `firecrackers_budget_policy_id`; do not invent a default policy ID.
- Keep `bundle.engine: direct` while the Terraform bundle engine remains blocked by the checksum-signature issue.
- Set explicit page `url_path` values when page callables share the name `render`.

## Current State — Verify

- The shared selector used `ATS Warehouse` with ID `30c57a3c1e729b60` in both `databricks.yml` and `app.yaml`; verify before reuse.
- Verify active file layout, accepted user scopes, bundle engine behavior, and Jira state in the repositories before changing them.
