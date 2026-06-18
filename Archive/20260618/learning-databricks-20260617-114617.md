---
type: short-term-learning
topic: databricks
created: 2026-06-17 11:46:17
source: codex-conversation
confidence: high
---

# Learning: refactor_aula verification hardening targets

## Context
During a read-only verification/test audit of `/Users/jnyjc2/code/innovation-week/refactor_aula`, the user wanted tests to protect pass-through-only behavior, selectable warehouses, the `../everything_app` default warehouse/options, and greenfield cleanup after legacy removal.

## Learning
The current repo has only the active `app.py`, `aula_app/`, tests, and config surface; legacy `Pages/`, `Utilities/`, `Resources/`, `Queries/`, `dst_data/`, and sync/app-runner scripts are gone and tested as absent. `uv run pytest -q`, `uv run ruff check .`, and `databricks bundle validate --strict -t dev` pass. Remaining useful hardening is structural config assertions for no app data resources/valueFrom, exact warehouse JSON parity with `../everything_app`, UI selector default/selection behavior, and a regression test for `annotation_repository_from_query_manager`, whose constructor adapter path can drift from `DatabricksAnnotationRepository`.

## Future Use
When continuing this refactor, keep tests readably focused on the pass-through contract. Prefer structured YAML validation if adding `pyyaml` as a dev dependency; otherwise use strict CLI JSON validation plus exact source assertions.
