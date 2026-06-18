---
type: short-term-learning
topic: databricks
created: 2026-06-17 11:45:13
source: codex-conversation
confidence: high
---

# Learning: refactor_aula legacy cleanup state

## Context
During a read-only legacy cleanup audit of `/Users/jnyjc2/code/innovation-week/refactor_aula`, the intended greenfield surface was root `app.py` plus `aula_app/`, with Databricks Apps user pass-through only.

## Learning
The legacy root paths `Pages/`, `Utilities/`, `Resources/`, `Queries/`, `dst_data/`, `app_runner.py`, `sync_files.py`, and `sync_files_2.py` are no longer active and were absent by the end of the audit. Query text files that the active app still needs now live under `aula_app/queries/`, matching `aula_app/sql.py`'s `QUERY_DIR`. `databricks.yml` no longer excludes those deleted legacy paths, but still excludes `.agents`, `.databricks`, `.venv`, docs, caches, and tests. Tests now include a legacy-removal assertion.

## Future Use
For future cleanup or deployment work, treat those old root paths as removed unless the user restores them intentionally. Keep pass-through-only boundaries: no app resources for SQL warehouses, UC securables, or volumes.
