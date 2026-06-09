---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:28:59
source: codex-conversation
confidence: high
---

# Learning: Workspace assignments real plan smoke is weak

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/tests/real_plan_smoke.tftest.hcl`, the user asked for a Terraform test semantics and CI-discovery lens.

## Learning
The file is visually compact and plan-only, but its "real provider" value is weak: it lives under default `tests/`, configures the real Databricks account provider, uses a made-up workspace ID, mocks the workspace provider, and stubs the `users` group lookup. Its assertions mostly read planned resource/output values already covered by mock tests, so it does not prove live workspace assignment, entitlement application, or the fragile built-in `users` system-group behavior.

## Future Use
For future reviews of this wrapper, prefer keeping ordinary CI coverage mock/provider-plan safe. Move real-provider smoke coverage to an opt-in path or give it actual shared fixture IDs and assertions that prove live-only Databricks behavior.
