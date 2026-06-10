---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 14:04:36
source: codex-conversation
confidence: high
---

# Learning: treat example units root as orphan unless consumed

## Context
This came from scoring `data-platform-terragrunt-catalog/example/units/root.hcl` for looks, readability, KISS, YAGNI, maintainability, and safety-aware simplification.

## Learning
In this catalog, `example/units/root.hcl` is not part of the active example topology. Active stack examples live under `example/non-prod-env/...`, and generated unit includes resolve `root.hcl` through `example/non-prod-env/root.hcl`. The `example/units` directory has no stack files or unit examples, so its root provider generation reads as dormant fixture scaffolding. Its fallback region also drifts from the active `eu-central-1` example defaults.

## Future Use
For future Terragrunt catalog reviews, penalize `example/units/root.hcl` as orphan config unless an active test, generated stack, or documented fixture path consumes it. Prefer deleting it, or making a real fixture with explicit consumers and aligned region/provider behavior.
