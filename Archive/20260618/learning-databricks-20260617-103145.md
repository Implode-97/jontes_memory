---
type: short-term-learning
topic: databricks
created: 2026-06-17 10:31:45
source: codex-conversation
confidence: high
---

# Learning: refactor_aula greenfield pass-through app structure

## Context
During implementation in `/Users/jnyjc2/code/innovation-week/refactor_aula`, the user wanted a greenfield Streamlit refactor modeled on `everything_app` user pass-through credentials.

## Learning
The active app path is now the root `app.py` thin entrypoint plus the `aula_app` package. Streamlit pages live in `aula_app/pages`, runtime context in `aula_app/ui.py` and `aula_app/state.py`, and pass-through auth/config/SQL/files/repository helpers in `aula_app/{auth,config,sql,files,repositories,services,models}.py`. The legacy `Pages/` files are wrappers only, while `Utilities/` is excluded from bundle sync and no longer imported by active pages. Tests cover auth fallback, config parsing, user-token SQL connector usage, files errors, image keys, annotation merge behavior, and DAB pass-through config.

## Future Use
Future work should preserve this package path, avoid default `WorkspaceClient()` and token-bearing Streamlit caches, and verify with `uv run pytest`, `uv run ruff check .`, compileall, and `DATABRICKS_BUNDLE_ENGINE=direct databricks bundle validate --strict -t dev --profile DEFAULT`.
