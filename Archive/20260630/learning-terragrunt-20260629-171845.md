---
type: short-term-learning
topic: terragrunt
created: 2026-06-29 17:18:45
source: codex-conversation
confidence: high
---

# Learning: one E2E structure gate per feature

## Context
While refining the platform E2E flow, the user wanted the external locations feature to appear as one top-level function call instead of separate assert and lifecycle calls.

## Learning
For `data-platform-terragrunt-catalog` E2E tests, prefer one top-level `run<Feature>AssertStage(...)` call per feature in `platform_e2e_test.go`. The feature stage file may split implementation into private helper functions, but it should keep a single `testStructureRunStage` gate for the feature. This keeps future selective execution clean because test structure stage names are expected to decide whether a feature's checks run.

## Future Use
When adding E2E coverage for a new platform feature, avoid adding several sibling calls or several structure stages for the same feature unless the user explicitly wants independent rerun/skipping behavior.
