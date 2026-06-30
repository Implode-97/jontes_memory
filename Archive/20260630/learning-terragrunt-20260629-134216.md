---
type: short-term-learning
topic: terragrunt
created: 2026-06-29 13:42:16
source: codex-conversation
confidence: high
---

# Learning: External Location Lifecycle E2E Pattern

## Context
During the Databricks external-location feature work, the user asked for the E2E test to exercise the destructive lifecycle path rather than relying only on guard unit tests.

## Learning
For external locations, the active lifecycle sequence to prove in E2E is `enabled` -> failed direct `destroyed` apply -> `destroy_pending` apply -> `destroyed` apply -> row removal apply -> recreate from the baseline YAML. The canonical state name is `destroy_pending`, even if the user says pending-destroy. Direct `enabled` -> `destroyed` must fail with the lifecycle guard message that says to apply `destroy_pending` first.

## Future Use
When extending lifecycle tests for this project, prefer a live E2E proof of the guarded transition plus restoration to the original output shape. Verify outputs after row removal are empty and after recreation match the baseline handoff/state outputs.
