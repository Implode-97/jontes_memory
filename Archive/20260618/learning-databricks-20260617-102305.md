---
type: short-term-learning
topic: databricks
created: 2026-06-17 10:23:05
source: codex-conversation
confidence: high
---

# Learning: refactor_aula DAB strict excludes

## Context
While refactoring `/Users/jnyjc2/code/innovation-week/refactor_aula` deployment files into a greenfield Databricks App bundle, strict bundle validation was run with Databricks CLI v0.296.0.

## Learning
For this repo and CLI version, `databricks bundle validate --strict -t dev` treats `sync.exclude` entries that match no files as warnings, and strict mode fails on those warnings. Required pass-through excludes such as `.agents/**`, notebook/debug scripts, and `dst_data/**` should remain, but optional cache patterns like `.pytest_cache/**` and `.ruff_cache/**` should not be listed unless they exist.

## Future Use
When editing this repo's DAB config, keep the app pass-through-only with no SQL/UC/volume app resources. Keep sync excludes focused on present files and directories so strict validation stays clean; add optional cache excludes only when the matching paths exist or after confirming the CLI behavior changed.
