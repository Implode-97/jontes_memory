---
type: short-term-learning
topic: terragrunt
created: 2026-05-15 14:30:23
source: codex-conversation
confidence: high
---

# Learning: verify Terratest against latest upstream source and CI Go version

## Context
While implementing the Terragrunt catalog repo E2E test, the user flagged that the code was still using deprecated Terratest stack helpers and asked to verify against the latest upstream `modules/terragrunt` source instead of relying on older docs.

## Learning
For Terratest Terragrunt work, the user expects verification against the latest upstream repo source when deprecations or best-practice drift are in question. Terratest `v1.0.0` uses `StackGenerateContext`, `StackRunContext`, and `StackCleanContext`, and upgrading to it also raises the Go toolchain requirement to `1.26`, which must be accounted for in CI.

## Future Use
When updating Terratest-based E2E code, inspect the current upstream source first, prefer context-based helpers over deprecated wrappers, and check CI toolchain compatibility before treating the upgrade as complete.
